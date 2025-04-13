# Module 9

## Lab 6

In this lab the objectives were to:

    Use AWS Systems Manager Run Command to install the CloudWatch Agent on Amazon EC2 instances
    Monitor Application Logs using CloudWatch Agent and CloudWatch Logs
    Monitor system metrics using CloudWatch Agent and CloudWatch Metrics
    Create real-time notifications using CloudWatch Events
    Track infrastructure compliance using AWS Config


First I navigated to Run command and chose AWS-ConfigureAWSPackage

![kuva](https://github.com/user-attachments/assets/603874a5-d2ef-4914-a0d7-d0d542e0bc37)

![kuva](https://github.com/user-attachments/assets/630ee6d9-9cbe-44b8-87cb-e2c059195bc2)

*Command parameters*

![kuva](https://github.com/user-attachments/assets/8bafb20f-80f0-4877-8e97-c93550a27106)

*Command succesfully ran*

![kuva](https://github.com/user-attachments/assets/7e270d4b-105f-4615-bd97-7c388c147523)

Output shows the command was successfully ran.


Next I created the parameter as instruted.

After looking at the code of the ManageAgent, I configured it and chose instances

![kuva](https://github.com/user-attachments/assets/b7f99548-c3ca-4f7a-b0d1-d8e916ff6e94)

## Task 2

Going forward I found the CloudWatch logs after looking at the url.

![kuva](https://github.com/user-attachments/assets/31ca002c-e18a-4eea-9c9e-17d518c4687d)

After setting the metric pattern, I can see it works.

![kuva](https://github.com/user-attachments/assets/ea96da0d-9d31-4138-abac-ec24dfde4818)

Topic created

![kuva](https://github.com/user-attachments/assets/38eb0dc7-4ca8-4277-b284-638619a6133b)

Alarm showing insuffient data.

![kuva](https://github.com/user-attachments/assets/ad19bdd1-aaeb-4c87-b962-57b518bb30dc)

After creating the alert and subscribing to the notification I loaded an non existient page many times.

![kuva](https://github.com/user-attachments/assets/c6091421-c5bb-43a4-b6be-003a08060f5a)

Error showed up.

![kuva](https://github.com/user-attachments/assets/d5e182b0-e8a9-48e3-b236-ea0bed564816)

Error also in email

## Task 3

While creating a rule I selected service, event type and secified states.

![kuva](https://github.com/user-attachments/assets/1fa3adbb-e1a4-40be-bca4-8deed049a57e)

Rule was created

![kuva](https://github.com/user-attachments/assets/81b67d64-4f51-4f45-9334-d75134cd0ec9)


After stopping the Instance I got an email notification

![kuva](https://github.com/user-attachments/assets/2e0098e9-2c00-4615-a0ec-836c09e95683)


## Task 4

I chose the rule and created it as instructed.

After that I created another rule where I set the parameters.

![kuva](https://github.com/user-attachments/assets/5032f53b-0748-41c5-af1a-f4103c44925a)

After checking the rules, I see one compliant instance and all others that are noncompliant.

![kuva](https://github.com/user-attachments/assets/8e35a86b-2a71-46d9-ae4e-2314d962f28c)

Lab completed.


## Activity 9

In this Acticvity the objectives are:

    Configure an AWS CloudTrail trail.
    Analyze CloudTrail logs by using various methods to discover relevant information.
    Import AWS CloudTrail log data into Amazon Athena.
    Run queries in Amazon Athena to filter AWS CloudTrail log entries.
    Resolve security concerns within the AWS account and on an EC2 Linux instance.


### Task 1

I started by creating a rule to allow TCP traffic from my IP address as inbound to port 22.

The website worked normally.

![kuva](https://github.com/user-attachments/assets/34c74eeb-1bab-435a-ad41-ddf7b7f064dd)

In CloudTrail I created a new trail and started by naming the Trail as "Monitoring"

![kuva](https://github.com/user-attachments/assets/52a34740-4c6c-4f44-a4e2-cb0324d6e4b7)

Events seems to be as instructed.

![kuva](https://github.com/user-attachments/assets/1128d6c2-5b92-4cce-a9a4-74ac1b5e4534)

Trail was succesfully created.

![kuva](https://github.com/user-attachments/assets/31f0f5b5-1a9e-49c4-8def-2f085e46b238)


Oh no! Mom and Pop got hacked.

![kuva](https://github.com/user-attachments/assets/00ad83d5-615a-4996-8661-638c38cb2beb)

In the EC2 I got an error.

![kuva](https://github.com/user-attachments/assets/8b7941b8-88fc-4908-93ef-4389abda2395)

Also now there was a new rule for anyone to access with SSH.

![kuva](https://github.com/user-attachments/assets/2d29c20b-ba3b-47dc-8862-e2ddceb4ce27)

Next I connected to the instance via SSH and downloaded the logs.

![kuva](https://github.com/user-attachments/assets/f9cff617-f9b4-42a7-8353-c1d52c30c029)

After donwloading, I went to the correct folder and extracted the data.

![kuva](https://github.com/user-attachments/assets/d8845feb-94ee-47d7-a89a-8ef3e8e730b2)

After grepping the logs I got to see a new all that has happened

![kuva](https://github.com/user-attachments/assets/4abef128-4e2f-46cb-9d86-ec7d15cd6e3a)

After filtering some I got some Logs from security groups, but rally hard to read still.

![kuva](https://github.com/user-attachments/assets/6442212c-a574-42d7-bfab-18eddcd8616e)

### Task 4

I created a Athena table as instructed.

After running some SQL commands, I see that the user chaos has been doing something with the security groups.

![kuva](https://github.com/user-attachments/assets/f3a45c9c-a6fd-43d6-8931-54369835ab7f)


## Task 5

At this point in the instructions I should have seen the chaos user still logged in, however it was only me. 

![kuva](https://github.com/user-attachments/assets/e8cbe45d-bc65-49c9-a158-102d45700cc9)

I see it in the user list so I will just delete it.

![kuva](https://github.com/user-attachments/assets/780d8571-3d52-4ea9-908f-6ac5737f54e6)

After this I deleted the ability to login with a password.

![kuva](https://github.com/user-attachments/assets/75a0dd43-a442-49d9-9c53-91022241bcd8)

Next I deleted the SSH inbound rule from the security group.

After that I restored the image from the backup.

![kuva](https://github.com/user-attachments/assets/f65708b8-1f38-47b4-9e9e-030280d28440)

Now the website had been restored.

![kuva](https://github.com/user-attachments/assets/50045dd6-484f-435d-8371-3c7f199a4ea5)


After that I went to AWS console and deleted user chaos.

![kuva](https://github.com/user-attachments/assets/ebd23c4e-d382-48b0-9558-c15cd15fdb6a)

This gave me an error and I had to delete the login profile first.

I did that from the terminal and now the user was gone.

![kuva](https://github.com/user-attachments/assets/1d338e85-9b3d-45b4-a769-af602db9cf82)

























