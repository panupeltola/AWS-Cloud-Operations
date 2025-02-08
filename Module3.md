## Lab 2 

Lab overview and objectives

Traditional methods of deploying servers and configuring security are complex and often involve multiple teams and long delays. Fortunately, it is quick and easy to deploy secure infrastructure in the cloud. As a Systems Operator, you can automate many of these processes using the AWS Command-Line Interface.

After completing this lab, you should be able to do the following:

- Launch an Amazon EC2 instance using the management console

- Launch an Amazon EC2 instance using the AWS Command-Line Interface (AWS CLI)


![kuva](https://github.com/user-attachments/assets/0002b653-3ee5-47af-95f9-a8f1e201a44d)

Name and image as per instructions.

![kuva](https://github.com/user-attachments/assets/f96fa6d1-9238-4ff4-bce1-6c75e58587f9)

Security settings as per instructions. *Check later if Key pair is custom or generated each time.*

![kuva](https://github.com/user-attachments/assets/4942f657-e651-44dd-8996-bd6e7d8b136f)

Instance running.

![kuva](https://github.com/user-attachments/assets/bb785fc0-deaf-41bf-b033-550d9b083b6a)

Connected to the instance.

![kuva](https://github.com/user-attachments/assets/1bfb2e2a-1c8a-44ef-8c35-1b50b1800499)

Got the parameters to the AMI ID.

![kuva](https://github.com/user-attachments/assets/4a2ff518-a206-45d3-b29c-53a087c9c8c6)

Got the Subnet and Security group variables. From my understanding what we have done here is first created a token, then with that token aquired some variables from the account connected to the user. The commands are done to local account or group files.


![kuva](https://github.com/user-attachments/assets/3640b2b1-0066-4d5d-82ca-1ed35dce1c90)

After running the command pasted from the instructions I got an error. Looking at manual page all seemed to be right. In the end the problem was solved by deleting spaces and empty lines between the new dashes.

![kuva](https://github.com/user-attachments/assets/e9a81cb6-6906-409e-a080-5273380f5ccb)

I used the commands in the instructions and got to the page. 

![kuva](https://github.com/user-attachments/assets/98b525f6-2a6a-4dc2-9915-25602ab49683)


Web server shows in the instance list

![kuva](https://github.com/user-attachments/assets/56a88b98-7039-4efd-8996-f25076e42413)

Lab done.


## Activity 3 - Troubleshoot Creating an EC2 Instance Using the AWS CLI

In this activity, you will use the AWS Command Line Interface (AWS CLI) to launch an Amazon Elastic Compute Cloud (Amazon EC2) instance into the eu-west-2 (London) Region.

Activity objectives

After completing this activity, you will be able to:

- Launch an Amazon EC2 instance using the AWS CLI
- Troubleshoot AWS CLI commands and Amazon EC2 service settings


I skip the connection part since it is the same as always.

![kuva](https://github.com/user-attachments/assets/3d0e4c15-7394-4e87-8a2b-80471d3ef3fc)

Configuration.

After running the command I noticed that the program does not echo VPC or Subnet Id.
This was also a recurring reason for errors. I will look into this problem.

![kuva](https://github.com/user-attachments/assets/1d27b149-8a6d-46ac-9db4-3e029d206b28)

Looking into my VPC listing, the VPC in eu-west-2 does not have a name so it will not be found by the filters. Since it is read only, I will try to change it's name from here.

Also looking while looking with command 'aws ec2 --region eu-west-2 describe-subets' We can see that no subnet is in the region of eu-west-2a. While the query in the script tries to find one. I will change the area to 2b.

This didn't work so I went back to VPC management window in AWS. I found the MomPopCafe VPC in a different region of us-east-1a.

![kuva](https://github.com/user-attachments/assets/77a0e89a-a934-4134-9c9d-d3580b4cad3d)

Under this VPC there also seems to be the wanted subnets.

After a quick google session, it seems you can't easily copy a whole vpc and the other way would be to build it from ground up. I will simply change the origin of the vpc and subnet location.

![kuva](https://github.com/user-attachments/assets/0610cf8f-6959-49c6-8f9a-e2b40755d90f)

A new error is always progress. Now I will check the images.

![kuva](https://github.com/user-attachments/assets/aebfa9c5-bfd9-4e4c-b58b-3fde76f933fc)

Found the culprit to be a wrong region in the creating EC2 instance command.

![kuva](https://github.com/user-attachments/assets/498192fa-e15f-492c-ab53-f27b40e1c22b)

This time the program got through and I can see if the connection works.

While waiting for the booting to complete, I noticed that a wrong port is opened while creating the instance. I will change it to 80.

My SSH connection would not work. I checked and it seems no key pair was ever created while creating a machine. In ec2 keypairs there is a keypair called vockey. I will try to use this.

![kuva](https://github.com/user-attachments/assets/37acab53-7654-4f8e-9764-941a06b72025)

![kuva](https://github.com/user-attachments/assets/0344dfe9-2053-4c86-84e3-28456842b633)



After adding parameter '--key-name $key' to the file it now lets me log in.


It also seems the fixing of the port now lets me load the page.

![kuva](https://github.com/user-attachments/assets/137cd021-e3fe-45a9-ab08-82100b4e83e0)

The logs were fine.

The page also works now.

![kuva](https://github.com/user-attachments/assets/e86a90e5-ecd3-4da8-aaf9-f78294707dce)

Order confirmation worked.

![kuva](https://github.com/user-attachments/assets/43734482-0b9c-4694-8231-1c55f22c4381)

And it shows in the order history page.

![kuva](https://github.com/user-attachments/assets/14db53b1-9730-40bf-9f5e-75df172342f8)

Lab completed.


## Thoughts

This Activity was a fairly confusing in the beginning since it was not clear what to do with the region being wrong. I spent a long time just trying to figure out if it even was possible to move the VPC and subnets. However this figuring out and banging my head on the wall helped me understand better how the EC2 launch process goes and what potholes there could be while troubleshooting. All in all this was a good challenge to seek information but frustrating because it wasn't clear was it expected to just get this working or create an AWS infrastructure for it also.




