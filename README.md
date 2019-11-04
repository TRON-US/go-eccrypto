# go-eccrypto
go Elliptic curve cryptography library

#### Implementation details
- Only secp256k1 curve, only SHA-512 (KDF), HMAC-SHA-256 (HMAC) and AES-256-CBC for ECIES

#### Usage
```
const msg = "hello,world\n"
const pkHex = "048903aca62f342426d0595597bcd4b03519723c7292f231a5d40c02" +
	"d43c0f69309de7166e91d2fe7a479bcad8a4f611175b8a593178683518fcab05b8bf74dc09"
const privHex = "0abfa58854e585d9bb04a1ffad0f5ac507ac042e7aa69abbcf18f3103a936f6f"

cipherText, metadata, err := Encrypt(pkHex, []byte(msg))
decrypted, err := Decrypt(privHex, cipherText, metadata)
```

#### Encryption:

```
1. Use `Public key` and `Private Key` to derive the `Share Secret`.

2. Use `Share Secret` to do SHA512.

3. Generate 16 bytes random IV.

4. Take the value of the step2 as the first 32 bits as `encryptionKey`

5. Take the value of the step2 as the last 32 bits as `macKey`

6. Encrypt iv and encryptionKey with AES-256-CBC to get `ciphertext`。
```

#### Decryption:

```
1. Use `Public key` and `Private Key` to derive `Share Secret`

2. Use `Share Secret` to do SHA512.

3. Take the value of the step2 as the first 32 bits as `encryptionKey`.

4. Take the value of the step2 as the last 32 bits as `macKey`.

5. Verify whether the macKey same as input macKey.

6. Input iv、encryptionKey、ciphertext，and decrypt with AES-256-CBC.
```

Reference:
https://en.wikipedia.org/wiki/Integrated_Encryption_Scheme
https://github.com/bitchan/eccrypto
