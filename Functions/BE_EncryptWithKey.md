## BE_EncryptWithKey

    BE_EncryptWithKey ( text ; keyText )

**Description**

Encrypts the *text* with the *keyText* supplied.

**Parameters**

* *text* : the text to encrypt.
* *keyText* : the key in RSA PUBLIC KEY format as text.

**Keywords**

Encrypt With Key RSA

**Version History**

* 5.0.0 : First Release

**Notes**

If you have issues with the keys, check the line endings.  The DataViewer can mangle line endings in copy and pasted text, and line breaks in keys need to be Char ( 10 ).

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes  |
| Win FMP | Yes  |
| FMS | Yes  |
| iOS | Yes  |
| Linux | Yes  |

**Example Code**

	Set Variable [ $encrypted ; BE_EncryptWithKey ( $text ; $publicKey ) ]
