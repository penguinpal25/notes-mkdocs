## Encryption in flight (SSL)
- Data is encrypted before sending and decrypted after receiving
- SSL certificates help with encryption (HTTPS)
- Encryption in flight ensures no MITM (man in the middle attack) can happen

![enc1](https://user-images.githubusercontent.com/65948438/202100461-b1120c25-5120-4680-aa43-04e281708ac0.png)

## Server side encryption at rest
- Data is encrypted after being received by the server
- Data is decrypted before being sent
- It is stored in an encrypted form thanks to a key (usually a data key)
- The encryption/decryption keys must be managed by somewhere (like KMS) and the server must have access to it

![image](https://user-images.githubusercontent.com/65948438/202102600-3e0ea10f-b254-463b-9b44-817f9e43d47d.png)

## Client side encryption
- Data is encrypted by the client and never decrypted by the server
- Data will be decrypted by the receiving client
- The server should not be able to decrypt the data
- Could leverage Envelope Encryption

![image](https://user-images.githubusercontent.com/65948438/202103745-d4292790-b7f8-49fb-b498-d945deec28aa.png)

[Documentaion](https://docs.aws.amazon.com/whitepapers/latest/introduction-aws-security/data-encryption.html)
