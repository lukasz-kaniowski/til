# How to enable kms encryption for cloudtrail


## Create kms key with key rotation and policy for cloudtrail

Kms requires alias to be created.

Below policy is default one that aws creates if this would be done via aws console.

 ```terraform
variable "aws_account_no" {}

resource "aws_kms_key" "cloudtrail" {
  description = "KMS key for claoudtrail logs"
  deletion_window_in_days = 10
  enable_key_rotation = true
  policy = <<POLICY
{
  "Version": "2012-10-17",
  "Id": "Key policy created by terraform for cloudtrail",
  "Statement": [
    {
      "Sid": "Enable IAM User Permissions",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::${var.aws_account_no}:root"
      },
      "Action": "kms:*",
      "Resource": "*"
    },
    {
      "Sid": "Allow CloudTrail to encrypt logs",
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudtrail.amazonaws.com"
      },
      "Action": "kms:GenerateDataKey*",
      "Resource": "*",
      "Condition": {
        "StringLike": {
          "kms:EncryptionContext:aws:cloudtrail:arn": "arn:aws:cloudtrail:*:${var.aws_account_no}:trail/*"
        }
      }
    },
    {
      "Sid": "Allow CloudTrail to describe key",
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudtrail.amazonaws.com"
      },
      "Action": "kms:DescribeKey",
      "Resource": "*"
    },
    {
      "Sid": "Allow principals in the account to decrypt log files",
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": [
        "kms:Decrypt",
        "kms:ReEncryptFrom"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "kms:CallerAccount": "${var.aws_account_no}"
        },
        "StringLike": {
          "kms:EncryptionContext:aws:cloudtrail:arn": "arn:aws:cloudtrail:*:${var.aws_account_no}:trail/*"
        }
      }
    },
    {
      "Sid": "Enable cross account log decryption",
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": [
        "kms:Decrypt",
        "kms:ReEncryptFrom"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "kms:CallerAccount": "${var.aws_account_no}"
        },
        "StringLike": {
          "kms:EncryptionContext:aws:cloudtrail:arn": "arn:aws:cloudtrail:*:${var.aws_account_no}:trail/*"
        }
      }
    }
  ]
}
POLICY
}

resource "aws_kms_alias" "cloudtrail" {
  name = "alias/cloudtrail-kms-key"
  target_key_id = "${aws_kms_key.cloudtrail.key_id}"
}
 ```

## Configure cloudtrail to use kms key to encrypt logs

```terraform
resource "aws_cloudtrail" "sample-trail" {
  ...
  kms_key_id = "${aws_kms_key.cloudtrail.arn}"
}
```

## Useful resources:

* [How AWS CloudTrail Uses AWS KMS](http://docs.aws.amazon.com/kms/latest/developerguide/services-cloudtrail.html)
* [Required CMK policy sections for use with CloudTrail](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/create-kms-key-policy-for-cloudtrail-policy-sections.html)
