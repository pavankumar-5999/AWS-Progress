# IAM Service Notes

## Introduction to IAM in AWS

### Overview

* IAM (Identity and Access Management) allows you to securely manage access to AWS services and resources.
* Provides granular control over who can access which resources and what actions they can perform.
* Centralized management of permissions, roles, and policies.

### Why IAM Matters

* Security: Protects AWS resources from unauthorized access.
* Compliance: Helps meet regulatory requirements.
* Best Practices: Least privilege principle, multi-factor authentication, and role-based access.

### Example Use Case

* A company wants developers to have read-only access to production databases but full access to development resources.
* IAM allows the creation of policies and roles to enforce this access control.

### IAM Use Case

* Grant temporary access to external contractors via IAM roles.
* Centralized user management for large teams.
* Logging and monitoring user activities for auditing purposes.

## AWS Account Root User

### Introduction

* The root user is created when you first create an AWS account.
* Has unrestricted access to all AWS resources and services.

### Best Practices

* Use root user only for account setup and billing purposes.
* Enable multi-factor authentication (MFA).
* Create IAM users with necessary permissions instead of using root.

### Discussion Question

* How can over-reliance on root user compromise account security?

  * Root credentials are highly sensitive; compromise risks total account control.

## Core IAM Features

### Introduction

* IAM core features enable secure and organized access management.

### IAM Users

* Represents a person or service needing access.
* Each user can have individual credentials: passwords, access keys.
* Can be grouped to simplify permission management.

### IAM Policies

* JSON documents defining permissions.
* Types: Managed Policies (AWS-managed, customer-managed), Inline Policies.
* Policies specify allowed or denied actions on resources.

### IAM Groups

* Collection of IAM users.
* Assign permissions to a group instead of individual users.
* Simplifies management for teams.

### IAM Roles

* Assignable to users, services, or applications.
* Provide temporary credentials with defined permissions.
* Useful for cross-account access, EC2 instances, Lambda functions.

### Multi-factor Authentication (MFA)

* Adds an extra security layer using a physical or virtual device.
* Strongly recommended for root users and privileged IAM users.
* Types: Virtual MFA (app-based), Hardware MFA.

## Knowledge Check Questions

* What is the difference between an IAM user and an IAM role?
* Why is it recommended to avoid using the root user for daily operations?
* How do IAM policies enforce the principle of least privilege?
* What are the benefits of using IAM groups?

## Conclusion

* IAM is essential for securing AWS resources.
* Use root user sparingly and enable MFA.
* Apply least privilege using users, groups, roles, and policies.
* Regularly audit and review permissions to maintain security.

---

These notes can be directly uploaded to GitHub and provide a clear overview of AWS IAM concepts relevant to the SAA exam.
