# AWS IAM - Identity and Access Management
## Authentication & Authorization
`Authentication` is the process of verifying user's identity.  
Authentication uses credentials such as usernames and passwords, multifactor authentication (MFA), or biometric data to confirm that the user is who they claim to be.

`Authorization` is the process of determining user's permissions and what actions they are allowed to perform after their identity has been authenticated.  
Authorization uses policies, roles and permissions to define what resources a user can access and what operations they can perform on those resources.

**Note:** Authentication takes place first, followed by Authorization.  
For example, when you log into your AWS account, AWS checks your credentials (authentication). Once authenticated, AWS checks what resources and actions you are allowed to access based on your permissions (authorization).

## What is IAM?  
IAM is an AWS service that allows you to create and manage identities (users, groups, and roles), and define permissions to control access to AWS resources.  
In short, IAM handles authentication and authorization for users, applications, and services interacting with AWS resources.  

## Key Concepts of IAM
1. `Users` Individual identities that represent people or services needing access to AWS resources. Each user has a unique set of credentials (password, access keys, MFA) to log in to AWS account. (This is Authentication).  

2. `Policies` These are the **permissions** that define what actions are allowed or denied for users or groups. Policies are written in JSON format and specify the resources and actions that can be performed. (This is Authorization).  

3. `Groups` Groups are collections of users that share the same permissions.
They exist to simplify permission management. Instead of assigning permissions to each user individually, you assign permissions/policies to a group (such as developers, QA, DBA, admin etc.) & add users to a specific group. Users inherit permissions from their group.  
  
    ```
    Important rules:  
    - Groups contain only users  
    - Groups cannot contain other groups  
    ```

4. `Roles`  These are similar to users with specific permissions, but they are not associated with a specific person or service. Instead, roles are assumed by trusted entities (users, applications, or services) to gain temporary access to AWS resources.  
They are identities that can be assumed. 
Roles provide temporary security credentials (Access key ID, Secret access key, Session token) for accessing AWS resources and are often used for cross-account access or when granting permissions to AWS services or applications running on AWS resources.  

**Note:** In modern best practice, applications should rarely use IAM users. They should use roles instead.  

## Assume Role Overview
Assume role is an AWS IAM security process where a user, application, or service obtains temporary security credentials (access key, secret key, and session token) to access resources. These short-term credentials, often used via the AWS CLI or SDK, allow for secure, delegated access without sharing long-term credentials.  
When a role is assumed, it grants the permissions defined in its policies to the entity assuming the role.  

**Steps to create and use an IAM Role:**
1. Create an IAM Role with the necessary permissions (policies) to access specific AWS resources. You must attach policies to the role defining what actions are allowed or denied.  
2. Attach the IAM Role to the entity (for example, attach the role to an EC2 instance, lambda function, or the user to assume the role). 
3. The entity then assumes the role and performs actions on AWS resources using the permissions defined in the role's policies.  

**How Assume Role works:**
1. An entity (user, application or service) calls the `sts:AssumeRole` API (when using SDK) or uses the AWS Management Console to request to assume a role.  
    ```json
    {
        "Version":"2012-10-17",		 	 	 
        "Statement": {
            "Effect": "Allow",
            "Action": "sts:AssumeRole",                           // Action to assume a role. STS (Security Token Service) is used to create and provide trusted users with temporary security credentials that can control access to your AWS resources.
            "Resource": "arn:aws:iam::111122223333:role/Test*"      // Specify the role ARN to assume. Role ARN is the Amazon Resource Name of the role.
        }
    }
    ```
  
2. AWS STS (Security Token Service) verifies the request by checking:    
    - Does the caller have permission to assume the role?  
    - Does the role trust the caller?  
  
3. AWS STS (Security Token Service) issues temporary credentials (access key ID, secret access key, and session token) to the entity.  
  
4. The entity uses those credentials to access AWS resources based on the role's permissions.  
  
5. Credentials expire (15 minutes to several hours). The entity must re-assume the role to obtain new credentials if needed.  
  
**For example:**  
❌ Bad practice: EC2 instance (virtual server) or a Lambda function using an IAM user (long-term credentials) to access S3 bucket is not secure. Anyone who gains access to those credentials can misuse them.  
✔️ Best practice: Instead, you create an IAM role with the necessary policies to access the S3 bucket. Attach that role to the EC2 instance. The EC2 instance assumes the role and obtain temporary security credentials to access the S3 bucket securely.  


# AWS Federated Authentication (SSO Login Without AWS Credentials)
Users can log in to AWS Management Console or access AWS resources via CLI/SDK using either
1. `Root user`: The email and password used to create the AWS account.  (Never recommended for use)
2. `IAM User`: A user created directly in AWS IAM with a username and password. (Not recommended for modern applications)
3. `IAM Role`: A role created in AWS IAM that can be assumed by trusted entities (users, applications, or services) to gain temporary access to AWS resources. (Recommended for modern applications)
4. `Federated User`: A user authenticated via an external identity provider (IdP) such as Azure AD, Okta, or Google Workspace, and mapped to an IAM Role in AWS. (Recommended for enterprise access)
5. `AWS SSO User`: A user managed through AWS Single Sign-On service, which can also be integrated with external identity providers. (Recommended for enterprise access)  

## Overview
In organizations, usually users do not log in to AWS using:
    - Root user credentials
    - IAM usernames and passwords

Instead, authentication is handled through **Federated Single Sign-On (SSO)** using the company’s identity system.
This approach is the industry standard for enterprise AWS access.

## What happens behind the scenes?
Users authenticate through the company’s **Identity Provider (IdP)** (such as Azure AD, Okta, or Google Workspace), and AWS trusts this provider.
The flow is:
`User → Company Identity System → AWS`
and not:
`User → AWS` directly.

## Key Concepts
1. `Federated Identity`  
A federated identity means AWS does not manage user passwords. AWS relies on an external identity system to validate users.


2. `Identity Provider (IdP)`
This is the system that actually authenticates the user, for example:  
   - Azure AD (Entra ID)
   - Google Workspace
   - On-prem Active Directory (via ADFS)


3. `IAM Roles (Not Users)`
Users are mapped to **IAM Roles**, not IAM Users.  
  
Roles define:  
    - What AWS account can be accessed  
    - What permissions are granted  


4. `Temporary Credentials`
After successful authentication, AWS generates:  
   - Access Key  
   - Secret Key  
   - Session Token  

These are valid only for a limited duration (typically 1–12 hours). No permanent AWS credentials exist for the user.  

## Authentication Flow
1. User clicks company AWS login link (e.g. `https://company.awsapps.com/start`)
2. User authenticates with corporate credentials
3. Identity Provider sends a signed token to AWS
4. User selects AWS account and role
5. AWS STS issues temporary session credentials
6. User gains console or CLI access

## What the User Actually Is in AWS
Users are **federated identities assuming IAM roles**.  
They are NOT Root users or IAM users. They are Temporary role sessions.  

You can verify this in the AWS Console. Top-right menu → shows:  
Federated User: user@company.com  

## Benefits of Federated Authentication:
- Centralized user management
- Easy onboarding/offboarding
- No password to manage in AWS & no risk of password leak
- Enforced MFA via corporate IdP
- Role-based access control

> This model is known as `Federated Identity with Role-Based Access Control (RBAC)`