# Challenge 1

## Challenge Statement
We all know that public buckets are risky. But can you find the flag?



## IAM Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::thebigiamchallenge-storage-9979f4b/*"
        },
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::thebigiamchallenge-storage-9979f4b",
            "Condition": {
                "StringLike": {
                    "s3:prefix": "files/*"
                }
            }
        }
    ]
}
```
### write a short analysis about the IAM policy here
```
* What do I have access to?
    We have access to the list of objects in the "files/*" directory of the bucket.
    We can download any objects inside the bucket

* What don't I have access to?
    We cannot write, delete, modify objects in the bucket.

* What do I find interesting?
    The bucket is open to the public, allowing unrestricted download access to its contents, which is a security risk. The condition limits listing to a specific folder, but downloading objects isn't restricted.
* etc
```

## Solution
Detailed steps to the flag
    
    Use AWS CLI or a tool like "aws s3 ls s3://thebigiamchallenge-storage-9979f4b/files/" to list the files.  
    List the objects and identify the files that might contain the flag.  
    Use "aws s3 cp" command to copy the files to the terminal and analyze them for the flag.  


## Reflection
* What was your approach?
    
    I have learned about bucket's permissions and utilized the actions to list and download objects.

* What was the biggest challenge?

    Found the specific location of the flag among the list of files.

* How did you overcome the challenges?

    Systematically listing and downloading files to identify the flag.
* What led to the break through?

    Public read access enabled straightforward exploration.
* On the blue side, how can the learning be used to properly defend the important assets? 


    Avoid public permissions on S3 buckets unless necessary. Implement least privilege principles and monitor bucket access logs for unusual activity.