# AWS IAM

## Part 1: Core Concepts (Foundation)

### What is IAM?
**Identity and Access Management** - A global service controlling authentication (who you are) and authorization (what you can do) in AWS. Free to use, no region selection required.

### The Four Pillars

**1. Users**
- Represent individual people or applications
- One physical person = one IAM user (best practice)
- Access methods: Console (password) or Programmatic (access keys)
- Can belong to multiple groups
- Default: no permissions (implicit deny)

**2. Groups**
- Collections of users with shared permissions
- Simplifies permission management at scale
- Groups CANNOT contain other groups
- Groups CANNOT be nested
- Users can belong to 0 to 10 groups

**3. Roles**
- Temporary identities assumed by users, applications, or AWS services
- NO permanent credentials (uses temporary security credentials via STS)
- Common use: EC2 accessing S3, Lambda accessing DynamoDB
- Has two policy types: Trust Policy (who can assume) + Permission Policy (what they can do)

**4. Policies**
- JSON documents defining permissions
- Attach to users, groups, or roles
- Follow principle of least privilege

---

## Part 2: Deep Dive - IAM Policies

### Policy Structure Explained

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "OptionalStatementID",
      "Effect": "Allow",
      "Principal": "arn:aws:iam::123456789012:user/Alice",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::my-bucket/*",
      "Condition": {
        "IpAddress": {
          "aws:SourceIp": "203.0.113.0/24"
        }
      }
    }
  ]
}
```

**Elements Breakdown:**
- **Version**: Always use "2012-10-17" (latest version)
- **Sid**: Optional statement identifier for documentation
- **Effect**: Allow or Deny (Deny always wins)
- **Principal**: Who the policy applies to (used in resource-based policies)
- **Action**: API calls allowed/denied (supports wildcards like s3:*)
- **Resource**: ARN of resources the actions apply to
- **Condition**: Optional circumstances for policy enforcement

### Policy Types - Know the Differences

**AWS Managed Policies**
- Created and maintained by AWS
- Automatically updated when AWS releases new services/features
- Examples: AdministratorAccess, ReadOnlyAccess, PowerUserAccess
- Cannot be modified
- Shared across all AWS customers
- Use when: Standard use cases, quick setup needed

**Customer Managed Policies**
- Created and maintained by you
- Reusable across multiple users/groups/roles in YOUR account
- Version controlled (up to 5 versions)
- Can be modified anytime
- Use when: Custom permissions needed, want to follow least privilege precisely

**Inline Policies**
- Embedded directly into a single user, group, or role
- Strict 1:1 relationship
- Deleted when the identity is deleted
- Cannot be reused
- Use when: Want to ensure permissions are never accidentally applied to other identities

**Resource-based Policies**
- Attached to resources (S3 buckets, SQS queues, KMS keys), not identities
- Specify who (Principal) can access the resource
- Support cross-account access WITHOUT assuming a role
- Use when: Granting access to specific resources, cross-account scenarios

**Permission Boundaries**
- Advanced feature setting maximum permissions
- Doesn't grant permissions itself, only limits them
- Applied to users or roles
- Use when: Delegating admin tasks, ensuring developers can't escalate privileges

### Permission Evaluation Logic - THE CRITICAL PART

**The Decision Flow:**
1. **Default**: Implicit Deny (no access)
2. Check for **Explicit Deny** → If found, DENY (game over)
3. Check for **Explicit Allow** → If found and no Explicit Deny exists, ALLOW
4. If no Explicit Allow found → **Implicit Deny** (default to no access)

**Key Rule**: Deny ALWAYS wins. One Deny overrides 100 Allows.

**Evaluation Order for Multiple Policy Types:**
1. Organization SCPs (if using AWS Organizations)
2. Resource-based policies
3. Identity-based policies
4. IAM permission boundaries
5. Session policies (for temporary credentials)

**Result**: The MOST RESTRICTIVE policy wins.

### Action vs NotAction, Resource vs NotResource

**Action vs NotAction:**
```json
// Allow specific actions
"Action": ["s3:GetObject", "s3:PutObject"]

// Allow everything EXCEPT these actions
"NotAction": ["s3:DeleteBucket"]
```

**Critical**: NotAction with Allow means "allow everything except these". NotAction with Deny means "deny everything except these".

**Common Exam Trap**: NotAction doesn't equal Deny. Using NotAction with Effect: Allow can grant broad permissions.

---

## Part 3: IAM Roles - Deep Dive

### Role Components

**Trust Policy (AssumeRolePolicyDocument)**
- Defines WHO can assume the role
- Specifies the Principal (service, account, user, federated user)
- This is NOT a permission policy

**Permission Policy**
- Defines WHAT the role can do once assumed
- Same as regular IAM policies
- Attached to the role

### Common Role Use Cases

**1. EC2 Instance Roles (via Instance Profiles)**
```
EC2 Instance → Instance Profile → IAM Role → Permissions
```
- EC2 gets temporary credentials automatically
- Credentials auto-rotate
- No need to store access keys on EC2
- **Exam favorite**: ALWAYS use roles for EC2, never hardcode credentials

**2. Cross-Account Access**
```
Account A (Dev) → Assumes Role → Account B (Prod)
```
- Trust policy in Account B allows Account A users to assume role
- Users in Account A need permission to sts:AssumeRole
- **Two-step permission**: Permission to assume + Trust to be assumed

**3. Service Roles**
- Allow AWS services to perform actions on your behalf
- Examples: Lambda execution role, ECS task role
- Each service has specific trust policy requirements

**4. Web Identity Federation (Cognito)**
- External users (Google, Facebook, SAML) authenticate
- Cognito exchanges token for temporary AWS credentials via STS
- Role assumed with limited permissions

### AssumeRole Mechanics

**Process:**
1. Entity calls sts:AssumeRole API
2. STS validates trust policy
3. STS issues temporary credentials (Access Key, Secret Key, Session Token)
4. Credentials expire (default 1 hour, max 12 hours for roles, max 36 hours for federated)
5. Entity uses temporary credentials to access AWS resources

**Role Chaining**: Assuming a role from within another assumed role (max 1 hour sessions, limited to 1 hop in some scenarios).

---

## Part 4: Security Best Practices - Exam Critical

### Root Account Security

**What is Root?**
- Email + password used to create AWS account
- Complete unrestricted access to ALL AWS services and resources
- Cannot be restricted by policies

**Best Practices:**
- Enable MFA immediately
- Delete root access keys if they exist
- Create admin IAM user for daily tasks
- Use root only for: changing account settings, closing account, changing support plan, certain billing operations
- Store root credentials in secure vault

### MFA (Multi-Factor Authentication)

**Types:**
- Virtual MFA devices (Google Authenticator, Authy)
- Hardware MFA tokens (Gemalto, YubiKey)
- U2F security keys (YubiKey)

**Where to Enable:**
- Root account (mandatory in practice)
- IAM users with console access
- For privileged operations (can enforce via policy Condition)

**Policy Enforcement Example:**
```json
{
  "Effect": "Deny",
  "Action": "*",
  "Resource": "*",
  "Condition": {
    "BoolIfExists": {
      "aws:MultiFactorAuthPresent": "false"
    }
  }
}
```

### Access Keys Management

**Structure:**
- Access Key ID: Public identifier (like username)
- Secret Access Key: Secret (like password) - shown only once

**Best Practices:**
- Never share or commit to code repositories
- Rotate regularly (90 days recommended)
- Delete unused keys
- Use roles instead of access keys wherever possible
- Enable "Require MFA for API calls" for sensitive operations
- Monitor usage with IAM Access Advisor

### Password Policy Configuration

**Enforceable Requirements:**
- Minimum length (default 8, recommend 14+)
- Require uppercase, lowercase, numbers, symbols
- Password expiration (30, 60, 90 days)
- Prevent password reuse (remember last 5-24 passwords)
- Allow users to change own password
- Require admin reset if password expires

### Principle of Least Privilege

**Implementation Strategy:**
1. Start with zero permissions
2. Grant minimum permissions needed for the task
3. Use AWS managed policies for common patterns
4. Create custom policies for specific needs
5. Review and reduce permissions over time
6. Use Access Advisor to identify unused permissions

---

## Part 5: Advanced IAM Concepts

### Permission Boundaries

**What They Do:**
- Set maximum permissions an entity can have
- Don't grant permissions themselves
- Even if user has AdministratorAccess policy, boundary limits what they can actually do

**Use Case - Delegated Administration:**
```
Developer → Permission Boundary (can't touch billing/IAM) → Creates resources within boundary
```

**Exam Scenario**: Company wants developers to create IAM roles for Lambda but prevent privilege escalation.

**Solution**: Apply permission boundary to developer's user account limiting what permissions they can grant to roles they create.

### Service Control Policies (SCPs)

**Part of AWS Organizations, not IAM, but important:**
- Apply to entire AWS accounts
- Set maximum permissions for ALL users/roles in account
- Root user is also affected
- Don't grant permissions, only restrict

**Difference from Permission Boundaries:**
- SCPs: Account level, affect everyone including root
- Permission Boundaries: Individual IAM entities

### Identity vs Resource-Based Policies

**Identity-Based:**
- Attached to users, groups, roles
- Don't specify Principal (implied by attachment)
- Used for: Granting permissions to your identities

**Resource-Based:**
- Attached to resources (S3 bucket, SQS queue, KMS key, Lambda)
- Must specify Principal
- Used for: Cross-account access, service-to-service access

**Critical Exam Concept - Cross-Account S3 Access:**

Requires BOTH:
1. IAM policy allowing s3:GetObject in source account
2. S3 bucket policy allowing Principal from source account

Missing either one = Access Denied

### Federation and External Identity

**SAML 2.0 Federation (Enterprise):**
- Corporate users authenticate with on-premises Active Directory
- SAML assertion exchanged for temporary AWS credentials
- Users assume IAM role via STS
- Use case: Enterprise SSO to AWS Console/API

**Web Identity Federation (Mobile/Web Apps):**
- Users authenticate with Google, Facebook, Amazon
- Cognito exchanges token for AWS credentials
- Temporary credentials with limited permissions
- Use case: Mobile apps accessing AWS resources

**Amazon Cognito:**
- User pools: User directory with sign-up/sign-in
- Identity pools: Provide AWS credentials to access AWS services
- Supports unauthenticated identities (guest access)

---

## Part 6: IAM Tools and Monitoring

### IAM Credentials Report (Account-Level)

**What it shows:**
- List of all IAM users in account
- Status of passwords (enabled, last used, last changed)
- Access keys status
- MFA device status
- When user was created

**Use for:**
- Compliance auditing
- Security reviews
- Identifying inactive users
- Credential rotation tracking

**How to generate**: IAM Console → Credential Report → Download (CSV)

### IAM Access Advisor (User-Level)

**What it shows:**
- Services the user/role is allowed to access
- When those services were last accessed
- Granular permission usage data

**Use for:**
- Implementing least privilege
- Identifying unused permissions
- Refining policies based on actual usage
- Audit trail for permission reviews

**Exam tip**: Question asks "how to reduce permissions" → Use Access Advisor to see what's actually being used.

### AWS CloudTrail Integration

- Logs all IAM API calls (CreateUser, DeleteRole, PutUserPolicy)
- Provides audit trail of who did what and when
- Essential for security investigations and compliance
- Can identify unauthorized access attempts

---

## Part 7: Common Exam Scenarios and Solutions

### Scenario 1: EC2 Needs to Access S3

**Wrong Approach:**
- Store access keys on EC2 instance
- Hardcode credentials in application

**Correct Approach:**
1. Create IAM role with S3 permissions
2. Attach role to EC2 instance (via instance profile)
3. Application uses AWS SDK which automatically retrieves temporary credentials
4. Credentials rotate automatically

**Why**: Roles provide temporary credentials, no secret management needed, follows security best practices.

### Scenario 2: Cross-Account Resource Access

**Setup**: Account A needs to access S3 bucket in Account B

**Solution**:
1. Account B creates IAM role with trust policy allowing Account A
2. Account B role has permission policy to access S3 bucket
3. Account A user/role needs permission to sts:AssumeRole
4. Account A assumes role in Account B
5. Gets temporary credentials to access S3

**Alternative for S3 specifically**:
- Use S3 bucket policy in Account B granting access to Account A principal
- No role assumption needed

### Scenario 3: Developer Privilege Escalation Prevention

**Problem**: Developers can create IAM roles but shouldn't be able to grant themselves admin access.

**Solution**: Use Permission Boundaries
1. Create permission boundary policy limiting maximum permissions (e.g., no IAM/billing access)
2. Attach boundary to developer users
3. Developers can create roles/users but they'll inherit the same boundary
4. Even if developer creates role with AdministratorAccess, boundary limits actual permissions

### Scenario 4: Temporary Access for External Consultant

**Wrong Approach:**
- Create IAM user with password
- Share credentials via email

**Correct Approach:**
1. Create IAM role with required permissions
2. Add consultant's AWS account to trust policy (if they have AWS account)
3. OR use federation with their corporate IdP
4. Consultant assumes role to get temporary credentials
5. Access expires automatically
6. Delete role when engagement ends

### Scenario 5: Mobile App Backend Access

**Solution**: Use Amazon Cognito
1. Users authenticate with Cognito User Pool (or social provider)
2. Cognito Identity Pool exchanges token for temporary AWS credentials
3. Credentials have limited permissions (read own data only)
4. Use policy variables to restrict access: `"Resource": "arn:aws:s3:::bucket/${cognito-identity.amazonaws.com:sub}/*"`

### Scenario 6: Enforce MFA for Sensitive Operations

**Solution**: IAM Policy with Condition
```json
{
  "Effect": "Deny",
  "Action": "ec2:TerminateInstances",
  "Resource": "*",
  "Condition": {
    "BoolIfExists": {
      "aws:MultiFactorAuthPresent": "false"
    }
  }
}
```

This denies instance termination unless MFA was used during authentication.

### Scenario 7: Grant S3 Access Without Changing Bucket Policy

**Problem**: Third-party account needs access to your S3 bucket but you can't modify bucket policy.

**Solution**: 
- This is NOT possible
- Cross-account S3 access requires BOTH identity-based policy AND resource-based (bucket) policy
- Exception: If bucket owner is same as object owner, bucket policy alone can grant access

---

## Part 8: Common Anti-Patterns and Mistakes

### ❌ Don't Do This

**1. Using Root Account for Daily Tasks**
- Root cannot be restricted
- Compromise = complete account takeover
- Always use IAM users/roles

**2. Hardcoding Access Keys**
- Keys in code → exposed in version control
- Keys on servers → can be stolen
- Use roles instead

**3. Sharing IAM User Credentials**
- Cannot track individual actions
- Password changes affect all users
- Create individual IAM users

**4. Overly Permissive Policies**
- Using `"Action": "*"` on `"Resource": "*"`
- Granting more than needed
- Increases blast radius of compromise

**5. Never Rotating Credentials**
- Compromised keys remain valid indefinitely
- Rotate every 90 days
- Delete unused keys

**6. Ignoring MFA**
- Password-only authentication is weak
- Enable MFA for all console users
- Especially critical for privileged accounts

**7. Inline Policies Everywhere**
- Cannot reuse
- Hard to maintain
- Prefer managed policies

**8. Not Using Groups**
- Attaching policies to individual users
- Becomes unmanageable at scale
- Use groups for permission management

---

## Part 9: IAM Limits (Know for Exam)

### Default Limits (Can request increases for most)

- **Users per account**: 5,000
- **Groups per account**: 300
- **Roles per account**: 1,000
- **Customer managed policies per account**: 1,500
- **Groups per user**: 10
- **Managed policies per user/group/role**: 10 (AWS managed + customer managed combined)
- **Inline policy size**: 2,048 characters for users, 5,120 for groups, 10,240 for roles
- **Access keys per user**: 2 (for rotation)

### Important Non-Configurable Limits

- **Policy versions**: 5 per customer managed policy
- **Role session duration**: 1 hour to 12 hours
- **Federated user session**: Up to 36 hours

---

## Part 10: Quick Reference - Exam Tips

### When to Use What

| Scenario | Solution |
|----------|----------|
| AWS service needs permissions | IAM Role |
| EC2 accessing other AWS services | IAM Role (Instance Profile) |
| Lambda function needs permissions | IAM Role (Execution Role) |
| Cross-account access | IAM Role + Trust Policy |
| External users (mobile app) | Cognito + IAM Role |
| Enterprise SSO | SAML Federation + IAM Role |
| Prevent privilege escalation | Permission Boundaries |
| Restrict entire account | Service Control Policies (SCPs) |
| Grant S3 bucket access | Bucket Policy + IAM Policy |
| Audit credential usage | IAM Credentials Report |
| Identify unused permissions | IAM Access Advisor |
| Temporary credentials | STS AssumeRole |

### Red Flags in Exam Questions

- "Hardcode credentials" → WRONG, use roles
- "Share IAM user" → WRONG, create individual users
- "Root account for daily tasks" → WRONG, use IAM users
- "Store access keys on EC2" → WRONG, use instance roles
- "Permanent credentials for mobile app" → WRONG, use Cognito + temporary credentials

### Policy Troubleshooting Checklist

1. Is there an explicit Deny? (Deny wins always)
2. Is there an explicit Allow? (Needed for access)
3. Are you checking the right policy type? (Identity vs Resource)
4. Is MFA required but not present?
5. Is IP restriction blocking access?
6. Are SCPs or Permission Boundaries limiting access?
7. For S3: Do both IAM policy AND bucket policy allow access?

---

## Part 11: Practice Questions Style Examples

**Q1**: A company wants developers to create Lambda functions but prevent them from creating functions with admin permissions. What should you implement?

**Answer**: Permission Boundaries. Apply a permission boundary to developer IAM users that limits the maximum permissions they can grant to Lambda execution roles.

---

**Q2**: An EC2 instance needs to upload files to S3. What is the most secure approach?

**Answer**: Create an IAM role with S3 write permissions and attach it to the EC2 instance. The instance will receive temporary credentials automatically.

---

**Q3**: You need to grant a third-party AWS account access to objects in your S3 bucket. How do you accomplish this?

**Answer**: Create an IAM role in your account with S3 permissions and a trust policy allowing the third-party account to assume it. OR add a bucket policy to the S3 bucket granting access to the third-party account principal.

---

**Q4**: How can you identify IAM users who haven't used their permissions in 90 days?

**Answer**: Use IAM Access Advisor to see when permissions were last used, or generate an IAM Credentials Report to see last activity dates.

---

**Q5**: A user has AdministratorAccess policy attached but still cannot access billing information. Why?

**Answer**: Billing access must be explicitly activated for IAM users in the account settings by the root user, even if they have admin permissions.

---

## Summary: Your Exam Day Mindset

**Remember These Core Principles:**
1. Deny always wins
2. Roles over access keys for AWS services
3. Least privilege is the goal
4. Temporary credentials are better than permanent
5. Trust policies define WHO, permission policies define WHAT
6. Cross-account S3 needs both IAM policy and bucket policy
7. Root account usage = bad practice
8. MFA everywhere for humans
9. Groups simplify management
10. Access Advisor helps implement least privilege

**You're ready when you can:**
- Explain the difference between users, groups, and roles
- Write basic IAM policies in JSON
- Troubleshoot permission issues using evaluation logic
- Choose the right authentication method for different scenarios
- Implement cross-account access
- Secure AWS accounts following best practices


