## BE_CreateKeyPair

    BE_CreateKeyPair

**Description**

Generates an RSA Private Key in text format as :

-----BEGIN RSA PRIVATE KEY-----
MIIJKQIBAAKCAgEAsbUeQ/7WK0Dltk4Ko/MAkAyowbHL7OkCRIBLjdd54nJIwFbF
...
WzdSYrUaE5Dnwdf8V0GjBKg6HsiFzgDXW2sEKd/Y8zQ91Pqnhh914bLczCf7
-----END RSA PRIVATE KEY-----

**Parameters**

None.

**Keywords**

Create Key Pair RSA

**Version History**

* 5.0.0 : First Release

**Notes**

You can extract the public key from Private Keys using BE_GetPublicKey.

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

	Set Variable [ $privateKey ; BE_CreateKeyPair ]
