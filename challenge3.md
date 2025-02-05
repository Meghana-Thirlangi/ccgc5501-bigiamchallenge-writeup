# copy the contents of sample.md to start
# Challenge number 3

## Challenge Statement
The challenge statement that you are working on 

Enable push notifications. We sent you a message.
## IAM Policy
```json
copy and paste the IAM policy from the challenge here
```
{
    "Version": "2008-10-17",
    "Id": "Statement1",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "SNS:Subscribe",
            "Resource": "arn:aws:sns:us-east-1:092297851374:TBICWizPushNotifications",
            "Condition": {
                "StringLike": {
                    "sns:Endpoint": "*@tbic.wiz.io"
                }
            }
        }
    ]
}
### write a short analysis about the IAM policy here
```
* What do I have access to?
* What don't I have access to?
* What do I find interesting?
* etc
```

## Solution
Detailed steps to the flag

1.  Found a command on aws sns cli website for subcribing a topic
2. Run the command with the email id provided in the IAM policy
3. Use request catcher to confirm the request sent with the subscribe url.
4. Then we will be seing the flag value in the message section.

## Reflection
* What was your approach?

    As I didn't have any idea about what is this challenge about I started analyzing the IAM policy then I see "Action" section says sns:subscribe. So I have gone through the aws sns documentation. Following the available commands, gone through the subscribe command document where I could find the command for subscribing to a topic which included protocol, topic and terms like that. That way I found flag.
* What was the biggest challenge?

    My biggest challenge was, after finding and running the command the notification was sent to the domain that was mentioned in the IAM policy. To retrive the flag value we need to confirm the request that sent to the domain. But I had no idea how to view the request to confirm it. Finding a way through it was a challenge for me.  
* How did you overcome the challenges?

    Ater a lot of research on how to catch the request in other way rather than email protocol, we got to know that there is a https protocol i.e., request catcher where we can push the notifications to this protocol and confirm them and with an URL.
* What led to the break through?
    
    The subscribe command on the sns aws cli website
* On the blue side, how can the learning be used to properly defend the important assets? 
    1. Strengthening IAM and Least Privilege Access
    2. Securing AWS Resources Against Unauthorized Access
    3. Detecting and Responding to Security Incidents
    4. Hardening Networking & AWS Infrastructure