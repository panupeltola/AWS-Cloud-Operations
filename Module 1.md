I started by following the instructions to add a private key to Putty and connected via SSH to the instance.

I got the IPv4 address from graphic AWS user EC2 interface

![kuva](https://github.com/user-attachments/assets/bb21cda1-8d7f-446b-ab60-477c1bc9e18d)


After connecting to the instance via SSH I started by downloading the file, unzipping the file and installing the service with command './aws/install.

After that I configured the instance using 'aws configure' command and using the given credientials.

In the same picture I also listed users with command 'aws iam list-users'

![kuva](https://github.com/user-attachments/assets/a2c96ead-b65c-464c-be9e-840dd30375f5)

Next I looked at the documentation for the iam console commands. 

The ones that caught my eye were 'list-policies', 'get-policy' and 'get-policy-version'

I started with 'aws iam list-policies'

![kuva](https://github.com/user-attachments/assets/68e66b74-2d41-44f5-8949-b19e6dd5d785)

The first one gave me the metadata for the policy but not the JSON.

I tried running 'aws iam get-policy-version lab_policy' but this gave me an error saying I need arguments --policy-arn and --version-id

I looked at the data of the listing command and saw that there was an arn.

After that I thought the command 'get-policy' could give me the JSON

I ran the command 'aws iam  get-policy --policy-arn arn:aws:iam::160889904058:policy/lab_policy'
Which gave me the same info as 'list-policies'

![kuva](https://github.com/user-attachments/assets/013461f4-a552-4f47-ac2e-f3ac3590ed4b)

Next I looked at the command list again and noticed comman 'list-policy-versions' that gave me an error that I needed the parameter --policy-arn.

Giving it that parameter I got the following response

![kuva](https://github.com/user-attachments/assets/2d8dff61-f74f-422c-b786-f7d26e7443cc)

At this point I realized I had the version number from the first command but didn't know to use it.

However now I had the missing parameters for the command 'get-policy-version'

After running the command with the parameters I got the Json, which I saved to a file with the pipe > lab_policy.json

![kuva](https://github.com/user-attachments/assets/b4298ac4-6a07-42cc-8c94-727b05f4776b)


*Panu Peltola*




