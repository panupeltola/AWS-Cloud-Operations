# Module 8 Storage

## Lab 5 Managing storage

In this Lab the objectives are:

This lab is divided into two parts:

- In the Task portion of this lab, you will create:

- Amazon EC2 instance

- Setup AWS CLI

- Create snapshots

- Upload log files to Amazon S3

 In the Challenge portion of this lab, you will be challenged to synchronize contents of a local directory to an Amazon S3 bucket

Objectives
After completing this lab, you will be able to:

- Create and maintain snapshots for Amazon EC2 instances
- Upload files to and download files from Amazon S3


### Task 1 Create an Amazon S3 bucket

I started By opening the AWS Management console, navigating to S3 and choosing create bucket.

I named the bucket "testbucket-4455" and left everything else as is.

![kuva](https://github.com/user-attachments/assets/af2f0233-d5a3-4dc1-9abb-d415c141dc07)


After creating the bucket I went to EC2, chose the correct Instance, and went to menu "Actions -> Security -> Modify IAM role"

![kuva](https://github.com/user-attachments/assets/08612d40-f517-4142-b2f7-754222c9a8af)

Here I updated the security role and clicked on save.

![kuva](https://github.com/user-attachments/assets/e14c5ff1-8fa6-4da9-b00d-16e72ddf7e85)

After modifying the role I connected to the Command Host via SSH as before

Next I needed to take a snapshot with the aws ec2 create-snapshot command.

After using the command on the instructions I got a VolumeId.

![kuva](https://github.com/user-attachments/assets/f215097c-4a25-4ae1-80a9-feba0a3b5d5c)

With the next two commands I searched for the instance Id and stopped the instance.
The instance can't run while creating the snapshot.

The command given on the instructions did not return a prompt even though the GUI console showed it had stopped. I moved forward.

Next I created a snapshot from the root volume of the Processor instance with the volume ID I got from the earlier query.

![kuva](https://github.com/user-attachments/assets/70c8a09d-6867-413e-9fc4-4c46b4c301e9)

Next I started the instance again.

![kuva](https://github.com/user-attachments/assets/165cc430-1085-443f-9377-0fe64c1f5a6b)

After creating the snapshot I had to create a cronjob to make it repeatable. I did it by using the commands given.

![kuva](https://github.com/user-attachments/assets/ad7d9860-99ff-4571-b7f5-b46e4d606270)

After waiting for a while, I had two snapshots with the same volume ID, this means the sync works.

![kuva](https://github.com/user-attachments/assets/da07b1e5-7a81-4308-8e3b-9828870c3956)

After running the script no snapshots were deleted because there were no more than two.

### Task 3 Challenge: Synchronize Files With Amazon S3

I started by starting the versioning and syncing the files:

![kuva](https://github.com/user-attachments/assets/a65f1e84-629d-4e20-9e53-32b54bce335c)

Next I deleted one file and synced them again.

![kuva](https://github.com/user-attachments/assets/5ddd503b-7a19-4e5c-a0d5-ec01ecc52277)

I got an output about one of the files having been deleted.

Next I had to download the file back via version history, because there is no command to directly restore the file.

![kuva](https://github.com/user-attachments/assets/811d0d65-1eca-4f21-a464-284a9c0d3225)

After this I synced the files and the old version was back.

![kuva](https://github.com/user-attachments/assets/f224009a-6536-4527-a1cc-64c57cf472b1)

Lab complete.

## Activity 8 - Work with Amazon S3

Objectives for the activity

- Use the s3api and s3 AWS CLI commands to create and configure an Amazon S3 bucket.
- Configure an Amazon S3 bucket for file sharing with an external user.
- Secure an Amazon S3 bucket for different access requirements by using Amazon S3 permissions.
- Configure event notification on an Amazon S3 bucket

### Task 2

I started by creating the bucket.

![kuva](https://github.com/user-attachments/assets/24ee3a6a-4328-487b-b6b7-0ec32a54ed25)

Here the command 'mb' is short for make bucket and is a command called for that.

![kuva](https://github.com/user-attachments/assets/95c2fd61-f7c8-47ce-bc26-37cd5bc16a87)

Synced files and listed them.

### Task 3

I looked at the permissions of the user.

![kuva](https://github.com/user-attachments/assets/da28765b-3841-4859-8901-a12943b0a5ba)

It allows to get list and describe the objects of s3 buckets.

Next I revieved the policies and created a new access key.

![kuva](https://github.com/user-attachments/assets/48e7173c-6030-4ea5-86d4-886936d295cb)

I copied the account number as:

533267375319


Next I logged in via another browser session.

![kuva](https://github.com/user-attachments/assets/55d900ea-76c3-4f28-8cc5-431e10d42157)

Here I navigated to the Donuts picture and it worked.

![kuva](https://github.com/user-attachments/assets/f31a1ba9-33c1-43e4-a8cc-64cc2607212b)

I could upload photos

![kuva](https://github.com/user-attachments/assets/ab4163d6-61b0-4528-8a9c-70b3a6365c1d)

Deletion succeeded

![kuva](https://github.com/user-attachments/assets/568d5f14-f66c-4eed-8650-3ab5c59965ac)

Permissions could not be changed.

![kuva](https://github.com/user-attachments/assets/d7d3f0cd-dce8-4b8e-82af-44d9e97a3f32)

### Task 4 Configure event notifications on the Amazon S3 share bucket

I created a topic with ARN arn:aws:sns:us-east-1:533267375319:s3NotificationTopic

![kuva](https://github.com/user-attachments/assets/900eb6ea-9d0d-481c-a726-4b012510de06)

In the access policy I created a rule to allow the SNS to be accessed to the topic.

Next I created a subscription

![kuva](https://github.com/user-attachments/assets/1b9c0f7e-4219-4936-a69c-79b4f2f9900f)

Next I created the json.

![kuva](https://github.com/user-attachments/assets/1dd6da6d-ddc2-4b8f-a438-d97f30447233)

At this point I got stuck, I couldn't get the notification to work. Probably a typo somewhere, but this error didn't go away and I didn't get the notification to work.

![kuva](https://github.com/user-attachments/assets/c5fb67d7-85c5-4aa3-a2fb-9f276dc81bfd)

*Panu Peltola*

































































