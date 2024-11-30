
# AWS KMS (Key Management Service)
- Regional service (keys are bound to a region)
- Encrypt up to 4KB of data per call (if data > 4 KB, use envelope encryption)
- Anytime you hear "encryption" for an AWS service, it's most likely KMS
- AWS manages encryption keys for us
- Fully integrated with IAM for authorization
- Easy way to control access to your data
- Able to audit KMS key usage using CloudTrail
- Need to set IAM Policy & Key Policy to allow a user or role to access a KMS key
- Seamlessly integrated into most AWS services (RDS, EBS, S3...)
- **Never ever store your secret in plain text, especially in your code!**
  - KMS key encryption is also available thrrough API calls (SDK/CLI)
  - Encrypted secrets can be stored on the code / environment variables

## KMS Keys Types
- KMS Keys is the new name of KMS Customer Master Key
- Symmetric (AES-256 keys)
  - Single encryption key that is used to encrypt and decrypt
  - necessary for envelope encryption
  - AWS services that are integrated with KMS use Symmetric CMKs
  - You never get access to KMS Key unencrypted (must call KMS API to use)
- Asymmetric (RSA && ECC key pairs)
  - Public (Encrypt) and Private (Decrypt) key pairs
  - Used for Encrypt/Decrypt, or Sign/Verify operations
  - The public key is downloadable but you can't access the private key unencrypted
  - Not eligible for automatic rotation (use manual rotation)
  - No need to call the KMS API to encrypt data (data can be encrypted by the client)
  - Use case: encryption outside of AWS by users who can't call the KMS API
 
 - Three types of KMS Keys:
   - AWS Managed key: free (aws/service-name, example: aws/rds or aws/ebs)
     - Default KMS key for each supported service
     - Fully managed by AWS (cannot view, rotate or delete them)
   - Customer Managed Keys (CMK) created in KMS: $1/month
     - Option to enable automatic yearly rotation
   - Customer Managed Keys imported (must be 256-bit symmetric key): $1/month
     - Generated and imported from outside
     - Not recommended
- Deletion has a waiting period (pending deletion state) between 7 - 30 days (default 30 days). The key can be recovered during the pending deletion state.
- In addition pay for API calls to KMS ($0.03/10000 calls)
- Automatic key rotation
  - AWS managed KMS key: automatic every 1 year
  - Customer managed KMS key: (must be enabled) automatic every 1 year
  - Imported KMS key: only manual rotation is possible using alias

## KMS Key Policies
- Control access to KMS keys, "similar" to S3 bucket policies
- Difference: you can't control access without them
- Default KMS key policy:
  - Created if you don't provide a specific KMS key policy
  - Complete access to the key to the root user = entire AWs account
- Custom KMS Key Policy:
  - Define users, roles that can access the KMS key
  - Define who can administer the key
  - Useful for cross-account access of your KMS key

## Copying snapshots across accounts
1. Create a snapshot, encrypted with your own KMS key (Customer Managed Key)
2. Attach a KMS key policy to authorize cross-account access
3. Share the encrypted snapshot
4. (in target) Create a copy of the snapshot, encrypt it with a CMK in your account
5. Create a volume from the snapshot

<img src=./images/kmskeypolicy.png width="500"/>

## Cross-region Encrypted Snapshot Migration
- Copy the snapshot to another region with re-encryption option using a new key in the new region (keys are bound to a region)

<img src=./images/snapshotcopyenc.png width="500"/>

## KMS Multi-Region Keys
- Identical KMS keys in different AWS regions that can be used interchangeably
- Multi-region keys have the same key ID, key material, automatic rotation...
- Encrypt in one region and decrypt in other regions
- No need to re-encrypt or making cross-region API calls
- KMS multi-region are not global (Primary + Replicas)
- Each multi-region is key is managed independently
- Use cases: global client-side encryption, encryption on Global DynamoDB, global Aurora

<img src=./images/kmsmulti.png width="500"/>

## S3 replication with encryption
- **Unencrypted objects and objects encrypted with SSE-S3 are replicated by default**
- Objects encrypted with SSE-C are never replicated
- **for objects encrypted with SSE-KMS,** you need enable the option
- You can use multi-region KMS keys, but they are currently treated as independent keys by Amazon S3

## Encrypted AMI sharing process
1. AMI in source account is encrypted with KMS key from source account
2. Must modify the image attribute to add a **Launch Permission** which corresponds to the specified target AWS account
3. Must share the KMS keys used to encrypt the snapshot with the target account/IAM Role
4. The IAM Role/User in the target account must have the permissions to DescribeKey, ReEncrypted, CreateGrant, Decrypt
5. When launching an EC2 from the AMI, optionally the target account can specify a new KMS key in its own account to re-encrypt the volumes