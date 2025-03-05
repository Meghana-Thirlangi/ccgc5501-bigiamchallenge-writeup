# copy the contents of sample.md to start
## Solution  

1. **Enable Bastion Host:**
   - In the `terraform.tfvars` file located in `cloudfoxable/aws/`, set `bastion_enabled = true`.

   ```hcl
   bastion_enabled = true
   ```

   - Run the command: 

   ```bash
   terraform apply
   ```

2. **Fix JSON Error:**
   - Resolve the JSON error by hardcoding the variable values in the `terraform.tfvars` file.

   ```hcl
    #######################
    # User IP Configuration 
    #######################

    # Specify your IP address here to bre used with security groups (so that "public" challenges are only accessible to you)
    # If you don't specify anything, the default behavior is to use https://ifconfig.me to pull your IP address
    user_ip = "38.85.175.159"
   ```

   - Re-run the command:

   ```bash
   terraform apply
   ```

3. **Install Cloudfox:**
   - Follow the provided documentation to install Cloudfox on your local machine.

4. **Find Instance ID:**
   - Execute the command to list instances:

   ```bash
   cloudfox aws -p cloudfoxable instances -v2
   ```

5. **Locate SSM Command in Loot File:**
   - Access the loot file generated and stored locally.
   - Find the SSM command required to connect to the instance.

6. **Install Session Manager:**
   - Follow the AWS official documentation to install the AWS Session Manager plugin on your machine.

7. **Start an SSM Session:**
   - Use the command from the loot file to start an SSM session:

   ```bash
   aws ssm -p cloudfoxable start-session --target <target id>
   ```

8. **Check Permissions:**
   - Run the command to check the instance permissions:

   ```bash
   cloudfox aws -p cloudfoxable permissions --principal [NameOfInstanceRole]
   ```

9. **List Bucket Contents:**
   - Execute the command to list the contents of the S3 bucket:

   ```bash
   aws s3 ls s3://<bucket-name>
   ```

10. **Retrieve the Flag:**
    - Use the command to copy and display the contents of the flag file:

    ```bash
    aws s3 cp s3://<bucket-name>/flag.txt -
    ```

## Reflection  

* **What was your approach?**  
My approach involved systematically following the steps outlined in the Cloudfoxable Bastion 100 challenge, focusing on enabling the bastion host, setting up the session manager, and then leveraging the permissions I found to access the S3 bucket and retrieve the flag.

* **What was the biggest challenge?**  
The biggest challenge was the initial error regarding the JSON format in the policy files. This was something I had encountered before, but it was still an obstacle that took a bit of time to debug.

* **How did you overcome the challenges?**  
I quickly recalled from my previous experience that the issue was related to improperly formatted JSON in the policy files, so I resolved it by hardcoding the variable in the `terraform.tfvars` file. Once I did that, the deployment proceeded smoothly.

* **What led to the breakthrough?**  
The breakthrough came when I leveraged my previous experience, particularly with S3 access and the retrieval of flags from similar challenges. The knowledge from those experiences helped me to quickly identify the problem and use the correct commands to retrieve the flag.

* **On the blue side, how can the learning be used in the future?**  
The process I followed here can be applied in future scenarios when working with cloud infrastructure, particularly with AWS, to troubleshoot and overcome similar issues in permission handling and S3 access. The steps I followed will be useful for future CTF challenges or real-world tasks related to accessing and managing resources securely on AWS.