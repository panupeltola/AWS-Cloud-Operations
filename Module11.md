![kuva](https://github.com/user-attachments/assets/20f5133b-1ee0-49b5-a8b2-2aa95a1c3386)# Module 11

## Lab 8

Activity objectives:

    Deploy an AWS CloudFormation stack with a defined Virtual Private Cloud (VPC), and Security Group.
    Configure an AWS CloudFormation stack with resources, such as an Amazon Simple Storage Solution (S3) bucket and Amazon Elastic Compute Cloud (EC2).
    Terminate an AWS CloudFormation and its respective resources.

After looking at the YAML it seemed pretty straght forward. Some parameters were given and later referenced. All recources had a type and properties.

Onething that was not explained was the DependsOn attribute. It is a wait function that only starts creating the recource when the dependencies are completed.

After creating the stack I can see the Parameters from the YAML show up.

![kuva](https://github.com/user-attachments/assets/ab5a3062-821d-4093-8d3f-2c3f5d2ba03e)

After doing everything I had the stack in creation.

![kuva](https://github.com/user-attachments/assets/0cc90017-8733-4586-acc3-f352e114557c)

After this I needed to create a S3 bucket into the YAML file. I did these two lines.

![kuva](https://github.com/user-attachments/assets/23aabf8f-e82d-494a-ae14-360d3d284056)

I chose update stack and went with direct update, so no change file was created.

Going down the list, in the end I can see my S3 bucket added.

![kuva](https://github.com/user-attachments/assets/16e03f4d-b328-446c-98aa-44d125556bec)

After trying to add an instance to the YAML, I tried to update the file.

I got an error about invalid parameter property

![kuva](https://github.com/user-attachments/assets/c538ef61-cb86-464c-9ab8-c404b2529e16)

After looking at the code, I noticed the indents were wrong

![kuva](https://github.com/user-attachments/assets/9952b992-2850-4468-a4a4-1de4f8a86c92)

After fixing I tried again.

![kuva](https://github.com/user-attachments/assets/817cccc4-dfc0-4d9f-b808-d43405512f7e)

A new error always means progress, I removed the symbols.

![kuva](https://github.com/user-attachments/assets/25dcab8c-3c2b-4ef5-801b-9a77e3e8bc93)

Now the instance showed in my changes.

After this I deleted the stack by selecting Delete.

![kuva](https://github.com/user-attachments/assets/5fc6c72c-b8f7-4d31-bb9d-c62b5c166c51)

![kuva](https://github.com/user-attachments/assets/c27e55b8-020e-4af8-96cc-85c012b9bea5)

Lab complete.

## Activity 11

Activity objectives:

    Practice using JMESPath to query JSON-formatted documents.
    Troubleshoot the deployment of an AWS CloudFormation stack by using the AWS CLI.
    Analyze log files on a Linux EC2 instance to determine the cause of a create-stack failure.
    Troubleshoot a failed delete-stack action.

I start by going to the address and copying the instructed JSON and looked at all the possibilities of the possible searches and filters. It worked like a database would.

### Task 2

After connecting to the instance and configuring AWS I created the stack and waited for the resources to complete  creating.

![kuva](https://github.com/user-attachments/assets/2acb54ea-6d50-4375-b472-4bed437b3fca)

After a while it started deleting recourses.

![kuva](https://github.com/user-attachments/assets/b90953ad-1a9e-4930-a1ae-6fc2654ed104)

This probably happened because a stack deletes rolls back and deletes everything, if any of the recourses fails to create.

![kuva](https://github.com/user-attachments/assets/539148e7-ef1a-4dfb-a17f-e3b948be7a25)

After looking at logs, it would seem that the problem was in the userdata section of the EC2 creation. Giving an error from WaitCondition.

After adding the flag --on-failure DO_NOTHING I created the stack again. The reason for this stack is to stop the rollback after the instance fails.

![kuva](https://github.com/user-attachments/assets/ee5e56ee-8591-4f2d-abfd-1d0c13a4e574)

Once again the problem is the same, but this time I will connect to the problem instance.

I got the IP address of the instance 54.210.19.43.

After lookiung at the error logs, I found the script that was giving problems. 

It seems the problem was caused by package http not being found and the flag '-e' failed the whole script because it didn't execute fully.

I corrected the script, deleted the failed stack and created a new stack.

![kuva](https://github.com/user-attachments/assets/4105138d-1f15-4522-a2de-337d0db9098c)

Now the WaitCondition completed.

![kuva](https://github.com/user-attachments/assets/97db819f-b02e-4433-bcdb-9cd95a4a5857)

Output also shows what it needs to show.

![kuva](https://github.com/user-attachments/assets/21871e70-15dd-40b8-b54a-91ba4a682b44)

Now the webpage works.

### Task 3

I started by changing the SSH to only work from my IP.

Next I created a new file and uploaded it to S3

![kuva](https://github.com/user-attachments/assets/2e608bc9-f330-4626-8d82-0bd10de11b90)

After checking for drift I could see that Security group had been modified but S3 bucket was still in sync.

![kuva](https://github.com/user-attachments/assets/a7cc2725-4f03-4697-84c6-159ec21adb56)

After looking closer at it it shows that the IP to port 22 is NOT_EQUAL to the property.

![kuva](https://github.com/user-attachments/assets/7ae3982b-3872-4b98-8b87-cb7f91b1d6c3)

Lastly I try to delete the stack.

It started deleting, but I noticed that the bucket could not be deleted.

![kuva](https://github.com/user-attachments/assets/e8b15856-dc32-4582-b267-37ea58f3cebb)

Next I needed to retain the S3 bucket but delete everything else.

For that there was a command --retain-resources. Now I needed the logical IDs.

For that I found a command describe-stack-resources. I will try that one.

![kuva](https://github.com/user-attachments/assets/06f27a36-f5de-4ae1-9713-0bdef6ebaaf9)

From this command I found out that the logical name is MyBucket.

![kuva](https://github.com/user-attachments/assets/4f2edd52-f2f2-45a8-bfbb-19d967913baa)

After trying to look at the statistics I couldn't because the stack was deleted. So command worked.

Activity completed.











































  
