# How To Sign Message

## How To GET Your L2 Private Key

To sign messages on Layer 2, you need to obtain your L2 private key. This key is used to generate signatures that authorize various actions on the platform.

<figure><img src="../../.gitbook/assets/20250102-134437.png" alt=""><figcaption><p><strong>How To GET Your L2 Private Key</strong></p></figcaption></figure>

> **Warning:** Keep your private key secure and never share it with anyone. Anyone with access to your private key can sign messages on your behalf.

## Signature Algorithm

The signature algorithm used is **Ecdsa** (Elliptic Curve Digital Signature Algorithm). This algorithm ensures that signatures are secure and verifiable.

> [Python Signature Demo](https://github.com/starkware-libs/starkex-resources/blob/master/crypto/starkware/crypto/signature/signature_test.py#L62)

> [Java Script Signature Demo](https://www.npmjs.com/package/@starkware-industries/starkware-crypto-utils#signing-a-starkex-order)

### Java Implementation Demo

Below is a Java implementation of the Ecdsa signature algorithm. This example demonstrates how to sign a message using a private key.

``` java
import java.math.BigInteger;
import org.web3j.abi.TypeEncoder;
import org.web3j.abi.datatypes.Utf8String;
import org.web3j.abi.datatypes.generated.Uint256;
import org.web3j.crypto.Hash;
import org.web3j.utils.Numeric;

public class EcdsaSignatureDemo {
    public static final BigInteger K_MODULUS = Numeric
            .toBigInt("0x0800000000000010ffffffffffffffffb781126dcae7b2321e66a241adc64d2f");

    public static void main(String[] args) {
        String privateKeyHex = "0463ac809cc7d7c1baf*********************baff9fc6e3d8e5b160ea3fc";

        // Ensure that the private key is a hexadecimal string without the "0x" prefix.
        if (privateKeyHex.startsWith("0x")) {
            privateKeyHex = privateKeyHex.substring(2);
        }

        BigInteger mySecretKey = new BigInteger(privateKeyHex, 16);
        PrivateKey privateKey = PrivateKey.create(mySecretKey);

        String message = "1735542383256GET/api/v1/private/account/getPositionTransactionPageaccountId=543429922991899150&filterTypeList=SETTLE_FUNDING_FEE&size=10";
        String msg = TypeEncoder.encodePacked(new Utf8String(message));

        BigInteger msgHash = Numeric.toBigInt(Hash.sha3(Numeric.hexStringToByteArray(msg)));

        msgHash = msgHash.mod(K_MODULUS);

        Signature signature = Ecdsa.sign(msgHash, privateKey);

        String starkSignature = TypeEncoder.encodePacked(new Uint256(signature.r)) +
                TypeEncoder.encodePacked(new Uint256(signature.s)) +
                TypeEncoder.encodePacked(new Uint256(privateKey.publicKey().point.y));

        System.out.println(starkSignature);
    }

    public static Signature sign(BigInteger msgHash, PrivateKey privateKey) {
        Curve curve = privateKey.curve;
        BigInteger randNum = new BigInteger(curve.N.toByteArray().length * 8 - 1, new SecureRandom()).abs().add(BigInteger.ONE);
        Point randomSignPoint = EcMath.multiply(curve.G, randNum, curve.N, curve.A, curve.P);
        BigInteger r = randomSignPoint.x.mod(curve.N);
        BigInteger s = ((msgHash.add(r.multiply(privateKey.secret))).multiply(EcMath.inv(randNum, curve.N))).mod(curve.N);
        return Signature.create(r, s);
    }
}
```

# Signature Construction Guide

This section provides detailed instructions on constructing signatures for various actions on the platform.

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