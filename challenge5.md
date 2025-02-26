# Challenge number

## Challenge Statement
The goal of this challenge was to set up a personal AWS account while ensuring strong security. This involved:

Adding Multi-Factor Authentication (MFA) to the root user
Creating an IAM admin user instead of using the root account for daily tasks
Enabling MFA for the IAM admin user
Applying security measures to protect the AWS account

### write a short analysis about the IAM policy here
```
* What do I have access to?
    
    Users can perform most IAM-related tasks except deleting IAM users, roles, or policies without MFA.

* What don't I have access to?

    If MFA is not enabled, users cannot delete IAM users, roles, or policies.

* What do I find interesting?

    It protects IAM resources by ensuring only users with MFA can make critical changes.
Prevents accidental or unauthorized deletions of IAM settings.
```

## Solution
Detailed steps to the flag

Step 1: Enable MFA for Root User

Logged into AWS root account.

Went to IAM → Security Credentials.
Enabled Multi-Factor Authentication (MFA) using an app like Google Authenticator.

Scanned the QR code and entered the generated code to complete the setup.

Confirmed that MFA was active.

Step 2: Create an IAM Admin User

Opened AWS IAM Dashboard.

Clicked on Users → Add User.

Created a new admin user with:

Programmatic access for CLI/API.
AWS Console access.

Assigned AdministratorAccess policy to this user.

Step 3: Enable MFA for IAM Admin User

Signed in as the IAM admin user.

Went to IAM → Users → Security Credentials.

Enabled MFA following the same process as the root user.

Applied an IAM policy to require MFA for sensitive actions.

## Reflection
Security Measures Taken & Why
MFA for Root and IAM Admin Users

Prevents unauthorized access if login credentials are stolen.
Not Using Root for Daily Tasks

The root user has full control of the account and is a security risk.
Instead, an IAM admin user is used for better security.
Restricting IAM Actions Without MFA

Ensures that deletions of IAM users, roles, and policies require MFA.
Enabling AWS Security Features

AWS GuardDuty for security monitoring.
AWS CloudTrail for tracking activity logs.

```

Why Use an Admin User When the Root User Exists?

Root User is Too Powerful: It can control everything, making it a risk.

AWS Best Practices: Root access should be limited and locked away.

Better Access Management: IAM admin users allow for controlled access and monitoring.

* Challenges Faced & Solutions:

Forgetting to Set Up MFA Early
Fixed by making it the first step in the setup.
Trouble Setting IAM Policy Correctly
Solved by testing the policy using IAM Policy Simulator.

Key Takeaways for Security
Never use the root account for everyday work.

Always enable MFA for important users.

Use least privilege access to control permissions.

Monitor AWS logs and set up security alerts.