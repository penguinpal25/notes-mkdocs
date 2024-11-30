# Transfer Family
- AWS managed service to transfer files in and out of S3 using FTP (instead of using proprietary methods)
- Supported Protocols
  - FTP (File Transfer Protocol) - unencrypted in flight
  - FTPS (File Transfer Protocol over SSL) - encrypted in flight
  - SFTP (Secure File Transfer Protocol) - encrypted in flight
- Supports Multi AZ
- Pay per provisioned endpoint per hour + fee per GB data transfers
- Clients can either connect directly to the FTP endpoint or optionally through Route 53
- Transfer Family will need permission to read or put data into S3 or EFS
- Store and manage user's credentials within the service, can integrate with existing authentication systems (MS Active Directory, LDAP, Okta, Amazon Cognito)

<img src=./images/tf.png width="500"/>