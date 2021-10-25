# Kms Decrypt
The objective is to create a kms key, encrypt a text and later decrypt it with a cloudformation to save it in the Secrets manager service. So we can save the encrypted password within our repositories by making a version control of it.
- The first step that I have done has been the creation of a kms key through the aws cli
```json
$ aws kms create-key 
{
    "KeyMetadata": {
        "AWSAccountId": "243425076453",
        "KeyId": "c6874718-316c-4261-a9bc-c7524703ab18",
        "Arn": "arn:aws:kms:eu-central-1:243425076453:key/c6874718-316c-4261-a9bc-c7524703ab18",
        "CreationDate": "2021-10-22T23:17:57.519000+02:00",
        "Enabled": true,
        "Description": "",
        "KeyUsage": "ENCRYPT_DECRYPT",
        "KeyState": "Enabled",
        "Origin": "AWS_KMS",
        "KeyManager": "CUSTOMER",
        "CustomerMasterKeySpec": "SYMMETRIC_DEFAULT",
        "KeySpec": "SYMMETRIC_DEFAULT",
        "EncryptionAlgorithms": [
            "SYMMETRIC_DEFAULT"
        ],
        "MultiRegion": false
    }
}
```
- - After having created the kms key, I encrypt a text
```json
$ aws kms encrypt --key-id 'c6874718-316c-4261-a9bc-c7524703ab18' --plaintext hola
{
    "CiphertextBlob": "AQICAHjotYDwr4vd5K5jtkZ3lrxMrE/pDNpzuZe48sooveGVrwEsvn2P/S7ZDtGcmBGCc++iAAAAYTBfBgkqhkiG9w0BBwagUjBQAgEAMEsGCSqGSIb3DQEHATAeBglghkgBZQMEAS4wEQQMan9NMYQ7wTzT4M6QAgEQgB7Bocu/r6HqksBYmGuO/hcGHVGOC0K20VmeklVyHfk=",
    "KeyId": "arn:aws:kms:eu-central-1:243425076453:key/c6874718-316c-4261-a9bc-c7524703ab18",
    "EncryptionAlgorithm": "SYMMETRIC_DEFAULT"
}
```
To decrypt this password I have used the Lambda service creating a function whose only task is to decrypt the text with the *boto3* library, for this I created the `decrypt` roles and in another separate file I have created a custom resource where it calls It has this function and passes it the parameters of the id of the kms key and the encrypted text.
As output from the cloudformation stack there is the password already decrypted and the secret created in the resources.
To create the secret resource I have created a dummy value for the key.

I think the best way to have control over our passwords is the direct use of the secrets manager service, the random creation and the rotary encryption of kms, already integrated in this service.
