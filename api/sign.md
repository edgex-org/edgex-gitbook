# How To Sign Message

## Overview

In Layer 2 trading systems, to ensure transaction security and prevent unauthorized operations, critical operations (such as limit orders, transfers, withdrawals, etc.) require users to provide digital signatures to prove the legitimacy of the operations. This signature is passed through the `l2Signature` parameter.

## How To GET Your L2 Private Key

To generate l2Signature, you first need to obtain your L2 private key. This key is used to generate signatures that authorize various actions on the platform.

<figure><img src="../../.gitbook/assets/20250102-134437.png" alt=""><figcaption><p><strong>How To GET Your L2 Private Key</strong></p></figcaption></figure>

> **⚠️ Security Warning:**
> - Keep your private key secure and never share it with anyone
> - Anyone with access to your private key can sign messages and execute operations on your behalf

## Signature Algorithm

### Two algorithms are used in the signature process:

1. **ECDSA** (Elliptic Curve Digital Signature Algorithm) is used to generate and verify signatures.
2. **Pedersen Hash** which consumes more CPU resources compared to regular Ethereum signatures, is used for message hashing.

L2Signature generation follows these standard steps:

1. **Collect Parameters**: Gather all required parameters for the operation
2. **Calculate Hash**: Use Pedersen Hash to compute the message hash
3. **Generate Signature**: Use ECDSA algorithm and L2 private key to sign the hash
4. **Format Output**: Format the signature according to API requirements

> [Python L2Signature Demo](https://github.com/edgex-Tech/edgex-python-sdk/blob/main/edgex_sdk/internal/client.py#L111)

> [Golang L2Signature Demo](https://github.com/edgex-Tech/edgex-golang-sdk/blob/main/sdk/internal/utils.go#L30)

> [Java Script L2Signature Demo](https://www.npmjs.com/package/@starkware-industries/starkware-crypto-utils#signing-a-starkex-order)

### Java L2Signature Example

Below is a Java implementation of the Ecdsa signature algorithm. This example demonstrates how to sign a message using a private key.

``` java

    public static CreateOrderRequest signOrder(
            CreateOrderRequest request,
            Contract contract,
            Coin quotelCoin,
            PrivateKey privateKey) {
        BigInteger msgHash = L2SignUtil.hashLimitOrder(
                request.getSide() == OrderSide.BUY,
                BigIntUtil.toBigInt(quotelCoin.getStarkExAssetId()),
                BigIntUtil.toBigInt(contract.getStarkExSyntheticAssetId()),
                BigIntUtil.toBigInt(quotelCoin.getStarkExAssetId()),
                UnsignedLong.valueOf(new BigDecimal(request.getL2Value())
                        .multiply(new BigDecimal(BigIntUtil.toBigInt(quotelCoin.getStarkExResolution())))
                        .toBigIntegerExact()),
                UnsignedLong.valueOf(new BigDecimal(request.getL2Size())
                        .multiply(new BigDecimal(BigIntUtil.toBigInt(contract.getStarkExResolution())))
                        .toBigIntegerExact()),
                UnsignedLong.valueOf(new BigDecimal(request.getL2LimitFee())
                        .multiply(new BigDecimal(BigIntUtil.toBigInt(quotelCoin.getStarkExResolution())))
                        .toBigIntegerExact()),
                UnsignedLong.fromLongBits(request.getAccountId()),
                UnsignedInteger.valueOf(request.getL2Nonce()),
                UnsignedInteger.valueOf(request.getL2ExpireTime() / (60 * 60 * 1000L)));
        Signature signature = Ecdsa.sign(msgHash, privateKey);
        return request.toBuilder()
                .setL2Signature(L2Signature.newBuilder()
                        .setR(BigIntUtil.toHexStr(signature.r))
                        .setS(BigIntUtil.toHexStr(signature.s))
                        .build())
                .build();
    }

    public static BigInteger hashLimitOrder(
            boolean isBuyingSynthetic,
            BigInteger assetIdCollateral,
            BigInteger assetIdSynthetic,
            BigInteger assetIdFee,
            UnsignedLong amountCollateral,
            UnsignedLong amountSynthetic,
            UnsignedLong maxAmountFee,
            UnsignedLong positionId,
            UnsignedInteger nonce,
            UnsignedInteger expirationTimestamp) {
        BigInteger assetIdSell;
        BigInteger assetIdBuy;
        UnsignedLong amountSell;
        UnsignedLong amountBuy;
        if (isBuyingSynthetic) {
            assetIdSell = assetIdCollateral;
            assetIdBuy = assetIdSynthetic;
            amountSell = amountCollateral;
            amountBuy = amountSynthetic;
        } else {
            assetIdSell = assetIdSynthetic;
            assetIdBuy = assetIdCollateral;
            amountSell = amountSynthetic;
            amountBuy = amountCollateral;
        }
        BigInteger packedMessage0 = amountSell.bigIntegerValue();
        packedMessage0 = packedMessage0.shiftLeft(64).add(amountBuy.bigIntegerValue());
        packedMessage0 = packedMessage0.shiftLeft(64).add(maxAmountFee.bigIntegerValue());
        packedMessage0 = packedMessage0.shiftLeft(32).add(nonce.bigIntegerValue());

        BigInteger packedMessage1 = BigInteger.valueOf(3);
        packedMessage1 = packedMessage1.shiftLeft(64).add(positionId.bigIntegerValue());
        packedMessage1 = packedMessage1.shiftLeft(64).add(positionId.bigIntegerValue());
        packedMessage1 = packedMessage1.shiftLeft(64).add(positionId.bigIntegerValue());
        packedMessage1 = packedMessage1.shiftLeft(32).add(expirationTimestamp.bigIntegerValue());
        packedMessage1 = packedMessage1.shiftLeft(17);

        BigInteger msg = pedersenHash(assetIdSell, assetIdBuy);
        msg = pedersenHash(msg, assetIdFee);
        msg = pedersenHash(msg, packedMessage0);
        msg = pedersenHash(msg, packedMessage1);
        return msg;
    }

    public static BigInteger pedersenHash(BigInteger... input) {
        BigInteger[][] points = PEDERSEN_POINTS;
        Point shiftPoint = new Point(points[0][0], points[0][1]);
        for (int i = 0; i < input.length; i++) {
            BigInteger x = input[i];
            for (int j = 0; j < 252; j++) {
                int pos = 2 + i * 252 + j;
                Point pt = new Point(points[pos][0], points[pos][1]);
                if (x.and(BigInteger.ONE).intValue() != 0) {
                    shiftPoint = EcMath.add(shiftPoint, pt, Curve.secp256k1.A, Curve.secp256k1.P);
                }
                x = x.shiftRight(1);
            }
        }
        return shiftPoint.x;
    }

    public static Signature sign(BigInteger msgHash, PrivateKey privateKey) {
        Curve curve = privateKey.curve;
        BigInteger randNum = new BigInteger(curve.N.toByteArray().length * 8 - 1, new SecureRandom()).abs().add(BigInteger.ONE);
        Point randomSignPoint = EcMath.multiply(curve.G, randNum, curve.N, curve.A, curve.P);
        BigInteger r = randomSignPoint.x.mod(curve.N);
        BigInteger s = ((msgHash.add(r.multiply(privateKey.secret))).multiply(EcMath.inv(randNum, curve.N))).mod(curve.N);
        return Signature.create(r, s);
    }

```

# Signature Construction Guide

This section provides detailed instructions for constructing signatures for various actions on the platform. Each operation has specific message formats and parameter requirements.

## Withdrawal Signature
Used to authorize withdrawing assets from Layer 2 to an Ethereum address.

### Parameters
- `assetIdCollateral` - Asset ID for the collateral token from [`meta_data.coinList.starkExAssetId`](public-api/metadata-api.md)
- `positionId` - User's account ID in Layer 2 
- `ethAddress` - Destination Ethereum address for withdrawal
- `nonce` - Unique transaction identifier to prevent replay attacks
- `expirationTimestamp` - Unix timestamp when signature expires
- `amount` - Amount to withdraw in base units

### Calculation
The following TypeScript function constructs the withdrawal message for signing:

```typescript
// Construct withdrawal message for signing
function getWithdrawalToAddressMsg({
  assetIdCollateral,
  positionId,
  ethAddress, 
  nonce,
  expirationTimestamp,
  amount
}) {
  // Pack parameters into 256-bit words
  const w1 = assetIdCollateral;
  let w5 = BigInt(withdrawalToAddress); // Constant identifier
  w5 = (w5 << 64) + BigInt(positionId);
  w5 = (w5 << 32) + BigInt(nonce); 
  w5 = (w5 << 64) + BigInt(amount);
  w5 = (w5 << 32) + BigInt(expirationTimestamp);
  w5 = w5 << 49;

  // Calculate Pedersen hash
  return pedersen([
    pedersen([w1, ethAddress]),
    w5.toString(16)
  ]);
}
```

## Limit Order Signature
Used to authorize a limit order for perpetual trading.

### Parameters
- `assetIdSynthetic` - Synthetic asset ID from [`meta_data.contractList.starkExSyntheticAssetId`](public-api/metadata-api.md)
- `assetIdCollateral` - Collateral asset ID from [`meta_data.coinList.starkExAssetId`](public-api/metadata-api.md) 
- `isBuyingSynthetic` - `true` for buy orders, `false` for sell orders
- `assetIdFee` - Fee token asset ID from [`meta_data.coinList.starkExAssetId`](public-api/metadata-api.md)
- `amountSynthetic` - Amount of synthetic asset
- `amountCollateral` - Amount of collateral asset
- `maxAmountFee` - Maximum fee amount allowed
- `nonce` - Unique order identifier
- `positionId` - User's position ID
- `expirationTimestamp` - Unix timestamp when order expires

### Calculation
The following TypeScript function constructs the limit order message for signing:

```typescript
function getLimitOrderMsg({
  assetIdSynthetic,
  assetIdCollateral,
  isBuyingSynthetic,
  assetIdFee,
  amountSynthetic,
  amountCollateral,
  maxAmountFee,
  nonce,
  positionId,
  expirationTimestamp
}) {
  // Determine sell/buy assets based on order side
  const [assetIdSell, assetIdBuy] = isBuyingSynthetic 
    ? [assetIdCollateral, assetIdSynthetic]
    : [assetIdSynthetic, assetIdCollateral];
  const [amountSell, amountBuy] = isBuyingSynthetic
    ? [amountCollateral, amountSynthetic] 
    : [amountSynthetic, amountCollateral];

  // Pack order data into 256-bit words
  const w1 = assetIdSell;
  const w2 = assetIdBuy;
  const w3 = assetIdFee;

  // Calculate message hash
  let msg = pedersen([w1, w2]);
  msg = pedersen([msg, w3]);

  let w4 = BigInt(amountSell);
  w4 = (w4 << 64) + BigInt(amountBuy);
  w4 = (w4 << 64) + BigInt(maxAmountFee);
  w4 = (w4 << 32) + BigInt(nonce);
  msg = pedersen([msg, w4.toString(16)]);

  let w5 = BigInt(limitOrderWithFees); // Constant identifier
  w5 = (w5 << 64) + BigInt(positionId);
  w5 = (w5 << 64) + BigInt(positionId);
  w5 = (w5 << 64) + BigInt(positionId);
  w5 = (w5 << 32) + BigInt(expirationTimestamp);
  w5 = w5 << 17;

  return pedersen([msg, w5.toString(16)]);
}
```

## Transfer Signature
Used to authorize transfers between Layer 2 accounts.

### Parameters
- `assetId` - Asset ID being transferred
- `receiverPublicKey` - Recipient's public key
- `senderPositionId` - Sender's position ID
- `receiverPositionId` - Recipient's position ID  
- `srcFeePositionId` - Fee source position ID
- `nonce` - Unique transfer identifier
- `amount` - Transfer amount
- `expirationTimestamp` - Unix timestamp when transfer expires
- `assetIdFee` - Fee token asset ID (optional, default '0')
- `maxAmountFee` - Maximum fee amount (optional, default '0')

### Calculation
The following TypeScript function constructs the transfer message for signing:

```typescript
function getTransferMsg({
  assetId,
  receiverPublicKey,
  senderPositionId,
  receiverPositionId,
  srcFeePositionId,
  nonce,
  amount,
  expirationTimestamp,
  assetIdFee = '0',
  maxAmountFee = '0'
}) {
  // Pack transfer data into 256-bit words
  const w1 = assetId;
  const w2 = assetIdFee;
  const w3 = receiverPublicKey;

  let w4 = BigInt(senderPositionId);
  w4 = (w4 << 64) + BigInt(receiverPositionId);
  w4 = (w4 << 64) + BigInt(srcFeePositionId); 
  w4 = (w4 << 32) + BigInt(nonce);

  let w5 = BigInt(transfer); // Constant identifier
  w5 = (w5 << 64) + BigInt(amount);
  w5 = (w5 << 64) + BigInt(maxAmountFee);
  w5 = (w5 << 32) + BigInt(expirationTimestamp);
  w5 = w5 << 81;

  // Calculate message hash
  let msg = pedersen([w1, w2]);
  msg = pedersen([msg, w3]);
  msg = pedersen([msg, w4.toString(16)]);
  return pedersen([msg, w5.toString(16)]);
}
```

For more details on the signature construction, see the [StarkEx documentation](https://docs.starkware.co/starkex/perpetual/signature_construction_perpetual.html).
