# Encoding and Encryption — BaseElements Plugin

## BE_Encrypt_AES

```
BE_Encrypt_AES ( key ; text )
```

**Description**

Encrypts the value in *text* encrypted with AES encryption using the supplied *key*.

Result is the encrypted text or ? for errors.  Use the BE_GetLastError function to check for a successful result.

**Parameters**

* *key* : The secret key to use to encrypt the string.
* *text* : The text to be encrypted.

**Version History**

* 2.3.0 : First Release
* 4.0.0 : Deprecated with a recommendation to use the FileMaker 16 function *CryptEncrypt* ( data ; key ) instead.
* 4.1.0 : Removed the deprecation.

**Notes**

We removed the deprecation as the native *CryptEncrypt* function doesn't quite do all the same things that our function does, so there is still room for both.  We do recommend users use the native function where it's suitable.

The intention of this is purely that anything encrypted by the plugin can only be decrypted by the plugin and there is no guarantee that the methods and code used is the same as any other external AES encryption.

Although it is based on open standards, and you may find that you can encrypt/decrypt with software other than the BE plugin, it isn't guaranteed.

Specific external compatibility testing would be useful to add.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

---

## BE_Decrypt_AES

```
BE_Decrypt_AES ( key ; text )
```

**Description**

Decrypts the value in *text* encrypted with the BE_Encrypt_AES function and using the supplied *key*.

Result is the original unencrypted text or ? for errors.  Use the BE_GetLastError function to check for a successful result.

**Parameters**

* *key* : The secret key used in the BE_Encrypt_AES function when the text was encrypted.
* *text* : The text to be encrypted.

**Version History**

* 2.3.0 : First Release
* 4.0.0 : Deprecated with a recommendation to use the FileMaker 16 function *CryptDecrypt* ( data ; key ) instead.
* 4.1.0 : Removed the deprecation.

**Notes**

We removed the deprecation as the native *CryptDecrypt* function doesn't quite do all the same things that our function does, so there is still room for both.  We do recommend users use the native function where it's suitable.

The intention of this is purely that anything encrypted by the plugin can only be decrypted by the plugin and there is no guarantee that the methods and code used is the same as any other external AES encryption.

Although it is based on open standards, and you may find that you can encrypt/decrypt with software other than the BE plugin, it isn't guaranteed.

Specific external compatibility testing would be useful to add.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

---

## BE_CipherEncrypt

```
BE_CipherEncrypt ( cipher ; data ; key ; iv ; { paddingBoolean ; fileNameWithExtension } )
```

**Description**

Decrypt *encryptedData* using *cipher*.

**Parameters**

* *cipher* ( default:AES-256-CBC ) : The cipher name to use. If the value is empty, "AES-256-CBC" will be used.
* *data* : The data to encrypt. It can be HEX encoded text or container field.
* *key* : The key to encrypt with. It can be HEX encoded text or container field.
* *iv* : The IV (initialization vector). It can be HEX encoded text or container field.
* *paddingBoolean* ( optional, default:True ) : Set false to disable padding. Padding is enabled if the parameter is true, empty or not specified.
* *fileNameWithExtension* : The filename and extension for the encrypt data.

**Version History**

* 4.0.0 : First Release

**Notes**

Be careful, there is no undo. Modes that require authenticated encryption (e.g. GCM) are not supported. These ciphers return 'authentication tag' in addition to encrypted data.

The max data length is limited to 2GB.

The code uses OpenSSL for it's base library, so the list of ciphers and their details are here : https://www.openssl.org/docs/man1.0.2/man1/ciphers.html however not all available ciphers may work, and this list may change with future releases of openssl.

Not all possible ciphers will work, and thorough testing on Encryption and Decryption should be done before putting code into production.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

**Example Code**

	BE_CipherEncrypt ( "aes-256-cbc" ; HexEncode ( "Plain text" ) ; "603deb1015ca71be2b73aef0857d77811f352c073b6108d72d9810a30914dff4" ; "000102030405060708090a0b0c0d0e0f" )

---

## BE_CipherDecrypt

```
BE_CipherDecrypt ( cipher ; encryptedData ; key ; iv ; { paddingBoolean ; fileNameWithExtension } )
```

**Description**

Decrypt *encryptedData* using *cipher*.

**Parameters**

* *cipher* ( default:AES-256-CBC ) : The cipher name to use. If the value is empty, "AES-256-CBC" will be used.
* *encryptedData* : The data to decrypt. It can be HEX encoded text or container field.
* *key* : The key to decrypt with. It can be HEX encoded text or container field.
* *iv* : The IV (initialization vector). It can be HEX encoded text or container field.
* *paddingBoolean* ( optional, default:True ) : Set false to disable padding. Padding is enabled if the parameter is true, empty or not specified.
* *fileNameWithExtension* : The filename and extension for the decrypted data if being set into a container field.

**Version History**

* 4.0.0 : First Release

**Notes**

Modes that require authenticated encryption (e.g. GCM) are not supported. These ciphers return 'authentication tag' in addition to encrypted data.

The max data length is limited to 2GB.

The code uses OpenSSL for it's base library, so the list of ciphers and their details are here : https://www.openssl.org/docs/man1.0.2/man1/ciphers.html however not all available ciphers may work, and this list may change with future releases of openssl.

Not all possible ciphers will work, and thorough testing on Encryption and Decryption should be done before putting code into production.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

---

## BE_MessageDigest

```
BE_MessageDigest ( text ; { algorithm ; encoding } )
```

**Description**

Returns a hash value generated by the MD5 or SHA functions from the text given, and optional encoding.

**Parameters**

* *text* : description.
* *algorithm* ( optional, default:BE_MessageDigestAlgorithmSHA256 ) : The type of hashing function to use.
* *encoding* ( optional, default:none ) : Possible values are BE_EncodingHex, BE_EncodingBase64, or no encoding for a binary result.

**Version History**

* 1.2.0 : First Release
* 3.0.0 : Added the optional encoding parameter

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

**Example Code**

	BE_MessageDigest ( $text )
	BE_MessageDigest ( $text ; BE_MessageDigestTypeMD5 )
	BE_MessageDigest ( $text ; BE_MessageDigestTypeSHA256 ; BE_EncodingBase64 )
	BE_MessageDigest ( $text ; 2 ; 1 ) // BE_MessageDigestAlgorithm_SHA256 and BE_EncodingHex

---

## BE_SignatureGenerateRSA

```
BE_SignatureGenerateRSA ( data ; privateKey ; { privateKeyPassword ; algorithm ; fileNameWithExtension } )
```

**Description**

Generates a digital signature of *data*. The message digest of data is calculated first using the specified *algorithm*. Then the digest is encrypted with *privateKey*.

**Parameters**

* *data* : The data to be signed. It can be text or container field.
* *privateKey* : The openssl private key (as text) to digitally sign the input text.   PKCS#1 and PKCS#8 PEM formats are supported.
* *privateKeyPassword* ( optional, default:empty ) : The password of the private key if required. It will be ignored if the private key is not password-protected.
* *algorithm* ( optional, default:SHA256 ) : The hash algorithm name (as text) use to calculate the digest of the data.
* *fileNameWithExtension* ( optional, default:BE_MessageDigestAlgorithmSHA256 ) : The filename and extension for the generated signature file.

**Version History**

* 4.0.0 : First Release
* 4.0.2 : Renamed from BE_SignatureGenerate_RSA

**Notes**

Uses OpenSSL so algorithms come from there, and should be generally compatible with the equivalent openssl commands.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

---

## BE_SignatureVerifyRSA

```
BE_SignatureVerifyRSA ( data ; publicKey ; { signature ; algorithm )
```

**Description**

Verifies if signature is valid for *data* by comparing the message digest of *data* with the digest obtained from *signature* by decrypting it using *publicKey*.

**Parameters**

* *data* : The source data that was signed. It can be text or container field.
* *publicKey* : The openssl public key (as text) to verify the digital signature. PKCS#1 and PKCS#8 PEM formats are supported.
* *signature* : The digital signature. It can be container field or Base64 encoded text.
* *algorithm* ( optional, default:SHA256 ) : The hash algorithm name (as text) use to calculate the digest of the data.

**Version History**

* 4.0.0 : First Release
* 4.0.2 : Renamed from BE_SignatureVerify_RSA

**Notes**

Uses OpenSSL so algorithms come from there, and should be generally compatible with the equivalent openssl commands.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

---

## BE_SetTextEncoding

```
BE_SetTextEncoding ( { encoding } )
```

**Description**

Sets the text encoding format for any function that writes or reads text on disk, for example the *BE_WriteTextToFile* function.

**Parameters**

* *encoding* ( optional, default:UTF_8 ) : the encoding format to use. By leaving this parameter off, or setting it to empty you reset the encoding to its default.

**Version History**

* 1.0.0 : First Release

**Notes**

This is using the iconv library so the full list of encoding options can be found at 

[http://www.gnu.org/software/libiconv/ ](http://www.gnu.org/software/libiconv/ ) 

or by typing "iconv -l" on the command line.

Note that if you've got content that does not match the encoding specified, the result may be an error or may be indeterminate depending on the library in use.  As a general rule we don't try to convert one set of text to another as the outcome may not be as the user intended.

If you're at all curious about encoding and why it's so complex, this is a good write up :

[https://tonsky.me/blog/unicode/](https://tonsky.me/blog/unicode/) 

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

**Example Code**

To use UTF_16 which is the default encoding of the XML DDR : 

	BE_SetTextEncoding ( "UTF-16" )

To use ISO-8859-1 which is a common windows encoding.

	BE_SetTextEncoding ( "ISO-8859-1" )

To revert back to default UTF-8 :

	BE_SetTextEncoding

---

## BE_CreateKeyPair

```
BE_CreateKeyPair
```

**Description**

Generates an RSA Private Key in text format as :

-----BEGIN RSA PRIVATE KEY-----
MIIJKQIBAAKCAgEAsbUeQ/7WK0Dltk4Ko/MAkAyowbHL7OkCRIBLjdd54nJIwFbF
...
WzdSYrUaE5Dnwdf8V0GjBKg6HsiFzgDXW2sEKd/Y8zQ91Pqnhh914bLczCf7
-----END RSA PRIVATE KEY-----

**Version History**

* 5.0.0 : First Release

**Notes**

You can extract the public key from Private Keys using BE_GetPublicKey.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

---

## BE_EncryptWithKey

```
BE_EncryptWithKey ( text ; keyText )
```

**Description**

Encrypts the **text** with they **keyText** supplied.

**Parameters**

* *text* : description.
* *keyText* : description.

**Version History**

* 1.0.0 : First Release

**Notes**

If you have issues with the keys, check the line endings.  The DataViewer can mangle line endings in copy and pasted text, and line breaks in keys need to be Char ( 10 ).

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

---

## BE_DecryptWithKey

```
BE_DecryptWithKey ( encryptedText ; keyText )
```

**Description**

Decrypts some text that was previously encrypted using the public RSA version of this key.

**Parameters**

* *encryptedText* : the encrypted text as base64.
* *keyText* : the key in RSA PRIVATE KEY format as text.

**Version History**

* 5.0.0 : First Release

**Notes**

Uses the same logic as openssl on the command line so should be compatible with that.  These keys can be used as access keys for SSH or converted to other formats.

If you have issues with the keys, check the line endings.  The DataViewer can mangle line endings in copy and pasted text, and line breaks in keys need to be Char ( 10 ).

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

---

## BE_GetPublicKey

```
BE_GetPublicKey ( privateKey )
```

**Description**

Generates the public key from a given **privateKey** generated with the BE_CreateKeyPair function.

**Parameters**

* *privateKey* : the RSA Private Key in text format.

**Version History**

* 5.0.0 : First Release

**Notes**

If you have issues with the keys, check the line endings.  The DataViewer can mangle line endings in copy and pasted text, and line breaks in keys need to be Char ( 10 ).

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |
