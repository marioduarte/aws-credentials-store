Manage secrets on AWS instances with KMS encryption. Since KMS is only used to store encryption keys,
the actual secrets should be stored on S3 or DynamoDB and encrypted using the KMS key.

There are 2 tools that try to simplify this process of storing and retrieving secrets on AWS using KMS:
* [credstash](#credstash)
* [aws-secrets](#aws-secrets)

---

# credstash

https://github.com/fugue/credstash

The secrets are encrypted and stored in a DynamoDB instance.

## Setup
This setup is to be done only once.

* Set up a key called `credstash` in KMS (found in the IAM console)
* Run `credstash setup`


## Usage

#### Stashing secrets
Whenever you want to store/share a credential, such as a database password, you simply run:
```
credstash put <credential-name> <credential-value>
```
For example, `credstash put myapp.db.prod supersecretpassword1234`.

#### Getting Secrets

```
credstash get <credential-name>
```
For example, `export DB_PASSWORD=$(credstash get myapp.db.prod)`

---

# aws-secrets

https://github.com/promptworks/aws-secrets

The secrets are encrypted and stored in S3 with one object per application.

### Setup new application
```
aws-secrets-init-resources <application-name>
```

Creates the following AWS resources:
* A customer master key (CMK).
* An alias for the key.
* An S3 bucket.
* A few roles to be used by an instance profile: one for S3 access, one for decryption with the CMK.
* A group with access policies to get/put to S3 and encrypt/decrypt with the CMK.

### Usage

#### Storing secrets for a given application
```
aws-secrets-send <application-name> <secrets-file-path>
```

The `secrets-file-path` should be a file containing lines of the form:
```
key=value
...
```

#### Retrieving secrets for a given application
```
export `aws-secrets-get <application-name>`
```

This will put the properties stored for that application as environment variables.
