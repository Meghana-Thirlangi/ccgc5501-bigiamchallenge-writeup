# copy the contents of sample.md to start
Step-by-Step Approach
1. Identifying the Active AWS User
Before proceeding, I confirmed the AWS identity being used:


aws --profile cloudfoxable sts get-caller-identity

The response validated that the user in action was ctf-starting-user:


{

    "UserId": "AIDAQWHCQAQBZIFG4SU4G",
    "Account": "047719646211",
    "Arn": "arn:aws:iam::047719646211:user/ctf-starting-user"

}
This ensured that all actions taken would align with the permissions assigned to this user.

2. Scanning for Hidden Secrets
Next, I leveraged CloudFox, a reconnaissance tool, to scan for any secrets stored in AWS SSM:

cloudfox aws -p cloudfoxable secrets -v2

This scan revealed a potential lead:


│ SSM            │ us-west-2 │ /cloudfoxable/flag/its-a-secret       │                                      │

This confirmed that a hidden flag was stored as a SecureString parameter in SSM.

3. Analyzing Access Permissions
To check if the current user had the required access to retrieve the secret, I ran:

cloudfox aws -p cloudfoxable permissions --principal ctf-starting-user -v2


 Extracting the Secret from AWS SSM

With the permissions validated, I executed the following command to fetch the secret:

aws --profile cloudfoxable --region us-west-2 ssm get-parameter --with-decryption --name /cloudfoxable/flag/its-a-secret

The response contained the hidden flag:


{

    "Parameter": {
        "Name": "/cloudfoxable/flag/its-a-secret",
        "Type": "SecureString",
        "Value": "FLAG{ItsASecret::IsASecretASecretIfTooManyPeopleHaveAccessToIt?}",
        "Version": 1,
        "LastModifiedDate": "2025-02-12T09:26:40.932000-05:00",
        "ARN": "arn:aws:ssm:us-west-2:047719646211:parameter/cloudfoxable/flag/its-a-secret",
        "DataType": "text"
    }
}

 Flag Successfully Retrieved:

FLAG{ItsASecret::IsASecretASecretIfTooManyPeopleHaveAccessToIt?}

Lessons Learned & Key Takeaways:

 Approach Taken
I followed a structured methodology:
 Confirm user identity
 Scan for secrets using CloudFox
 Validate access permissions
 Extract the hidden flag

 Challenges Faced
 Identifying the right permission set for ctf-starting-user
 Understanding how CloudFox structures its output
 Navigating AWS IAM policies to confirm access

 How I Overcame These Challenges
 CloudFox’s permissions module helped determine that ssm:GetParameter was allowed
 Cross-checking IAM policies ensured that I had read access to the required secret


Final Thoughts
This exercise provided valuable insight into AWS IAM permissions, secret management, and CloudFox reconnaissance. Understanding how secrets can be discovered and exploited emphasizes the need for strong access controls and security monitoring in cloud environments.
