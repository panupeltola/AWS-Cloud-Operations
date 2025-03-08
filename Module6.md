# Activity 6 Migrate to Amazon RDS

The goal of this activity was to migrate a local database to Amazon RDS service.
The steps are:
- Create a new database using AWS CLI
- Migrate the existing database the the created database
- Create monitoring for the database

After running the command given in the instructions, I got the Instance ID, Public DNS, Public IP and aviability zone of my desired instance.

The describe-instance is starting to look fairly familiar and powerful at this point.

After this we find the IPV4 CIDR block of the VPC. I had to google what CIDR means and it seems to be an more flexible IPV4 routing protocol for use inside of organizatoions.


After the next step I also got the CIDR block of the Public Subnet 1

![kuva](https://github.com/user-attachments/assets/ea305487-2809-47e4-98ba-256f7ef3e74a)


And to record the values using the given instructuions:

MomPopCafe VPC IPv4 CIDR block: 10.200.0.0/20

MomPopCafe Public Subnet 1 Subnet ID: subnet-0dd63f2fd8cb25d8b
MomPopCafe Public Subnet 1 IPv4 CIDR block: 10.200.0.0/24

Next I searched for the aviability zones in us-east-1:

List of Aviability Zones in the region: us-east-1a, us-east-1b, us-east-1c, us-east-1d, us-east-1e, us-east-1f

Above is the record of Aviability Zones per instructions

After going to the Mom Pop cafe site, the order history is empty

![kuva](https://github.com/user-attachments/assets/ccb35829-7c36-447b-b880-45639477f9da)


Next I created the security group and got the security group ID

![kuva](https://github.com/user-attachments/assets/f284908a-ff3e-489a-9d81-49cc619c4817)


In the next part I created an inbound rule for the port 3306 to be access the instances in this security group. It will probably be so that this instance will have the database and an instance from the main Mom Pop security group will be able to access this.

![kuva](https://github.com/user-attachments/assets/dc6758ee-3cd2-4ed1-8179-1c53aa23502c)

Next I created a subnet for the database:

![kuva](https://github.com/user-attachments/assets/3b1759b7-39db-431f-ae53-98364305eac8)

PrivateSubnetId: subnet-05f3c115204f948c4

Next I created another subnet to a different region, in this case us-east-1b:

![kuva](https://github.com/user-attachments/assets/e101d187-4f9d-4d44-b170-905fa41718f6)

PrivateSubnetId2: subnet-0745314445112a733

After this I created a DB subnet group:

![kuva](https://github.com/user-attachments/assets/0fbbb7a0-38c8-43ee-af3b-64c4e5c4b020)

While creating the database using the instructions I got two errors:

The DB engine version in the instructions didn't exist anymore and i mixed up regions and aviability zones. After fixing these, the database was created.

![kuva](https://github.com/user-attachments/assets/8c332d1f-97ea-4cd7-a8a9-dd2a54b460e1)

*Errors*

![kuva](https://github.com/user-attachments/assets/32050c54-ed86-47c8-8b84-81fab177d680)

*Snip of the success output*

![kuva](https://github.com/user-attachments/assets/a4e50b91-88c4-430c-a355-8dc6e79b3c9d)

After a while the database was aviable.

## Task 2

After creating a dump of the db I looked what it contained, it was the basic info of the order data, but it was empty.

Next I imported the SQL file to the newly made database, made a connection and I could look up info on it. 

![kuva](https://github.com/user-attachments/assets/9278f6a1-d7b6-48ce-8c18-9b6dff77a1fd)

## Task 3

Next I went to edit the parameter of the dbUrl and changed it to the new rds database

![kuva](https://github.com/user-attachments/assets/abf0de91-788c-44d2-b8f5-2b52c02cb35f)

I had no orders before so it didn't show if the old orders stayed, but at least now I could still make orders.


In CloudWatch I can see my logins:

![kuva](https://github.com/user-attachments/assets/e098b1ee-4909-4088-ac5b-1a13f8cc545f)


Task Complete.


*Panu Peltola*

