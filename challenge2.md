## Challenge Statement
 We created our own analytics system specifically for this challenge. We think it's so good that we even used it on this page. What could go wrong?

Join our queue and get the secret flag.

## AWS SQS:

{
    "Messages": [
        {
            "MessageId": "c5045753-8421-49fe-87f6-5c4e6a43b40e",
            "ReceiptHandle": "AQEBMlqkrZzG+LdkFwFzo38Vclr0s3Iewxg0r3Lx1xQkNfuGv8QQwFoTlIpJWldybCuo8BFAMmV8dGydZKmk31hHw1Hxs
5zy6siLlM6f5U+ZxCekwJqGogTX/v7b86O/G+RWBUvl3BbAMaHQLM9EhopmpU8AogPtbhk54SnQmEylGyqGEp1613b0HM34y1FEbocJ34nnwHTqpSQjxXkuYjR9
SUX5Afaty5IyV1jrOehimyrIXv0t/nYdZ0dJfLilH6Kg07f+06JjlNWZxgP2/KZd2DvysoRVwGNoVSzy1FMvbUFcKnN9UJ+a5JKFIsbErqNjTH0VAzgxiEhDTej
CE3w2wt9xVLv/rZsC+9Yw9JnyBH2uUjGfhWOwZOyKa9pHB9xIAVSwiPnti/e/XTfRtwKMnAH1adAIWRrWXIvFBJM6Vlg=",
            "MD5OfBody": "4cb94e2bb71dbd5de6372f7eaea5c3fd",
            "Body": "{\"URL\": \"https://tbic-wiz-analytics-bucket-b44867f.s3.amazonaws.com/pAXCWLa6ql.html\", \"User-Agent
\": \"Lynx/2.5329.3258dev.35046 libwww-FM/2.14 SSL-MM/1.4.3714\", \"IsAdmin\": true}",
            "Attributes": {
                "SenderId": "AROARK7LBOHXGHGQ5XCT5:tbic-wiz-send-flag-to-sqs-8d265a4",
                "ApproximateFirstReceiveTimestamp": "1738170985383",
                "ApproximateReceiveCount": "1",
                "SentTimestamp": "1738167688509",
                "AWSTraceHeader": "Root=1-679a5588-54d15f1e6d10bfd31a424550;Parent=3b9ed10510d9cf9b;Sampled=0;Lineage=1:037
bed70:0"
            }
        }
    ]
}

## Please create a write-up for bigiamchallenge.com's challenge 2.  You need to be able to describe how you got the the flag.  Write as if you are providing an explanation to your peers with as much detail as possible.

This challenge hinted at an analytics system and a queue. I thought we are using AWS SQS. So I started browsing and checking the Network tab revealed API calls related to it's analytics system. After filtering for interesting requests and trying out several urls, I found one making a request to AWS SQS and that response contained the queue URL i.e., https://sqs.us-east-1.amazonaws.com/092297851374/wiz-tbic-analytics-sqs-queue-ca7a1b2. I used the AWS CLI to ask SQS if there were any messages waiting using this command "aws sqs receive-message --queue-url https://sqs.us-east-1.amazonaws.com/092297851374/wiz-tbic-analytics-sqs-queue-ca7a1b2 --attribute-names All". 
then received a message with a body field containing the url for the flag value. And tried that value which was finally succeeded.


This is a mix of "open-source intelligence" and "AWS knowledge". This is an example of why securing our cloud resources is super important

## Steps:
1. Finding the SQS Queue URL: First, we had to inspect network traffic to uncover the hidden SQS queue URL.
2. Retrieving Messages from the Queue: Once we found it, we used AWS CLI to retrieve messages.
3. Extracting the Flag: The flag was stored inside one of these messages, just waiting to be read.