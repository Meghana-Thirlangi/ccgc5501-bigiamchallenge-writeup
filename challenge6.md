# copy the contents of sample.md to start
# Challenge number

## Challenge Statement
Cloudfoxable - Do this first!

The goal of this challenge is to set up CloudFoxable, a Capture The Flag environment designed for cloud security testing. This requires setting up an AWS account, configuring Terraform, deploying CloudFoxable, and finding the first flag hidden in the Terraform output. 

## IAM Policy
```json

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstances",
                "iam:ListUsers",
                "s3:ListBucket",
                "s3:GetObject"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Deny",
            "Action": [
                "iam:DeleteUser",
                "s3:DeleteObject",
                "ec2:TerminateInstances"
            ],
            "Resource": "*"
        }
    ]
}

### write a short analysis about the IAM policy here
```
* What do I have access to?
    The policy allows users to view EC2 instances, IAM users, and S3 bucket details.
    It also allows users to read objects from an S3 bucket.

* What don't I have access to?
    Users cannot delete IAM users.
    They cannot remove objects from an S3 bucket.
    They cannot terminate EC2 instances.

* What do I find interesting?
    The policy is designed for read-only access, meaning users can see but not modify resources.
    The restrictions prevent accidental or intentional deletion of important data
* etc
```

## Solution
Detailed steps to the flag

Step 1: 

Created an AWS account specifically for this challenge.

Created a new IAM user with admin permissions instead of using the root account.
Generated and securely stored access keys for this user.

Step 2:

Installed the AWS Command Line Interface (CLI) on the system.
Configured AWS credentials using:
    
    aws configure

Verified the setup by checking the currently logged-in user:

    aws sts get-caller-identity

Step 3: 

Installed Terraform and added it to the system’s environment variables.
Cloned the CloudFoxable repository:

    git clone https://github.com/BishopFox/cloudfoxable
    cd cloudfoxable/aws

Copied and modified the Terraform variables file:

    cp terraform.tfvars.example terraform.tfvars

Opened the terraform.tfvars file and set the AWS profile:


    aws_local_profile = "YOUR_PROFILE"

Initialized Terraform:

    terraform init
    terraform plan
    terraform apply

Step 4:

After Terraform completed deployment, the output displayed some important information.

The output included credentials for the ctf-starting-user.
Configured AWS CLI to use this new user:

    aws configure --profile cloudfoxable

Verified the user switch with:

    aws sts get-caller-identity --profile cloudfoxable

Carefully reviewed the Terraform output and found the first flag:

    FLAG{congrats_you_are_now_a_terraform_expert_happy_hunting}

Submitted the flag to the CTF platform for validation.


## Reflection
* What was your approach?
    
    Followed the official CloudFoxable setup guide step by step.

    Ensured security best practices by using a separate IAM user instead of the root account.

    Carefully reviewed the Terraform output to locate the flag.
* What was the biggest challenge?
    
    Initially missed the flag in the Terraform output.
* How did you overcome the challenges?

    Re-ran Terraform and carefully analyzed the output to ensure no details were overlooked.
* What led to the break through?

    Realized that Terraform outputs important credentials after deployment.
    
    Used aws sts get-caller-identity to confirm which AWS user was currently active.
* On the blue side, how can the learning be used to properly defend the important assets? 

    Limit permissions: Ensure Terraform users only have necessary permissions.
    
    Monitor output logs: Avoid exposing sensitive credentials in Terraform output.
