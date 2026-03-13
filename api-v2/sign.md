# L2 Signature Guide

This document explains how to create Layer 2 (L2) signatures for operations on the EdgeX platform, including orders, withdrawals, and transfers. EdgeX V2 uses **EIP-712 typed data signing** for all L2 operations.

---

## Overview

### What is L2 Signature?

Layer 2 signature is a cryptographic signature that authorizes operations on EdgeX's Layer 2 scaling solution. These signatures:

- **Authorize trading operations** (orders, cancellations)
- **Approve asset movements** (withdrawals, transfers)
- **Ensure security** without requiring on-chain gas fees for every action
- **Enable fast execution** while maintaining cryptographic security

### Signature Algorithm: EIP-712

EdgeX V2 uses **EIP-712** (Ethereum Improvement Proposal 712) for structured typed data hashing and signing. EIP-712 provides:

- **Human-readable** structured data
- **Type-safe** signing
- **Domain separation** to prevent signature reuse across different contracts
- **Standard** supported by Ethereum wallets like MetaMask

> **Note**: This is different from V1 which used StarkEx's Pedersen hash-based signatures. V2 simplifies integration by using standard Ethereum signing.

---

## How To Get Your Signer Key

To sign L2 messages, you need your **Signer Key** (Private Key).

### Obtaining Your Signer Key

The Signer Key is generated in the same SDK Signer dialog as API credentials.

**Steps:**
1. Log in to EdgeX platform
2. Click your profile icon (top-right) → **API Management**
3. Open **Perps V2** tab
4. In the **SDK Signer** column, click **Detail**
5. Copy **Private Key** from the dialog (this is your Signer Key)

You can also copy API Key / Secret / Passphrase in the same dialog. For HTTP authentication usage, see [Authentication Guide](authentication.md).

<figure><img src="../../.gitbook/assets/get-l2-private-key.png" alt=""><figcaption><p>SDK Signer dialog showing Private Key (Signer Key) and API credentials</p></figcaption></figure>

**⚠️ Security Warning**: 
- Keep your Signer Key secure and never share it
- Anyone with access to this key can sign trades and withdrawals on your behalf
- This key is separate from your API Secret (used for HTTP authentication)
- Store it encrypted and never commit to version control
- Binding your L2 Key to third-party platforms may pose security risks

---

## EIP-712 Signature Structure

### Domain Separator

The domain separator ensures signatures are only valid for EdgeX platform:

```go
Domain {
    name:              "EdgeX"
    version:           "1"
    chainId:           "your-chain-id"        // From metadata.global.nativeChainId
    verifyingContract: "contract-address"     // From metadata.global.contractAddress
}
```

**How to get domain values**:

```go
// Get metadata first
metadata, err := client.GetMetaData(ctx)

chainID := metadata.Data.Global.NativeChainId
if chainID == "" {
    chainID = metadata.Data.Global.ChainId
}
verifyingContract := metadata.Data.Global.ContractAddress
```

### Common Type Definitions

All L2 signatures use the `OrderBase` struct:

```go
OrderBase {
    nonce:               uint256    // Unique identifier derived from clientOrderId
    signer:              address    // Your trading address
    accountId:           uint64     // Your EdgeX account ID
    expirationTimestamp: uint256    // When the signature expires (seconds)
}
```

---

## Order Signature (Limit Orders)

### EIP-712 Type Definition

```go
Types: {
    EIP712Domain: [
        { name: "name",              type: "string" },
        { name: "version",           type: "string" },
        { name: "chainId",           type: "uint256" },
        { name: "verifyingContract", type: "address" }
    ],
    OrderBase: [
        { name: "nonce",               type: "uint256" },
        { name: "signer",              type: "address" },
        { name: "accountId",           type: "uint64" },
        { name: "expirationTimestamp", type: "uint256" }
    ],
    LimitOrderParams: [
        { name: "base",              type: "OrderBase" },
        { name: "amountSynthetic",   type: "int256" },
        { name: "amountCollateral",  type: "int256" },
        { name: "amountFee",         type: "uint256" },
        { name: "assetIdSynthetic",  type: "uint64" },
        { name: "assetIdCollateral", type: "uint64" },
        { name: "isBuyingSynthetic", type: "bool" },
        { name: "parentOrderHash",   type: "bytes32" },
        { name: "subOrderIndex",     type: "uint256" }
    ]
}
```

### Parameters Calculation

#### 1. Amount Conversions

All amounts must be converted using their respective resolutions:

```go
// Get resolution values from metadata
contract := metadata.ContractList[...]
quoteCoin := metadata.CoinList[...]

contractResolution := parseResolution(contract.Resolution, contract.StarkExResolution)
quoteResolution := parseResolution(quoteCoin.Resolution, quoteCoin.StarkExResolution)

// Calculate amounts
amountSynthetic  = size × contractResolution
amountCollateral = (price × size) × quoteResolution
amountFee        = (price × size × feeRate) × quoteResolution
```

**Example**:
```go
// BTC-USDT perpetual
// size = 0.001 BTC
// price = 97463.4 USDT
// contractResolution = 10^10 (from contract.starkExResolution)
// quoteResolution = 10^10 (from quoteCoin.starkExResolution)

amountSynthetic  = 0.001 × 10^10 = 10000000
amountCollateral = (97463.4 × 0.001) × 10^10 = 974634000000
amountFee        = (97463.4 × 0.001 × 0.0005) × 10^10 = 487317000 (rounded up)
```

#### 2. Nonce Calculation

Nonce is deterministically derived from `clientOrderId`:

```go
import (
    "crypto/sha256"
    "math/big"
)

func CalcNonce(clientOrderID string) int64 {
    hash := sha256.Sum256([]byte(clientOrderID))
    hashBigInt := new(big.Int).SetBytes(hash[:])
    
    // Modulo 2^63 to fit in positive int64
    maxInt64 := new(big.Int).Lsh(big.NewInt(1), 63)
    nonce := new(big.Int).Mod(hashBigInt, maxInt64)
    
    return nonce.Int64()
}
```

#### 3. Expiration Timestamp

```go
nowMillis := time.Now().UnixMilli()

// Order expires in 30 days
orderExpireWindowMillis := int64(30 * 24 * 60 * 60 * 1000)
l2ExpireTime := nowMillis + orderExpireWindowMillis

// L1 offset (8 days)
l1OffsetMillis := int64(8 * 24 * 60 * 60 * 1000)
expireTime := l2ExpireTime - l1OffsetMillis

// Convert to seconds for signature
expirationTimestamp := expireTime / 1000
```

#### 4. isBuyingSynthetic Flag

```go
// For BUY orders (going long)
isBuyingSynthetic := true   // Buying BTC synthetic with USDT collateral

// For SELL orders (going short)
isBuyingSynthetic := false  // Selling BTC synthetic for USDT collateral
```

### Complete Golang Example

```go
package main

import (
    "context"
    "fmt"
    "strconv"
    "time"
    
    "github.com/shopspring/decimal"
    "github.com/edgex-Tech/edgex-golang-sdk/sdk/internal"
)

// SignLimitOrder creates an EIP-712 signature for a limit order
func SignLimitOrder(
    signerPrivateKey string,
    accountID int64,
    signerAddress string,
    contract Contract,
    quoteCoin Coin,
    metadata Global,
    side string,        // "BUY" or "SELL"
    price decimal.Decimal,
    size decimal.Decimal,
    clientOrderID string,
) (string, error) {
    
    // 1. Calculate amounts with resolutions
    contractResolution, _ := decimal.NewFromString(contract.StarkExResolution)
    quoteResolution, _ := decimal.NewFromString(quoteCoin.StarkExResolution)
    
    amountSynthetic := size.Mul(contractResolution).BigInt()
    amountCollateral := price.Mul(size).Mul(quoteResolution).BigInt()
    
    // Calculate fee (assuming 0.05% taker fee)
    feeRate := decimal.NewFromFloat(0.0005)
    limitFee := price.Mul(size).Mul(feeRate).Ceil()
    amountFee := limitFee.Mul(quoteResolution).BigInt()
    
    // 2. Calculate nonce from client order ID
    nonce := CalcNonce(clientOrderID)
    
    // 3. Calculate expiration
    nowMillis := time.Now().UnixMilli()
    orderExpireWindowMillis := int64(30 * 24 * 60 * 60 * 1000) // 30 days
    l1OffsetMillis := int64(8 * 24 * 60 * 60 * 1000)          // 8 days
    l2ExpireTime := nowMillis + orderExpireWindowMillis
    expireTime := l2ExpireTime - l1OffsetMillis
    expirationTimestamp := expireTime / 1000 // Convert to seconds
    
    // 4. Build EIP-712 domain
    typedDomain, err := internal.NewTypedDataDomain(
        "EdgeX",
        "1",
        metadata.NativeChainId,
        metadata.ContractAddress,
    )
    if err != nil {
        return "", err
    }
    
    // 5. Build typed data structure
    typedData := internal.TypedData{
        Types: internal.TypedDataTypes{
            "EIP712Domain": []internal.TypedDataType{
                {Name: "name", Type: "string"},
                {Name: "version", Type: "string"},
                {Name: "chainId", Type: "uint256"},
                {Name: "verifyingContract", Type: "address"},
            },
            "OrderBase": []internal.TypedDataType{
                {Name: "nonce", Type: "uint256"},
                {Name: "signer", Type: "address"},
                {Name: "accountId", Type: "uint64"},
                {Name: "expirationTimestamp", Type: "uint256"},
            },
            "LimitOrderParams": []internal.TypedDataType{
                {Name: "base", Type: "OrderBase"},
                {Name: "amountSynthetic", Type: "int256"},
                {Name: "amountCollateral", Type: "int256"},
                {Name: "amountFee", Type: "uint256"},
                {Name: "assetIdSynthetic", Type: "uint64"},
                {Name: "assetIdCollateral", Type: "uint64"},
                {Name: "isBuyingSynthetic", Type: "bool"},
                {Name: "parentOrderHash", Type: "bytes32"},
                {Name: "subOrderIndex", Type: "uint256"},
            },
        },
        PrimaryType: "LimitOrderParams",
        Domain:      typedDomain,
        Message: internal.TypedDataMessage{
            "base": map[string]interface{}{
                "nonce":               strconv.FormatInt(nonce, 10),
                "signer":              signerAddress,
                "accountId":           strconv.FormatInt(accountID, 10),
                "expirationTimestamp": strconv.FormatInt(expirationTimestamp, 10),
            },
            "amountSynthetic":   amountSynthetic.String(),
            "amountCollateral":  amountCollateral.String(),
            "amountFee":         amountFee.String(),
            "assetIdSynthetic":  contract.ContractId,
            "assetIdCollateral": quoteCoin.CoinId,
            "isBuyingSynthetic": side == "BUY",
            "parentOrderHash":   "0x0000000000000000000000000000000000000000000000000000000000000000",
            "subOrderIndex":     "0",
        },
    }
    
    // 6. Sign the typed data
    signature, err := internal.SignTypedDataWithPrivateKey(signerPrivateKey, typedData)
    if err != nil {
        return "", fmt.Errorf("failed to sign typed data: %w", err)
    }
    
    return signature, nil
}
```

### Using EdgeX Golang SDK

The SDK handles all signature generation automatically:

```go
package main

import (
    "context"
    "log"
    
    "github.com/edgex-Tech/edgex-golang-sdk/sdk"
    "github.com/edgex-Tech/edgex-golang-sdk/sdk/order"
    "github.com/shopspring/decimal"
)

func main() {
    // Create client with trading private key
    client, err := sdk.NewClient(&sdk.Config{
        BaseURL:       "https://edgex-testnet-internal-v2.edgex.exchange",
        AccountID:     724625476626153743,
        APIKey:        "your-api-key",
        APIPassphrase: "your-api-passphrase",
        APISecret:     "your-api-secret",
        SignerPriKey:  "your-signer-private-key",  // Signer key for L2 signatures
    })
    if err != nil {
        log.Fatal(err)
    }
    
    ctx := context.Background()
    
    // SDK automatically signs the order with EIP-712
    result, err := client.CreateOrder(ctx, &order.CreateOrderParams{
        ContractId: "10000001",
        Side:       order.OrderSideBuy,
        Type:       order.OrderTypeLimit,
        Price:      "97463.4",
        Size:       "0.001",
    }, nil, decimal.NewFromFloat(97463.4))
    
    if err != nil {
        log.Fatal(err)
    }
    
    log.Printf("Order created: %s", result.Data.OrderId)
}
```

---

## Withdrawal Signature

### EIP-712 Type Definition

```go
Types: {
    EIP712Domain: [...],  // Same as order
    OrderBase: [...],     // Same as order
    WithdrawParams: [
        { name: "base",              type: "OrderBase" },
        { name: "amount",            type: "uint256" },
        { name: "assetIdCollateral", type: "uint64" },
        { name: "ethAddress",        type: "address" }
    ]
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `amount` | uint256 | Withdrawal amount in base units (amount × resolution) |
| `assetIdCollateral` | uint64 | Coin ID from `metadata.coinList.coinId` |
| `ethAddress` | address | Destination Ethereum address (0x-prefixed) |

### Golang Example

```go
func SignWithdrawal(
    signerPrivateKey string,
    accountID int64,
    signerAddress string,
    coin Coin,
    metadata Global,
    ethAddress string,
    amount decimal.Decimal,
    clientOrderID string,
) (string, error) {
    
    // Calculate amount with resolution
    resolution, _ := decimal.NewFromString(coin.StarkExResolution)
    amountScaled := amount.Mul(resolution).BigInt()
    
    // Calculate nonce and expiration
    nonce := CalcNonce(clientOrderID)
    expirationTimestamp := time.Now().Unix() + (30 * 24 * 60 * 60) // 30 days
    
    // Build domain
    typedDomain, _ := internal.NewTypedDataDomain(
        "EdgeX", "1",
        metadata.NativeChainId,
        metadata.ContractAddress,
    )
    
    // Build typed data
    typedData := internal.TypedData{
        Types: internal.TypedDataTypes{
            "EIP712Domain": [...],
            "OrderBase":    [...],
            "WithdrawParams": []internal.TypedDataType{
                {Name: "base", Type: "OrderBase"},
                {Name: "amount", Type: "uint256"},
                {Name: "assetIdCollateral", Type: "uint64"},
                {Name: "ethAddress", Type: "address"},
            },
        },
        PrimaryType: "WithdrawParams",
        Domain:      typedDomain,
        Message: internal.TypedDataMessage{
            "base": map[string]interface{}{
                "nonce":               strconv.FormatInt(nonce, 10),
                "signer":              signerAddress,
                "accountId":           strconv.FormatInt(accountID, 10),
                "expirationTimestamp": strconv.FormatInt(expirationTimestamp, 10),
            },
            "amount":            amountScaled.String(),
            "assetIdCollateral": coin.CoinId,
            "ethAddress":        ethAddress,
        },
    }
    
    return internal.SignTypedDataWithPrivateKey(signerPrivateKey, typedData)
}
```

---

## Transfer Signature

### EIP-712 Type Definition

```go
Types: {
    EIP712Domain: [...],  // Same as order
    OrderBase: [...],     // Same as order
    TransferParams: [
        { name: "base",                type: "OrderBase" },
        { name: "amount",              type: "uint256" },
        { name: "assetIdCollateral",   type: "uint64" },
        { name: "receiverAccountId",   type: "uint64" },
        { name: "receiverPublicKey",   type: "address" }
    ]
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `amount` | uint256 | Transfer amount in base units |
| `assetIdCollateral` | uint64 | Coin ID being transferred |
| `receiverAccountId` | uint64 | Recipient's EdgeX account ID |
| `receiverPublicKey` | address | Recipient's signer address |

### Golang Example

```go
func SignTransfer(
    signerPrivateKey string,
    accountID int64,
    signerAddress string,
    coin Coin,
    metadata Global,
    receiverAccountID int64,
    receiverPublicKey string,
    amount decimal.Decimal,
    clientOrderID string,
) (string, error) {
    
    // Calculate amount with resolution
    resolution, _ := decimal.NewFromString(coin.StarkExResolution)
    amountScaled := amount.Mul(resolution).BigInt()
    
    // Calculate nonce and expiration
    nonce := CalcNonce(clientOrderID)
    expirationTimestamp := time.Now().Unix() + (30 * 24 * 60 * 60)
    
    // Build domain
    typedDomain, _ := internal.NewTypedDataDomain(
        "EdgeX", "1",
        metadata.NativeChainId,
        metadata.ContractAddress,
    )
    
    // Build typed data
    typedData := internal.TypedData{
        Types: internal.TypedDataTypes{
            "EIP712Domain": [...],
            "OrderBase":    [...],
            "TransferParams": []internal.TypedDataType{
                {Name: "base", Type: "OrderBase"},
                {Name: "amount", Type: "uint256"},
                {Name: "assetIdCollateral", Type: "uint64"},
                {Name: "receiverAccountId", Type: "uint64"},
                {Name: "receiverPublicKey", Type: "address"},
            },
        },
        PrimaryType: "TransferParams",
        Domain:      typedDomain,
        Message: internal.TypedDataMessage{
            "base": map[string]interface{}{
                "nonce":               strconv.FormatInt(nonce, 10),
                "signer":              signerAddress,
                "accountId":           strconv.FormatInt(accountID, 10),
                "expirationTimestamp": strconv.FormatInt(expirationTimestamp, 10),
            },
            "amount":              amountScaled.String(),
            "assetIdCollateral":   coin.CoinId,
            "receiverAccountId":   strconv.FormatInt(receiverAccountID, 10),
            "receiverPublicKey":   receiverPublicKey,
        },
    }
    
    return internal.SignTypedDataWithPrivateKey(signerPrivateKey, typedData)
}
```

---

## Helper Functions

### Derive Signer Address from Private Key

```go
import (
    "github.com/edgex-Tech/edgex-golang-sdk/sdk/internal"
)

func DeriveAddress(privateKeyHex string) (string, error) {
    return internal.DeriveAddressFromPrivateKey(privateKeyHex)
}

// Example usage
signerAddress, err := DeriveAddress("0x1234...")
// Returns: "0xAbC123..."
```

### Parse Resolution

```go
import "github.com/shopspring/decimal"

func ParseResolution(resolutionStr, starkExResolutionStr string) (decimal.Decimal, error) {
    // Priority: use starkExResolution if available
    if starkExResolutionStr != "" && starkExResolutionStr != "0" {
        return decimal.NewFromString(starkExResolutionStr)
    }
    
    // Fallback to resolution
    resolution, err := decimal.NewFromString(resolutionStr)
    if err != nil {
        return decimal.Zero, err
    }
    
    // Convert 10^N format to actual number
    // e.g., "10" means 10^10 = 10000000000
    exp := resolution.IntPart()
    return decimal.NewFromInt(10).Pow(decimal.NewFromInt(exp)), nil
}
```

---

## Signature Verification

### How EdgeX Verifies Signatures

1. **Reconstruct typed data** from request parameters
2. **Hash the typed data** using EIP-712 hashing algorithm
3. **Recover signer address** from signature
4. **Verify** recovered address matches the registered trading address for the account

### Manual Verification (Optional)

```go
import (
    "github.com/ethereum/go-ethereum/signer/core/apitypes"
    "github.com/ethereum/go-ethereum/crypto"
)

func VerifySignature(typedData apitypes.TypedData, signature string, expectedAddress string) bool {
    // Hash the typed data
    digest, _, err := apitypes.TypedDataAndHash(typedData)
    if err != nil {
        return false
    }
    
    // Decode signature (remove 0x prefix, decode hex)
    sigBytes, err := hex.DecodeString(signature[2:])
    if err != nil {
        return false
    }
    
    // Adjust v value (EIP-712 uses 27/28, ecrecover uses 0/1)
    if sigBytes[64] >= 27 {
        sigBytes[64] -= 27
    }
    
    // Recover public key
    pubKey, err := crypto.SigToPub(digest, sigBytes)
    if err != nil {
        return false
    }
    
    // Derive address and compare
    recoveredAddress := crypto.PubkeyToAddress(*pubKey).Hex()
    return strings.EqualFold(recoveredAddress, expectedAddress)
}
```

---

## Common Errors

### Error: Invalid L2 Signature

**Causes**:
1. Wrong Signer Key
2. Incorrect amount calculation (resolution not applied)
3. Wrong nonce calculation
4. Expired timestamp
5. Wrong domain parameters (chainId or contractAddress)

**Solutions**:
1. Verify Signer Key matches registered key
2. Check resolution is correctly applied to amounts
3. Ensure nonce is calculated from clientOrderId using SHA-256
4. Use current timestamp, not expired
5. Get domain from latest metadata

### Error: Signer Address Mismatch

**Cause**: The address derived from Signer Key doesn't match registered address

**Solution**:
```go
// Verify your key derives to correct address
derivedAddress, _ := internal.DeriveAddressFromPrivateKey(yourSignerKey)
fmt.Printf("Your signer address: %s\n", derivedAddress)

// This should match the address you registered on EdgeX platform
```

### Error: Amount Overflow

**Cause**: Amount × resolution exceeds maximum value

**Solution**: Use big.Int for all calculations:
```go
import "math/big"

amountBigInt := new(big.Int)
// Perform calculations using big.Int methods
```

---

## Migration from V1

### Key Differences: V1 vs V2

| Aspect | V1 | V2 |
|--------|----|----|
| **Algorithm** | Pedersen Hash + ECDSA | EIP-712 Typed Data |
| **Curve** | Custom StarkEx curve | Standard secp256k1 |
| **Signature Format** | r, s, y | Standard Ethereum signature (0x...) |
| **Wallet Support** | Custom implementation | MetaMask compatible |
| **Hash Function** | Pedersen hash | Keccak256 (EIP-712) |

### Migration Checklist

- [ ] Replace StarkEx signature code with EIP-712 implementation
- [ ] Update signature format from `{r, s}` to `0x...` hex string
- [ ] Use standard Ethereum private keys (not StarkEx keys)
- [ ] Update amount calculations (check if resolutions changed)
- [ ] Test with V2 testnet before production

---

## Security Best Practices

1. **Protect Your Signer Key**
   - Store encrypted (e.g., AWS KMS, HashiCorp Vault)
   - Never log or expose in error messages
   - Rotate periodically

2. **Validate All Inputs**
   - Check amounts are within valid ranges
   - Verify addresses are valid Ethereum addresses
   - Ensure nonce is unique

3. **Use Official SDK**
   - Let SDK handle signature generation
   - Reduces risk of implementation bugs
   - Automatically stays updated

4. **Monitor Signature Usage**
   - Track orders signed by your key
   - Alert on unusual patterns
   - Implement rate limiting

---

## Additional Resources

- **EIP-712 Specification**: https://eips.ethereum.org/EIPS/eip-712
- **Go-Ethereum EIP-712 Implementation**: https://pkg.go.dev/github.com/ethereum/go-ethereum/signer/core/apitypes
- **EdgeX Golang SDK**: https://github.com/edgex-Tech/edgex-golang-sdk
- **EdgeX API Documentation**: https://docs.edgex.exchange

---

**Document Version**: V2.0  
**Last Updated**: 2026-03-12  
**Based On**: EdgeX Golang SDK V2 Implementation (EIP-712)
