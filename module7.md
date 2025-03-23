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










































 















