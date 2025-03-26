# Module 7

In this module we looked at how AWS manages networking. The logic behind AWS is in a way a bit limited compared to creating your own network, but with the connectivity options these limitations can be avoided.
The backbone of a AWS network is a VPC (Virtual Private Cloud) which is a private IP address space in which you can deploy networking components. It works like any other network but for example a VPC is limited to one region and it can only have so many addresses.
The VPC can be further divided into smaller sections by adding subnets inside the VPC.
In this module there were two exercises: A lab on configuring a VPC and an activity on troubleshooting one.

## Module 7: Lab 4 - Configure VPC
Lab overview and objectives

Traditional networking is hard — it involves equipment, cabling, complex configurations and specialist skills. Fortunately, Amazon VPC hides the complexity while making it easy to deploy secure private networks.

This lab shows you how to build your own Virtual Private Cloud and deploy resources.

After completing this lab, you should be able to do the following:

    Create a Virtual Private Cloud (VPC) that contains a private and public subnet, an Internet Gateway (IGW), and a Network Translation (NAT) Gateway.
    Configure Route Tables associated with a public subnet for internet-bound traffic to be directed to the IGW for direct internet access.
    Configure Route Tables associated with a private subnet for isolated resources to securely connect to the internet through a NAT Gateway.
    Launch a Bastion Host in a public subnet for resource-based secured access to the private subnet.
    Evaluate the operation of the private subnet's ability to communicate with the internet.

### Task 1 Create a VPC

I started by going to VPC console on the AWS control panel and chose "Create VPC"

While creating the VPC I changed its name and IPv4 CIDR block. IPv4 CIDR block works the same way as a network address and subnetmask would in traditional networking. While choosing the CIDR block it is important to take into considiration the CIDR blocks of other parts of network if those are already created. This could be a problem if for example you tried to create peered connection between two VPC:s later. If the CIDR blocks have a conflict the connection would not succeed.

In this CIDR block the beginning 10.0.x.x is the network address and the last two digits are given to the subnets and interfaces inside the VPC.

![kuva](https://github.com/user-attachments/assets/f0ff4aa7-04b4-473d-90ac-2f9d6d1f3cdd)

*VPC options*
Next I was supposed to choose "Actions" and selected "Edit DNS hostnames" from the drop down menu. However this option was not aviable, I chose "Edit VPC settings" instead.

![kuva](https://github.com/user-attachments/assets/b241cc65-cf1d-4885-b007-65113f5a7e94)

*Options*

There I found DNS settings and in there was an option with an unselected flag for "Enable DNS hostnames" I tried selecting it and clicked "Save". I will return to this later to see if the "firendly DNS name" option is on.

![kuva](https://github.com/user-attachments/assets/18af3feb-d060-403e-b332-820e79397788)

*VPC options*

### Task 2 Create Subnets

In the next part I will be creating subnets. By default a subnet is private, meaning it can not be accessed directly from the internet. AWS documentation describes public subnet followingly: "A subnet is public if its route table conatains a reference to a public internet traffic being processed by an internet gateway attached to the VPC". This will be better explained later.

First task was to create a public subnet.

I went to Subnets page and selected "Create subnet"

Here I selected the VPC, aviability zone and CIDR block for the subnet.

While choosing the CIDR block it is again improtant to choose one that does not conflict with the CIDR block of the VPC or any other subnet. Having conflicting CIDR block addresses will make the interface unaviable.

![kuva](https://github.com/user-attachments/assets/702d6cad-9723-4b61-8114-15a48a9118b5)

Next I went into the subet settings and enabled "Auto-assign IP"
This means that every new interface will be automatically given a IPv4 address from the primary network interface. The primary interface is a network interface created with the subnet that is connected to the VPC and can't be detached from it.

![kuva](https://github.com/user-attachments/assets/9fbf12e3-55b8-448d-9981-bde219ed595e)


I saved the settings.

Next I created another subnet with the same steps.

![kuva](https://github.com/user-attachments/assets/10663465-fed9-4dc0-b17e-89f3cbc7b42f)

*New subnet, unconflicting CIDR block*

Note that with this subnet we did not turn on the automatic IP, it is usually only used with public subnets.

### Task 3 Create an Internet Gateway

Next I had to create an Internet Gateway, which is an gateway through which subnet interfaces can be connected to the public internet.

I started by creating it through the dashboard.

![kuva](https://github.com/user-attachments/assets/130039dc-f735-4484-aca4-4eaf8440a330)

After creating the gateway there was a button to attach it to a VPC. I followed the button and configured the VPC to be Lab VPC.

![kuva](https://github.com/user-attachments/assets/21d24e9c-acd0-4aeb-9e7a-9c02b833911a)

Now I had connection to the internet but neither of the subnets had routing to it.

### Task 4 Configure Route Tables

Next I had to configure the Route Tables. Route tables are a list of permissions that an incoming connection will be checked against. If no routing tables are created, the subnets will use a default routing table.

I went to routing tables and inside I could find my VPC. I knew which was the right VPC by going to the "Routes" page after selecting the VPC and seeing the CIDR block configured earlier.

![kuva](https://github.com/user-attachments/assets/480b3b34-0909-472e-aff2-7f7d0ff668f5)

I changed the name of the route table to "Private Route". Next I chose "Create route table" and named it "Public Route Table" and connected it to the correct VPC.

Next I needed to edit the routes. I created another route to 0.0.0.0/0 and gave it the target of the Internet Gateway I created.

![kuva](https://github.com/user-attachments/assets/8dd78305-0fdb-4f21-89e0-75cf3aa7172f)

Now this route table has two rules. First it will check is if the destination is in the VPC address range. In that case the connection would be seen as local and would be routed to the VPC router. However if the destination IP address is anything else this rule will route it to the internet gateway.

Creating Route Tables can be used in many different subnets and they are a quick way to configure subnets.

After creating the Route Table I associated it with the Public Subnet through the "Edit subnet associations" tab.

Now my Public Subnet had route to the internet.

### Task 5: Launch a Bastion Server in the Public Subnet

In the next task I needed to create a bastion server. Bastion servers are often used as an added security measure. Contents of the private and public subnets can only be accessed through them and often connections require two different key pairs or other authentication methods.

I will create the EC2 instance as instructed.

In here the interesting settings are the security group rules.

![kuva](https://github.com/user-attachments/assets/d36a6f28-2c8c-4e3a-8198-8c4aa93bc065)


This was created, because one did not exist before. The difference between security groups and route tables is that route table can be associeted with subnet or VPC while security group is to instances in this case. If both seucirty group and route table do not allow the connection, the connection can not be made.

I continued to create the instance as instructed.

### Task 6: Create a NAT Gateway

Next I needed to create a NAT gateway. This is so that my instances inside the subnet could be connected to the internet gateway.

I navigated back to the VPC page and selecte Create NAT gateway.

I created it as instructed.

![kuva](https://github.com/user-attachments/assets/cc329ea8-cf73-4da9-8e67-e03e7c868508)

An elastic IP needed to be allocated, because a NAT gateway needs to have an static IP address.

Next I needed to go to my Private Route route table and add a new route to it.

![kuva](https://github.com/user-attachments/assets/6a0daec2-9133-4b56-aebb-a48fb19bf32a)

This adds the rule so that all traffic that is not local is sent to the NAT gateway.
Now the private subnet can be accessed through the Bastion host, which is connected to the public internet because it is inside the Public subnet.

### Optional Task: Test the Private Subnet

I started by creating the EC2 instance as instructed.

Settings of note were:

- Subnet being private subnet
- Auto assign IP disabled
- New security group

The security group could have been made more secure for example by only allowing connections from the bastion host or from known IP ranges.

In the end I pasted a script that allows SSH login to be done with password.

![kuva](https://github.com/user-attachments/assets/15706d19-4bea-45a6-b599-77c05269b943)

I looked up the IP addresses needed and wrote them down:

Bastion Host IPv4: 44.222.93.36
Private Instance IPv4: 10.0.2.167

![kuva](https://github.com/user-attachments/assets/d2c55b9c-7f0d-4b46-9d7e-792958fc9a89)

As seen in the picture below, Private Instance does not have a public IPv4 address.

I downloaded the PEM file and did the steps as before. 

First I wanted to make sure the Private Instance could not be accessed directly through the internet.

![kuva](https://github.com/user-attachments/assets/09d0abe6-975c-4288-ae34-71f195a1633f)

No connection, as there shouldn't be. IPv4 addresses starting with 10 are always private addresses.

I connected to the bastion host.

![kuva](https://github.com/user-attachments/assets/1be95205-6a86-4aa7-b45c-d577c7a72d9a)

First I wanted to know if ping command could reach the Private Instance. I tried pinging the IP address. 


![kuva](https://github.com/user-attachments/assets/1564ab87-bd1a-4fc7-969b-76976f32e48c)

Nothing was reached. This makes sence, since only traffic from port 22 is allowed into the private instance.

Next i ran the command 'ssh 10.0.2.167'

This prompted a password from me and I gave the one instructed. This gave me access to the private instance.

![kuva](https://github.com/user-attachments/assets/929159fd-b554-4bde-aca8-06720ae1d007)

Last thing I did was try to ping a page from the private instance.

I ran command 'ping -c 3 haaga-helia.fi'

All the pings got through and access to the internet was confirmed.

![kuva](https://github.com/user-attachments/assets/9fb9865b-c477-4134-8162-703c9d314392)

Since the private instance only had routes for outbound traffic to be sent to the NAT gateway and only had ssh as inbound, the ping request I tried earlier did not go through. However once the traffic was outbound, the ping worked fine. After being routed to the bastion host, it used the route table in the public subnet to route it further into the internet gateway and from there beyond.


## Activity 7

The goal of this activity was to troubleshoot network and create a S3 bucket to hold logs.

The objectives for the activity are:



1. Create an Amazon Simple Storage Service (Amazon S3) bucket to hold the flow logs, and then you  enable VPC Flow Logs on VPC1.2.
   
2. This task includes two parts, which are labeled #2 and #3 on the diagram, and these parts are where you will:
3. Troubleshoot VPC1 network configurations. VPC1 has a public subnet with two instances in it. One of these instances is the MomPopCafe Web Server instance the runs the website. There is a public route table that maps network traffic from the public subnet to the VPC2 internet gateway. That is how the network is supposed to be configured. However, VPC1 has two issues that will require some troubleshooting.
4. Troubleshoot the issues in VPC1. A CLI Host instance is provided. You will run AWS Command Line Interface (AWS CLI) commands from the CLI Host. The CLI Host must run outside of the virtual private cloud (VPC) that has the issues. Therefore, a second VPC, named VPC2, has been created. The CLI Host runs in VPC2. There are no configuration issues with VPC2. Your CLI Host instance connects to VPC1 resources in the same way that your physical machine connects to VPC1 via the internet. VPC1 and VPC2 are not peered.
5. Troubleshoot the symptoms that you experience after you start the activity. They are depicted with a red X on the diagram. However, keep in mind that symptoms and root cause are different things.
6. Download the flow logs to the CLI Host, and analyze the log entries. Some of the actions you took during troubleshooting will be reflected in the flow logs.

### Task 1

I started the assignment by running the command 'curl http://169.254.169.254/latest/dynamic/instance-identity/document | grep region' Looking up what this address was, it is apparently an address used by AWS and other cloud computing platforms to host instance metadata. (https://serverfault.com/questions/427018/what-is-this-ip-address-169-254-169-254)

The search output my region as "us-east-1"

![kuva](https://github.com/user-attachments/assets/9783b301-4d5a-4ade-aa88-1ebcfe6eaadb)

Next I configured the AWS CLI as done before. 

![kuva](https://github.com/user-attachments/assets/67a4d040-a174-4a06-bd77-232092f56d66)

To create the S3 bucket I created the command 'aws s3api create-bucket --bucket flowlog4444 --region us-east-1 --create-bucket-configuration LocationConstraint=us-east-1'

Here the parts of the commands are:
- aws, calls the aws module
- s3api calls the s3 commands
- create-bucket is a command to create new bucket
- '--bucket flowlog4444' configures the name of the S3 bucket
- '--region' configures the region to use the command in. This command overrides the config settings.
- '--create-bucket-configuration LocationConstraint=us-east-1' sets the location of the bucket to the region

In this command the meaning of --region is to configure the region the command is run at and the --create-bucket-configuration LocationConstraint=us-east-1 is to configure the location of the bucket. They just seem similiar.

I got an error while running this command. When looking at the instructions it said to delete the '--create-bucket-configuration LocationConstraint=us-east-1' part if using us-east-1 region. According to AWS documentation us-east-1 is the default location of S3 buckets and that is the reason it can't be selected as the location. Kind of confusing.

![kuva](https://github.com/user-attachments/assets/ba8e6583-7e93-410d-9578-9aec47fc0e47)

Bucket got created.

Next I needed to find the VPC ID of VPC1. This was done by running the command 'aws ec2 describe-vpcs --query 'Vpcs[*].[VpcId,Tags[?Key==`Name`].Value,CidrBlock]' --filters "Name=tag:Name,Values='VPC1'"'

Here the command calls to describe VPCs and it has query values of all VPC IDs, their names and CIDR blocks if the filter condition of tag Name equals VPC 1. Only one VPC returns it's values in this case.

The VPC ID is vpc-09f026bece1c9bcb2.

![kuva](https://github.com/user-attachments/assets/4601a412-1fba-49f1-97f9-0456166085a8)

Next I needed to create flow logs using the following command 'aws ec2 create-flow-logs --resource-type VPC --resource-ids vpc-09f026bece1c9bcb2 --traffic-type ALL --log-destination-type s3 --log-destination arn:aws:s3:::flowlog4444'

- 'create-flow-logs', Creates one or more flow logs to capture information about IP traffic for a specific network interface, subnet, or VPC
- '--resource-type VPC' The type of resource to monitor.
- '--resource-ids vpc-09f026bece1c9bcb2' The IDs of the resources to monitor. For example, if the resource type is VPC , specify the IDs of the VPCs.
- '--traffic-type ALL' The type of traffic to monitor (accepted traffic, rejected traffic, or all traffic)
- '--log-destination-type' The type of destination for the flow log data.
- --log-destination arn:aws:s3:::flowlog4444' The destination for the flow log data.

![kuva](https://github.com/user-attachments/assets/941988d5-1cef-4fa1-9ac2-99d46ecef8bd)

After running the command I got the FlowLogIds and the client token for this.

![kuva](https://github.com/user-attachments/assets/b8e84b6b-3cbf-4326-bf63-f13db7bc173c)


After running the command 'aws ec2 describe-flow-logs' I got information about the just created logs with the correct destination.

![kuva](https://github.com/user-attachments/assets/1613fffe-38f2-45a5-bc60-99e65259ef1f)

### Task 3 Analyze and troubleshoot access to resources

In this task I needed to troubleshoot the access to resources. 

After pasting the public IPv4 adrress it would not load.

First I needed to describe the instance using the 'ec2 describe-instances' command

This returned a long list describing all properties of the instance. Next I ran with smaller amount of properties queried with the command 'aws ec2 describe-instances --filter "Name=ip-address,Values='<WebServerIP>'" --query 'Reservations[*].Instances[*].[State,PrivateIpAddress,InstanceId,SecurityGroups,SubnetId,KeyName]'
In this command only the query part is changed.

Now the list was a lot more simple.

![kuva](https://github.com/user-attachments/assets/da431463-371e-4a54-a6a2-7d1d0b994e3e)

After trying to connect to the host it got timed out.

For troubleshooting the instructions recommend to use nmap. I will not do this, because I don't want to accidentially scan a wrong network. I will try to only use the 'ec2 describe-security-groups' command and others.

After running the command with the correct flag --group-ids parameter I got a better look at things.

![kuva](https://github.com/user-attachments/assets/61226616-0cc5-4494-96b7-fd5066b3586c)

However it seems that there is no connection allowed to port 22 and 80, only from. I don't know yet if it matters.

Next I looked at the route tables with command 'aws ec2 describe-route-tables --filter "Name=association.subnet-id,Values='subnet-0ee36117ae7ed1ec0'"'

This is another basic describe command with the subnet value being the identifier. In there, I noticed that the route table only has route for the local traffic, none for the internet. A new route needs to be added.

![kuva](https://github.com/user-attachments/assets/16188ccd-0b18-467d-9535-670bfceabd43)

To create a route I needed the route-table-id and gateway-id. I got the gateway-id by using the command 'aws ec2 describe-internet-gateways'

Here I saw that the gateway with the same VPC-id and name as before was attaced. It had the id of  
igw-07da79316bece53f9

![kuva](https://github.com/user-attachments/assets/d2c543ca-5852-4aec-b1f7-77e203354de2)

I also got the route-table-id from the query done earlier. It was rtb-0e3edbf3eca3c73f2

Next I created a command 'aws ec2 create-route --route-table-id rtb-0e3edbf3eca3c73f2 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-07da79316bece53f9'

I had the cidr-block as 0.0.0.0/0 as I wanted all non local traffic to be routed to the internet gateway.

![kuva](https://github.com/user-attachments/assets/e4785687-74a9-4811-84ba-f49b02ef5fcc)

The output was true meaning the request succeeded. I ran the describe-route-tables again.

![kuva](https://github.com/user-attachments/assets/4c6719dd-e624-47da-9cd1-ed5bc434d098)

Now there was a new route created.

![kuva](https://github.com/user-attachments/assets/9c5b7735-1531-42e6-b642-8da6c768d22e)

Now the page loaded.

![kuva](https://github.com/user-attachments/assets/069ac732-78b2-48be-8504-ed81beb1b3f4)

### Troubleshooting task 2

Next I had to fix the SSH connection. The SSH problem was not solved allthough the HTTP issue was.

![kuva](https://github.com/user-attachments/assets/6cae4b07-157c-45f3-9d03-26450c31f4af)

I was instructed to look at NACL or network access control lists of the subnet.

I had to look up what was the difference between a route table and NACL because they seemed to do the same thing in blocking traffic.

The difference described in a Stack Overflow [forum post](https://stackoverflow.com/questions/60211533/what-is-the-diference-between-network-acl-and-route-tables-in-aws)  is that all traffic is checked against subnet's NACL rules and is working effectively as the firewall of the subnet. Route table only routes the traffic. If no route is found the packet is dropped so in a way they work in very similar way. Both need to allow traffic for it to go in or out the subnet. NACL is usually used for controlling larger entities like blocking all traffic from a certain IP address range or blocking some network traffic type for all interfaces inside the subnet.

I ran the command 'aws ec2 describe-network-acls --filter "Name=association.subnet-id,Values='subnet-0ee36117ae7ed1ec0'" --query 'NetworkAcls[*].[NetworkAclId,Entries]''

And in the output I saw an issue. 

![kuva](https://github.com/user-attachments/assets/e3863933-e72a-4320-bb6f-e7f59dcd29a1)

Rule 40 denied traffic from port 22.

I ran the command 'aws ec2 delete-network-acl-entry --network-acl-id acl-01a31ccc0946740bc --rule-number 40 --ingress'. I got the parameters from the query before.

After running the command 'describe-network-acls' the rule was gone.

![kuva](https://github.com/user-attachments/assets/dcab89c3-d7c1-4f52-89e1-0fb66eb58d50)

Next I tried to connect via SSH again.

After spending half an hour figuring out what was wrong with my settings, I noticed I had written my IP wrong in the ssh command. Now I got in.

![kuva](https://github.com/user-attachments/assets/72361ac3-c07b-431f-b873-2fb5fe60826a)

So the ACL was blocking traffic to and from port 22.

### Task 4 Analyze flow logs

Next I downloaded the logs from S3 by using command 'aws s3 cp s3://<flowlog4444>/ . --recursive'

![kuva](https://github.com/user-attachments/assets/a2e6a7ae-35d4-44eb-8ef5-0586c9a641e0)

I went as deep as I could using my tab key.

![kuva](https://github.com/user-attachments/assets/6ef6286c-ae47-4e10-af4e-42fdd89345e4)

I ran the command 'gunzip *.gz' and unzipped all the files.

Next I ran some grep commands to show all rejected logs and the ones that tried to connect port 22

![kuva](https://github.com/user-attachments/assets/cf357f92-fe8b-489c-873a-72c0caa75e5a)

Next I needed to check when I had failed to log in. I got my IP from the VPN I was using and used the command 'grep -rn ' 22 ' . | grep REJECT | grep <ip-address>'

For some reason no logs were found. Could be caused by my VPN.

The grep command only gave me data from different IP addresses. I do not know why. However just looking at the logs gave me data from the right interface id.

![kuva](https://github.com/user-attachments/assets/c8d0c493-4c6c-4c81-b105-b703f86c9146)

At this point my lab time ran out and I could not access the instance any more.

After thinking for a while, I realized that the reason my IP address would not show on the "REJECTED" filter was because I had tried to connect to the wrong IP-address and only corrected the IP after I had already solved the issue on the route table and ACL.

