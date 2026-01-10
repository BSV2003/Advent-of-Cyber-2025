# AWS Security - S3cret Santa
Learn the basics of AWS enumeration.

## Learning Objectives
- Learn the basics of AWS accounts.
- Enumerate the privileges granted to an account, from an attacker's perspective.
- Familiarise yourself with the AWS CLI.

**AWS:** Amazon Web Services (AWS) is a comprehensive cloud computing platform offered by Amazon. It provides a wide range of services such as computing power, storage, databases, networking, analytics, and more, delivered over the internet on a pay-as-you-go basis.

AWS accounts can be accessed programmatically by using an Access Key ID and a Secret Access Key.
The AWS CLI will look for credentials at ~/.aws/credentials, where you should see something similar to the following:
```
aws_access_key_id = AKIAU2.....Z
aws_secret_access_key = DhMy3ac4N6UBRiyKD43u0mdEBueOMKzyvnG+/FhI
```

Amazon Security Token Service (STS) allows us to utilise the credentials of a user that we have saved during our AWS CLI configuration. We can use the `get-caller-identity` call to retrieve information about the user we have configured for the AWS CLI.
```bash
user@machine$ aws sts get-caller-identity
```
**Output:**
```
{
    "UserId": "AIDAU2VYTBGYOHNOCJMX3",
    "Account": "332173347248",
    "Arn": "arn:aws:iam::332173347248:user/sir.carrotbane"
}
```

---

# IAM: Users, Roles, Groups and Policies

## 📌 What is IAM?

**AWS Identity and Access Management (IAM)** is used to manage who can access AWS resources and what actions they are allowed to perform.

Misconfigured IAM is one of the most common causes of cloud breaches, leading to exposure of:
- Sensitive data
- Customer information
- Internal documents

Real-world examples include major organisations like **Toyota, Accenture, and Verizon**.

## 👤 IAM Users

- Represents a **single identity** in AWS
- Has **long-term credentials**:
-- Password (console access)
-- Access Key ID & Secret Access Key (programmatic access)
- Permissions can be assigned **directly** or via groups

🔹 _Example:_ Sir Carrotbane as an individual AWS user.

## 👥 IAM Groups

- Collection of IAM users
- Used to **simplify permission management**
- Permissions are assigned to the **group**, not individual users

**Why groups matter:**
- Easier onboarding/offboarding
- Reduced configuration errors
- Centralised access control

🔹 _Example:_ Carrotbane’s army grouped together with the same permissions.

## 🎭 IAM Roles

- **Temporary identity**
- Can be assumed by:
-- IAM users
-- AWS services
-- External accounts
- No long-term credentials
- Uses **STS (Security Token Service)** for temporary access

**Security impact:**
- Misconfigured roles can lead to **privilege escalation**
- Common attacker target in AWS environments

🔹 _Example:_ Carrotbane assumes different roles depending on the mission (attacker vs defender).

## 📜 IAM Policies

Policies define **permissions** in AWS.

They are written in **JSON** and control:
- **Action** – What can be done
- **Resource** – On what
- **Effect** – Allow or Deny
- **Condition** – Optional constraints
- **Principal** – Who the policy applies to

**Example IAM Policy**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowSpecificUserReadAccess",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:user/Alice"
      },
      "Action": [
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::my-private-bucket/*"
    }
  ]
}
```

🔹 This policy allows:
- **User:** Alice
- **Action:** Read objects
- **Resource:** `my-private-bucket`

---

# Enumerating a User's Permissions

## 🔍 Enumerating IAM Users

With valid AWS credentials already configured, we begin by listing all IAM users in the account:
```bash
aws iam list-users
```

🔎 **Result**
- Displays all IAM users in the account
- Includes metadata such as **creation date** and **ARN**
- Useful for identifying potential privilege escalation targets

## 📜 Enumerating User Policies

IAM policies fall into two categories:
- **Inline Policies**
-- Attached directly to a user, group, or role
-- Deleted automatically when the identity is deleted
- **Managed (Attached) Policies**
-- Reusable across multiple identities
-- Updating the policy affects all attached identities

🔹 **List Inline Policies**
Check inline policies assigned to **sir.carrotbane**:
```bash
aws iam list-user-policies --user-name sir.carrotbane
```

🔹 **List Attached (Managed) Policies**
Check if any managed policies are attached:
```bash
aws iam list-attached-user-policies --user-name sir.carrotbane
```

👥 **Checking Group Membership**
Determine whether the user inherits permissions from a group:
```bash
aws iam list-groups-for-user --user-name sir.carrotbane
```

🔎 **Inspecting the Inline Policy**
Retrieve the full policy document to understand the granted permissions:
```bash
aws iam get-user-policy \
  --policy-name POLICYNAME \
  --user-name sir.carrotbane
```

📄 **Policy Document**
```json
{
  "UserName": "sir.carrotbane",
  "PolicyName": "POLICYNAME",
  "PolicyDocument": {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid": "ListIAMEntities",
        "Effect": "Allow",
        "Action": [
          "iam:ListUsers",
          "iam:ListGroups",
          "iam:ListRoles",
          "iam:ListAttachedUserPolicies",
          "iam:ListAttachedGroupPolicies",
          "iam:ListAttachedRolePolicies",
          "iam:GetUserPolicy",
          "iam:GetGroupPolicy",
          "iam:GetRolePolicy",
          "iam:GetUser",
          "iam:GetGroup",
          "iam:GetRole",
          "iam:ListGroupsForUser",
          "iam:ListUserPolicies",
          "iam:ListGroupPolicies",
          "iam:ListRolePolicies",
          "sts:AssumeRole"
        ],
        "Resource": "*"
      }
    ]
  }
}
```

---

# Assuming Roles

## 🔍 Enumerating IAM Roles

Since `sir.carrotbane` has permission to list roles, we start with:
```bash
aws iam list-roles
```

📄 **Result**
```json
{
  "Roles": [
    {
      "RoleName": "bucketmaster",
      "Arn": "arn:aws:iam::123456789012:role/bucketmaster",
      "AssumeRolePolicyDocument": {
        "Statement": [
          {
            "Effect": "Allow",
            "Principal": {
              "AWS": "arn:aws:iam::123456789012:user/sir.carrotbane"
            },
            "Action": "sts:AssumeRole"
          }
        ]
      }
    }
  ]
}
```

✅ Key Finding
- A role named `bucketmaster` exists
- It explicitly allows `sir.carrotbane` to assume it

## 📜 Enumerating Role Policies

🔹 **Inline Role Policies**
Check inline policies attached to the role:
```bash
aws iam list-role-policies --role-name bucketmaster
```
✔️ One inline policy found.

🔹 **Attached Role Policies**
Check for managed policies:
```bash
aws iam list-attached-role-policies --role-name bucketmaster
```
❌ No managed policies attached.

🔎 **Inspecting the Role Policy**
Retrieve the inline policy to understand granted permissions:
```bsh
aws iam get-role-policy \
  --role-name bucketmaster \
  --policy-name BucketMasterPolicy
```

📄 **Policy Document**
```json
{
  "Statement": [
    {
      "Sid": "ListAllBuckets",
      "Effect": "Allow",
      "Action": ["s3:ListAllMyBuckets"],
      "Resource": "*"
    },
    {
      "Sid": "ListBuckets",
      "Effect": "Allow",
      "Action": ["s3:ListBucket"],
      "Resource": [
        "arn:aws:s3:::easter-secrets-123145",
        "arn:aws:s3:::bunny-website-645341"
      ]
    },
    {
      "Sid": "GetObjectsFromEasterSecrets",
      "Effect": "Allow",
      "Action": ["s3:GetObject"],
      "Resource": "arn:aws:s3:::easter-secrets-123145/*"
    }
  ]
}
```

🧠 **Analysis**
The bucketmaster role grants:
- `s3:ListAllMyBuckets` → enumerate all S3 buckets
- `s3:ListBucket` → list contents of specific buckets
- `s3:GetObject` → read files from `easter-secrets-123145`
**🚨 This is a privilege escalation**
- Original user had only IAM enumeration permissions
- Role grants direct access to S3 data

## 🔐 Assuming the Role

To gain these permissions, assume the role using AWS STS:
```bash
aws sts assume-role \
  --role-arn arn:aws:iam::123456789012:role/bucketmaster \
  --role-session-name TBFC
```

📄 **Output**
```json
{
  "Credentials": {
    "AccessKeyId": "ASIAXXXXXXXXXXXX",
    "SecretAccessKey": "xxxxxxxxxxxxxxxx",
    "SessionToken": "FwoGZXIvYXdzEJr..."
  },
  "AssumedRoleUser": {
    "Arn": "arn:aws:sts::123456789012:assumed-role/bucketmaster/TBFC"
  }
}
```

🔧 **Setting Temporary Credentials**
Export the temporary credentials to your shell:
```bash
export AWS_ACCESS_KEY_ID="ASIAXXXXXXXXXXXX"
export AWS_SECRET_ACCESS_KEY="xxxxxxxxxxxxxxxx"
export AWS_SESSION_TOKEN="FwoGZXIvYXdzEJr..."
```

✅** Verifying Role Assumption**
Confirm the role is active:
```bash
aws sts get-caller-identity
```
✔️ Output should now show:
```
assumed-role/bucketmaster/TBFC
```

### 🧠 Security Takeaway
- `sts:AssumeRole` is high-risk
- Even low-privileged users can escalate access
- Misconfigured trust policies are a common AWS breach vector
- Always restrict:
-- Who can assume roles
-- What roles can access

---

# Grabbing a file from S3

## ☁️ What Is Amazon S3?

**Amazon S3 (Simple Storage Service)** is an object storage service used to store and retrieve any amount of data at any time.

**Key Concepts**
- **Bucket** → A container for objects (similar to a cloud directory)
- **Object** → A file stored inside a bucket (e.g., .txt, .jpg, .log)
- **Key** → The full path/name of the object inside the bucket

**Common Uses**
- Website assets (images, JS, CSS)
- Logs and backups
- Internal configuration files
- Sensitive documents (often misconfigured 😬)

### 🔍 Listing All S3 Buckets

Because the assumed role has `s3:ListAllMyBuckets`, we can enumerate all buckets:
```bash
aws s3api list-buckets
```

📌 **Observation**
One bucket stands out:
`easter-secrets-123145`

### 📂 Listing Objects in the Bucket

List the contents of the suspicious bucket:
```bash
aws s3api list-objects --bucket easter-secrets-123145
```

📄 **Result**
The bucket contains:
`cloud_password.txt`

### 📥 Downloading the File from S3

Since the role has `s3:GetObject` permission, we can download the file:
```bash
aws s3api get-object \
  --bucket easter-secrets-123145 \
  --key cloud_password.txt \
  cloud_password.txt
```
✔️ The file is now saved locally.

#### 🧠 Security Takeaways
- Sensitive files should never be stored in S3 without strict IAM controls
- Overly permissive roles (`GetObject`) lead directly to data exposure
- Always:
-- Use least privilege
-- Restrict bucket access
-- Enable logging (CloudTrail + S3 access logs)
