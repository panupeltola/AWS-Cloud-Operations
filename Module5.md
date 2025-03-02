# Activity 5

The goal of this activity was to configure a serverless Lambda function that generates a sales analysis report from database and emails.

The steps to do this were described in the instructions as following:

    [1] An Amazon CloudWatch event triggers the salesAnalysisReport Lambda function at 8:00pm every day, Monday through Saturday.
    [2] The salesAnalysisReport Lambda function invokes another Lambda function, salesAnalysisReportDataExtractor, to retrieve the report data.
    [3] The salesAnalysisReportDataExtractor function runs an analytical query against the Mom & Pop Café database (mom_pop_db).
    [4] The query result is returned to the salesAnalysisReport function.
    [5] The salesAnalysisReport function formats the report into a message and publishes it to the salesAnalysisReportTopic Amazon Simple Notification Service (SNS) topic.
    [6] The salesAnalysisReportTopic sends the message by email to Pop.


  # Task 1

  First step I did  was to look at the security role "salesAnalysisReportRole"

  In it I could see the permissions to create logs and run lambda functions.

  ![kuva](https://github.com/user-attachments/assets/1bda1d50-67fe-4bc2-9630-1c4332ebde40)

  One thing that crossed my mind was, if it is actually a good idea to give this large of permission to run functions, could be used by a malicious actor who got foothold of the role for example.


  # Task 2.1

  I navigated to Layers in Lambda page and clicked on "Create layer"

  While choosing the runtime, I noticed that Python 3.8 told in the instructions is no longer supported so I tried to just choose a newer version of python. I chose Python 3.13.

  
# Task 2.2

Next I created a function using the Create function button. I created the function using the options from the instructions. I chose the runtime to be Python 3.13 as I chose it before in the last part.

After succesfully creating the function I added the layer created earlier from the "Add layer" button.

![kuva](https://github.com/user-attachments/assets/f2e53dfc-d67d-41d3-b809-4423dd6efff2)

*Change Handler name*

![kuva](https://github.com/user-attachments/assets/2490b8a0-631d-4fe4-a05e-df980bd155f1)


Next I uploaded the file earlier to the code source:


![kuva](https://github.com/user-attachments/assets/ea7673db-f322-4eee-97f5-b9e10e746778)

The code seems to be a simple SQL query from the database using credidentials it got from the connection.

# Task 2.5


*VPC edited in Lambda*

![kuva](https://github.com/user-attachments/assets/95b1d8a6-ab15-4186-9ffa-2f1c9fc02322)


For later use I will now save the values from the parameter storage here:

dBUrl:ec2-44-197-239-200.compute-1.amazonaws.com
dbName:mom_pop_db
dBUser:root
dBPassword:re:St@rt!9

Next I created the test event in Lambda:

![kuva](https://github.com/user-attachments/assets/db6b0fd7-60ae-42e9-b916-6b6e9ea85238)

Note, after running the test got an error, because I forgot to put commas to seperate the JSON.

![kuva](https://github.com/user-attachments/assets/2482306f-7e3f-487c-966e-7fe5836b74a3)

After the fix I got an error about the dbUrl being wrong and the possibility it could be caused by runtime. I tried to fix it to Python 3.9 in case this would fix it.

![kuva](https://github.com/user-attachments/assets/019aadc2-bac9-4813-a52e-f86a5e7aa214)

*New version of layer*

![kuva](https://github.com/user-attachments/assets/8e54dced-0182-4ffb-964b-6a462edfc135)

*Runtime and layert updated in Lambda*

The same error persisted. I had no idea how to fix it, I tried to copy paste the code from the instructions and now it worked. Probably something to do with spacing.

![kuva](https://github.com/user-attachments/assets/aa93b641-2a2f-42da-909b-510f96b8ccfb)

Now I got the error from timing out after 3 seconds.


For the troubleshooting, the port to MySQL seems to not be included in the inound rules.

I allowed connection to this port from all addresses (bad for security) and after that I tried running the test again.

This time the test succeeded:

![kuva](https://github.com/user-attachments/assets/5571f4cd-ed3f-4f80-9ec2-d4c4843f0be4)

After placing an order it showed on the test

![kuva](https://github.com/user-attachments/assets/d88f5eaf-13a8-44cb-b1e7-272aa2189c98)


# Task 4

I created the topic and got ARN for it: 

arn:aws:sns:us-east-1:033801977266:salesAnalysisReportTopic

*Subscription created*

![kuva](https://github.com/user-attachments/assets/b6958337-7ca8-4c2e-8884-0170516c1651)

*Subscription accepted*

![kuva](https://github.com/user-attachments/assets/2adc8e3a-70f5-46db-862b-9556c5f77679)

# Task 5

*conncected and configured*
 
![kuva](https://github.com/user-attachments/assets/ea070383-68d2-402a-9075-0dc611093d9d)


Next I created a new function by following commands:

![kuva](https://github.com/user-attachments/assets/67e3804b-ef82-4556-adde-f101a51c86f3)

It also shows in my Lambda functions.

Coinfiguring this new function I created an eviromental variable for it using the topic I created earlier.

![kuva](https://github.com/user-attachments/assets/bc67e03a-9b1e-4a7f-9b3a-f87e6221a902)

*Test succesful*

![kuva](https://github.com/user-attachments/assets/2bc4f09b-2375-4253-ba4a-8055ac7f5496)

*E-mail recieved with the sales report*

![kuva](https://github.com/user-attachments/assets/b3b1f854-c049-4990-bbbe-086e4e919caf)

Trigger:

![kuva](https://github.com/user-attachments/assets/240b909d-9b89-4243-90f5-fdd27db81a9d)




In this Activity I learned how to use Lambdas. It seems to be very simple tool to create a lot of features without a server. 

*Panu Peltola*































































  

  

  
