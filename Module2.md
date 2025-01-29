# Lab 1 Using AWS Systems Manager

![kuva](https://github.com/user-attachments/assets/85348011-959a-4396-b0e6-13617ea6229c)

Found the managed instances in System Manager Service under "Fleet Manager"

![kuva](https://github.com/user-attachments/assets/bffb9572-af1f-4161-a6ec-632d236cdfc5)

Set the targets

![kuva](https://github.com/user-attachments/assets/0f95c8dc-f2ea-44a1-aad9-af639e75f0a1)

Got the collected metadata of the applications.
What was done in the first task was creating an inventory and attaching an instance to to collect data from it.

## Task 2

I started by going back to the main menu of AWS System Manager window

![kuva](https://github.com/user-attachments/assets/803af4e7-604c-451f-9308-9e3548b6be21)

Next I filtered by giving the prompt "Owner" to the search bar after which a drop down menu appeared.

![kuva](https://github.com/user-attachments/assets/dcb90e6d-ff2d-40ac-acd4-6022358a00db)

Targets and documents set.

![kuva](https://github.com/user-attachments/assets/9e7e49e3-cebc-4c05-9f02-a129ae71824d)

After running the command I could check it was correctly installed by opening the public IP address of the server.

## Task 3

![kuva](https://github.com/user-attachments/assets/b62a8b1a-48f5-4939-b9c8-4e012435bfa2)

Created a parameter.

![kuva](https://github.com/user-attachments/assets/f13c1341-1f18-4c8c-96a6-e5734430b657)

Now the dashboard shows a new window.

Note to self: Read more about Parameter store and how it can be used. Apparently it is a dictionary of data that can be accessed only through parameters.

## Task 4
I gave opened the instance as instructed and gave the necessary commands:

![kuva](https://github.com/user-attachments/assets/5bc89c7c-e07e-41fe-a430-de027f9f2da2)

The purpose of this activity was to show how session can be accessed without SSH.

# Activity 2 Create a Website on S3

I started by trying to use my Debian 12 virtual machine and SSH this time.
I downloaded the SSH .pem key, changed the permissions and started the SSH connection.

![kuva](https://github.com/user-attachments/assets/500a9145-31f0-4862-a857-8654db663b93)

After that I configured the aws console:

![kuva](https://github.com/user-attachments/assets/c4a467ba-ad8e-42d4-b412-5319482dd597)

After reading through the documentation and activity instrucions I found that I needed the flags --bucket to change the name and region restrictions.

![kuva](https://github.com/user-attachments/assets/643aeea6-a760-4231-864d-1cb6a15cf2a6)

After that I found the user and policy.

![kuva](https://github.com/user-attachments/assets/cfd1c5cf-e99a-4fd2-84e4-3725b1ea10e6)


Following the link found in Security details brought me to a log in site. I used the credientials given in the instructions.

![kuva](https://github.com/user-attachments/assets/35ca2c58-8773-4c51-8e26-f642ccfcd890)

The EC2 page is full of errors due to missing priviledges.

![kuva](https://github.com/user-attachments/assets/f54e10b8-5f98-4a16-bdda-8c7a5fec739c)

Following the link I can see my created bucket.

![kuva](https://github.com/user-attachments/assets/5e1fde3f-d0d4-43d8-a852-746ff90a92a0)


Going back to the CLI account I extracted the tar file and after that deleted it.

![kuva](https://github.com/user-attachments/assets/e89a7e8d-fecd-4afe-8a09-aececc762f07)

In the next part I did a lot of things. First I creates a prefrence so all items added to the bucket are owned by the bucket owner.
The  Next command allows for public access. The third configures the bucket as a static website and the last command copies the files in the current folder to the bucket.

![kuva](https://github.com/user-attachments/assets/0c4e3b5e-c86c-4461-8aa0-30bf824b262a)

Going back to the S3 page I can see that static website hosting is allowed.

![kuva](https://github.com/user-attachments/assets/97b52a47-9f79-4a76-a3f2-10e6392036c7)


Following the link, the website now works.

![kuva](https://github.com/user-attachments/assets/088ac556-c5e7-4bae-aa39-58a3c9f20188)


Ï created the file and added it's code.

![kuva](https://github.com/user-attachments/assets/0f81dc50-0339-4ed3-93a9-71fa65e8869e)

Created the file and changed permissions so it can be executed. Note to self, never use Vi again.

![kuva](https://github.com/user-attachments/assets/7440f1ae-26f8-4186-8257-cfb7213c3c0b)

I used nano to find and change the colors.

![kuva](https://github.com/user-attachments/assets/1b9aabf8-13d9-4a0b-8cb1-3913a3e771d6)


I ran the command update-website.sh

![kuva](https://github.com/user-attachments/assets/5a9b770f-8bd6-4b5f-92a9-03df43a5cd68)

The colors had changed and the deed was done.

![kuva](https://github.com/user-attachments/assets/27d2f660-1cef-44ad-b8fc-f697dcfd74b0)


# Conclusions

There were a lot of new things in this module but the logic is fairly simple so far. The biggest problem I think will come is the complexity of the aws commands. The documentation will come very familiar to me I think.

*Panu Peltola*






























