# AWS IAM 

## 1. What is AWS IAM?

AWS Identity and Access Management (IAM) is a **global AWS service** that enables you to securely control **who can authenticate** to your AWS account and **what actions they are authorized to perform**. IAM is free to use, does not require region selection, and operates using a default **implicit deny** security model.

IAM does not control networking access; it strictly governs **identity-based and resource-based permissions**.

---

## 2. Core IAM Identities

### IAM Users

An IAM user represents a **single human or application** that requires long-term access to AWS. Users can authenticate using a password for console access or access keys for programmatic access. Each IAM user has **permanent credentials** and starts with no permissions by default.

Best practice dictates one IAM user per person, and IAM users should **never be used by AWS services**.

---

### IAM Groups

IAM groups are **logical collections of IAM users** that simplify permission management by allowing policies to be attached once and applied to many users. Groups cannot be nested and exist only to assign permissions; they cannot be authenticated.

A user can belong to multiple groups, enabling flexible permission modeling at scale.

---

### IAM Roles

An IAM role is a **temporary identity** that can be assumed by users, AWS services, or external identities. Roles do not have permanent credentials. Instead, AWS Security Token Service (STS) issues **temporary security credentials** when the role is assumed.

IAM roles are the **preferred mechanism** for granting permissions to AWS services such as EC2, Lambda, ECS, and for enabling secure cross-account access.

---

## 3. IAM Policies

IAM policies are **JSON documents** that define permissions by specifying which actions are allowed or denied on which resources, under what conditions. Policies enforce the **principle of least privilege** by granting only the permissions required to perform a task.

---

### Policy Structure Explained

An IAM policy consists of one or more statements that include an Effect, Action, Resource, and optional Condition. Resource-based policies also include a Principal to define who is allowed access.

The Effect determines whether access is allowed or denied, and an explicit deny always overrides any allow.

---

## 4. Types of IAM Policies

### AWS Managed Policies

AWS managed policies are prebuilt policies maintained by AWS. They provide broad access and automatically update when AWS services change. While convenient, they are typically **not least-privilege** and are best used for quick setup or standard roles.

---

### Customer Managed Policies

Customer managed policies are created and maintained by the AWS account owner. They are reusable, version-controlled, and provide fine-grained control over permissions. These are considered **best practice** for production environments.

---

### Inline Policies

Inline policies are embedded directly into a single IAM identity. They cannot be reused and are deleted automatically when the identity is removed. Inline policies are useful when permissions must remain tightly coupled to one specific identity.

---

### Resource-Based Policies

Resource-based policies are attached directly to AWS resources such as S3 buckets, SQS queues, or KMS keys. These policies explicitly define which principals are allowed access and support **cross-account access without role assumption**.

---

### Permission Boundaries

Permission boundaries define the **maximum permissions** an IAM user or role can have. They do not grant permissions on their own but restrict what permissions can be effective, even if broader policies are attached. Permission boundaries are commonly used to prevent privilege escalation in delegated administration scenarios.

---

## 5. IAM Permission Evaluation Logic

IAM uses a layered permission evaluation process. All requests start with an implicit deny. AWS evaluates permissions across multiple policy types, and **any explicit deny at any layer results in a final deny**.

For access to be granted, every applicable policy layer must explicitly allow the action.

---

## 6. Identity-Based vs Resource-Based Authorization

Identity-based policies grant permissions to IAM users, groups, or roles and are evaluated based on the identity making the request. Resource-based policies grant permissions directly on the resource and specify who can access it.

In cross-account scenarios, such as S3 access, **both an identity-based policy and a resource-based policy are typically required**.

---

## 7. IAM Roles in Practice

### EC2 Instance Roles

EC2 instance roles are delivered through an instance profile and allow EC2 instances to retrieve temporary credentials automatically. These credentials rotate automatically and eliminate the need to store access keys on the instance.

---

### Cross-Account Roles

Cross-account roles enable users or services in one AWS account to access resources in another account securely. This requires a trust policy in the target account and permission to call sts:AssumeRole in the source account.

---

## 8. AWS STS and Temporary Credentials

AWS Security Token Service (STS) issues temporary credentials when a role is assumed. These credentials include an access key, secret key, and session token, and they automatically expire.

Role sessions default to one hour and can be extended up to twelve hours. When chaining roles, the final session duration is limited to one hour.

---

## 9. Federation and External Identities

Federation allows external identities to authenticate using existing identity providers and assume IAM roles. Common federation mechanisms include SAML 2.0 for enterprise SSO and web identity federation using Amazon Cognito.

AWS IAM Identity Center provides a centralized, AWS-managed solution for workforce authentication and authorization.

---

## 10. Security Best Practices

The root account has unrestricted access and should only be used for account-level tasks. MFA must be enabled on the root account, and root credentials should never be used for daily operations.

IAM users with console access should use MFA, and AWS services should always authenticate using roles instead of long-term credentials.

---

## 11. IAM Security and Audit Tools

IAM provides tools such as the Credentials Report for auditing user credentials, Access Advisor for identifying unused permissions, and the Policy Simulator for testing permissions safely. AWS CloudTrail records all IAM-related API activity for auditing and investigation.

---

## 12. Service Control Policies (SCPs)

Service Control Policies are part of AWS Organizations and define the maximum permissions allowed within an account or organizational unit. SCPs apply to all identities in an account, including the root user, but do not grant permissions themselves.

---

## 13. Interview-Grade Summary

IAM secures AWS environments by using identities, policies, and temporary credentials to enforce least privilege. Users represent people, roles represent temporary access, and policies define permissions. Explicit denies always override allows, and roles should be used whenever AWS services or cross-account access is involved.

If credentials are permanent, the design is usually wrong. If AWS services need access, IAM roles are the correct solution.
