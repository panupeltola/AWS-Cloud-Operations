## Lab 3

The Objectives of the lab were following:

After completing this lab, you will be able to:

    Create a new AMI by using the AWS Command Line Interface (AWS CLI).

    Use Amazon EC2 Auto Scaling to scale up the number of servers available for a specific task when other servers are experiencing heavy load.

The following components are created for you as a part of the lab environment:

    Virtual private cloud (VPC)

    Public subnets

    Private subnets

    Amazon EC2 - Command Host (in the public subnet): You will log in to this instance to create a few of your AWS assets.

You will create the following components for this lab:

    Amazon EC2 web server

    AMI

    Auto Scaling launch template

    Auto Scaling group

    Auto Scaling policies

    Application Load Balancer


    
For this exercise I will be using a Debian 12 virtual machine.
After the first steps I added the values to the ec2 command to create a new instance, the output gave me an ID for the instance. In this command we created a new instance and gave it for example machine type, user data and tags.

![kuva](https://github.com/user-attachments/assets/7a1c704e-97ca-4714-bdb0-b38e1e954ef8)

The first command didn't give me a response even after waiting for multiple minutes, but the page had a public dns address. I tried pasting it in a browser

![kuva](https://github.com/user-attachments/assets/d983cce8-d99e-4c5d-bfd5-8ede642a8582)

The page is visible.

After this I created an AMI from the instance using the 'aws ec2 create-image' command.

![kuva](https://github.com/user-attachments/assets/d697cf94-85c2-4dd8-a21f-4acb5e2c864d)

## Create an auto scaling enviroment

I went to the AWS website and went to EC2 page and chose Load Balancing and started to create a load balancer. I was to create an Application Load Balancer.

![kuva](https://github.com/user-attachments/assets/0f7275f8-3b7d-4467-bedd-86188d4856b8)

*Name and subnet settings*

![kuva](https://github.com/user-attachments/assets/1adeb3b8-099b-4b60-b6cd-030b6e1a0981)

*Target group*

What I am doing here is creating listeners and conditions to the load balancer for it to make decisions on some data I am now configuring.

![kuva](https://github.com/user-attachments/assets/f6b0e9fb-dc2a-4ac0-bd72-c57a640132f7)

*Listeners and routing now shows my target group*

![kuva](https://github.com/user-attachments/assets/3c723c5e-ec29-4d5a-ac23-cb4b487e744e)

*Rule created*

## Create a launch template

I browse to Create Launch Template

The AMI I created earlier in CLI shows up

![kuva](https://github.com/user-attachments/assets/cd1b4366-4b99-43d1-abf8-7753427fc3c0)

![kuva](https://github.com/user-attachments/assets/717944dc-5585-4ad7-9d62-7730596c709a)

*Instance type and SG*

![kuva](https://github.com/user-attachments/assets/5eb8638c-b6f1-4523-acee-d5c41398fb96)

*Template created*

## Auto Scaling group

![kuva](https://github.com/user-attachments/assets/e37db9bc-3dc9-419d-beee-25ee6b9245c4)

*Name and Template*

![kuva](https://github.com/user-attachments/assets/42436616-5d96-4094-861a-1076fb87f28d)

*Load balancer chosen*

I should now enable group metrics to be collected, but there is no such item on the page. I will see if it is in a later part.

![kuva](https://github.com/user-attachments/assets/1d7a49ab-294e-41e7-8278-53ba9f6fade9)

*Scaling adjusted*

![kuva](https://github.com/user-attachments/assets/169d5ca3-6add-48a7-ba48-29d91b2f56f3)

*The CloudWatch option was on this page instead of the last one*

![kuva](https://github.com/user-attachments/assets/9789d9a8-fde9-4d01-a2ec-69c067667a2a)

*Group created*

## Vertification

![kuva](https://github.com/user-attachments/assets/34e214da-c961-43a5-ba59-d75c44842c1e)

*Two instances of WebApp running*

![kuva](https://github.com/user-attachments/assets/bac37ff9-8090-4884-8db5-e1569697f256)

*All healthy in Target Groups*

![kuva](https://github.com/user-attachments/assets/36eb0e51-4e47-41f8-be42-8b6d1e402be9)

*Third instance opened after the server being under load due to AS group rules*

Lab complete









































    
