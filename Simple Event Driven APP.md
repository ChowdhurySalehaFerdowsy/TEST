In this project I have created a simple serverless event-driven app using AWS SQS, Lamda and DynamoDB. 
After completing this project, I can send messages using AWS CLI to AWS SQS and whenever SQS has any 
item arrived in its queue, it will trigger Lamda function and send it to DynamoDB table and CW logs.

---

**STEPS**
1. Install AWS CLI and configure for the AWS account
- Use your account in AWS 
- 	Install AWS CLI in your machine
- Configure AWS CLI using access key 
- Set region name : <us-east-1>
- Default output can be left empty

2. Go to AWS DynamoDB and create table 
- Table name : <ProductVisits>
- Partition key: ProductVisitKey

3. Go to AWS SQS and Create SQS Queue
- Select Type as Standard
- Table name : ProductVisitsDataQueue
- After creating the queue, note the queue URL
4. Go to AWS Lamda and create function
- Type Name: productVisitsDataHandler
- Select Runtime: Node.js 12.x
- From Role section select : create new role from templates	
- Put Role name as  lambdaRoleForSQSPermissions
- We will add two policies for this role : "Simple microservice permissions" and "Amazon SQS poller permissions" and create function
- After creating Lamda function we will upload a zip file . In the action menu there is a ‘upload’ option , select zip file there.
- Upload a zip file (DCTProductVisitsTracking.zip)
> At this point we will give a command in AWS CLI to see if SQS can poll messages 

5. Go to AWS CLI and send messages to SQS:
- In your computer go to the file location where all the codes has been saved.  
I am using windows OS , so I used “cd” command to go the location and “dir” command to see the listing
- Once you are in that file location use AWS CLI Command as follows : 
- In the *QUEUE URL* of the command use the SQS URL copied earlier from SQS  
It will return something with message ID , it means it is working 
6. Configure SQS to trigger Lamda function
- Go to SQS queue and go to the ProductVisitsDataQueue. At the right side there is option from poll messages. Select Poll message.   
   - One message will be received . If we click on the message we can the details of the message including the message of the body  
   - We can do this multiple times changing the command in AWS CLI . For example instead of message-body-1 we can type message -body-2 and so on.
- Go to Lamda trigger and configure Lamda function . Specify Lambda function with name: productVisitsDataHandler
- Now again poll for messages and we will see no message is receiving . This is because we have configured Lamda function and it is now sending the messages in DynamoDB table.

7. Find items in DynamoDB table 
    - Go to AWS Dynamo DB and select explore items from left menu
    - Select ProductVisits table we can see items in the DynamoDB table 

8.	Find log stream in CloudWatch Logs
- Go to AWS cloud watch and click on log groups.
- Select the log stream that has a name with productVisitsDataHandler and open it. We will see all the events for each invocation from Lamda


    