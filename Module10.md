![kuva](https://github.com/user-attachments/assets/67406e2d-b492-4bd4-a38b-6aa848e17a36)# Module 10

## Lab 7 Managing Resources with Tagging

I this lab the objectives were to: 
- Apply tags to existing AWS resources.
- Find resources based on tags.
- Use the AWS CLI or AWS SDK for PHP to stop and terminate Amazon EC2 instances based on certain attributes of the resource

I started by searching for all instances with the value "ERPSystem" for the key "Name"
It gave me a list of all instances with this tag.

![kuva](https://github.com/user-attachments/assets/94538b06-b3c2-48b1-b779-c5f622aaa327)

However this was too long of a list so a --query flag was added to only search for the instance Id

![kuva](https://github.com/user-attachments/assets/afc6e4c2-472c-44ff-a435-9008de8d06d4)

With the next command also the AZ is being queried.

![kuva](https://github.com/user-attachments/assets/f2754f17-faef-4f92-866e-c6643e6107f1)

More queries being made, the query part describes what data is being shown from the database and the filter flag filters the data down.

![kuva](https://github.com/user-attachments/assets/cbb5be61-518c-489d-9a68-e835635b0033)

In the second part the shell program looks for all instances with Project value of ERPSystem and Enviroment value of development. THen it gives the list of instances created by the first command a new tag with key "Version" and value "1.1"

![kuva](https://github.com/user-attachments/assets/e05b4b0e-6ec6-43a4-a382-b0ca5f4fbaad)

After running it we can see only the instances with these two key value pairs were affected.

![kuva](https://github.com/user-attachments/assets/fbbfd5af-b0bf-4f84-9da0-e62d0aa04ac5)

Next I ran the stopinator program.

![kuva](https://github.com/user-attachments/assets/9f8520a4-96de-4666-9dc2-eeb9a60cdae8)

Two instances were being stopped.

![kuva](https://github.com/user-attachments/assets/8bd55c11-19d1-4fa1-bb3a-6f3f7a790f42)

Same two were being stopped in aws console.

After running the second script, they were started again.

![kuva](https://github.com/user-attachments/assets/28198db3-cda6-48ed-a019-4bc09ebe77cf)

Here is the instance I deleted tag enviroment from.

![kuva](https://github.com/user-attachments/assets/2968ac9e-59d0-412b-b54d-83808711c4e1)

Then I ran the program and deleted an instance.

![kuva](https://github.com/user-attachments/assets/1975416f-6289-4953-8ba1-f06d2eca23ac)

## Activity 10 - Optimize Utilization

Objectives:

- Uninstall the decommissioned local database from the Mom & Pop Café instance to decrease the instance’s storage requirements.
- Change the instance type to T2 micro to reduce costs.

I started by logging in to both systems and removing local database from mompop instance.

![kuva](https://github.com/user-attachments/assets/40bcb53c-8ace-4496-864b-ea581334e087)

Deleted.

Next I used the CLI Host to describe the instanceID of mompop
The instanceId is "i-0de67d4ba77eb9ab9"

![kuva](https://github.com/user-attachments/assets/a9c21a61-56fa-4a50-b52a-bfffc3255a53)

Next I stopped the instance, changed its type to t2.micro and started it again.

![kuva](https://github.com/user-attachments/assets/1cb9aa8f-ed47-450a-841d-6771198e2e31)

After describing the instance I could see it had a new type.

DNS Name: ec2-3-95-34-28.compute-1.amazonaws.com
IPv4: 3.95.34.28

I could see the public IP works.

![kuva](https://github.com/user-attachments/assets/e58d603d-c14a-4c0e-b53d-172929a44bad)

## Task 2 calculate savings.

Original setup:

19.99 USD

![kuva](https://github.com/user-attachments/assets/4da0651c-8685-45ae-bdb4-dcfa5c84e662)


Downsized setup:

10.07 USD

![kuva](https://github.com/user-attachments/assets/a3b95f51-bee7-49de-ada4-a6340f168ff0)

Note: I might have used the wrong RDS but as it didn't change, it doesn't matter for the savings.

All in all 9,92 USD a month was saved by downsizing.


*Panu Peltola*








































