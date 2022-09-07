# Fetching Azure Active Directory's users list using Azure Logic app

## What is Azure Logic Apps?

[Azure Logic Apps](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview) is a cloud service platform for creating and running the workflow at ease consuming a range of APIs exposed as Connectors. These Logic App connectors will perform the sequence of actions defined in the workflow whenever the trigger gets fired. Further more you can quickly develop highly scalable integration solutions for your enterprise and business-to-business (B2B) scenarios. 

## Triggers
[Triggers](https://docs.microsoft.com/en-us/azure/connectors/built-in) are the starting point for a Azure Logic App workflow that will fire when new data or event that meets the trigger condition occur. Connectors in Logic Apps itself provide various triggers. Custom triggers can also be created using custom connectors.

## Actions 

The [Actions](https://docs.microsoft.com/en-us/azure/connectors/built-in) performs the business operations when input is provided through the managed connectors. Each time the trigger fires, the Logic App engine creates an instance that runs the actions in the workflow. These actions can also include data conversions and flow controls, such as conditional statements, switch statements, loops, and branching.



## Azure Active Directory Graph API
The [Graph API](https://www.kuppingercole.com/blog/kuppinger/azure-active-directory-what-is-the-graph-api#:~:text=The%20Graph%20API%20of%20Azure,groups%2C%20and%20other%20common%20entities.) of Azure AD provides a broad set of standard queries that can be used to retrieve metadata information about the tenant’s directory and its data structure, but also about users, groups, and other common entities.


## About this exercise

Previously we have already registered an APP in Azure Active Directory.

## In this exercise

 * We will get client secret token, ClientId and TenantId of registered App from Azure active directory.
 * We will create a logic app.
 * We will create a trigger to initiate the logic app.
 * We will initialize required variables.
 * We will get the access token using the required variable.
 * We will fetch Users information using access token.

---------------------
 ------------------

 ## Step 1: Getting Required Information

 First we will get all required information( Tenant ID, Client ID, Client Secret, etc ) to create our Azure logic app.
 
To get the Client secret token go to registered App and select **Certificates & Secrets** then click on the **Client Secret** tab and **copy** token from value column as below
![2](https://user-images.githubusercontent.com/100709775/168145372-8f3868b1-2fc4-4364-a6d2-171c2345a029.PNG)


Now get the **ClientId & TenantId**. For this go to registered App in Azure AD then click **overview and copy** the ClientId & TenantId values as below 

![3](https://user-images.githubusercontent.com/100709775/168145782-67887034-4979-478d-98ea-9edfa04d7c30.PNG)

**Save all values in the notepad and we will use these in later steps.* 

---------------


## Step 2: Create Azure Logic App

Create a new resource and select Logic App. Select the relevant Subscription and resource group, enter the logic app name and click **Review + Create** button as below 

![4](https://user-images.githubusercontent.com/100709775/168146805-87cb30dc-3c23-4068-b6fb-60487a67355f.PNG)

After creating logic app click on go to resource and then under templates create **Blank Logic App**.

![11](https://user-images.githubusercontent.com/100709775/168643642-958d7fb3-2da4-4613-9d72-2eceefdd4b9c.PNG)

---------------

## Step 3: Create Http Trigger

First we will initialize the logic app by adding first step. For this search the **Http triggers** and select **HTTP (When a HTTP request is received)** as below 

![22](https://user-images.githubusercontent.com/100709775/168644708-2f73b620-5b95-4940-9249-84676a6dd1d8.PNG)

Now we will create **JSON schema** for empty array. 
For this:
- Go to this **link** https://jsonschema.net/app/schemas/0
- Create **empty array** and click **Submit** button. 
![](/img/1.png)
- **Copy** the created json and **paste** it in **HTTP trigger's** Request Body JSON Schema as below 
![33](https://user-images.githubusercontent.com/100709775/168646161-bca61d2f-e42e-4162-8088-925bb9d9c3c0.PNG)

After thus step, **Save** and it will give you POST URL. 

-------------------

## Step 4: Initialize Action Variables

Now we will **Add New Steps**

At this stage we will create **environment variables** from the information we saved in step 1. For this initialize the 3 variables which are client secret token, ClientId and TenantId. To create a initialize variable search **variable** and select **Initialize Variable** as below 

![44](https://user-images.githubusercontent.com/100709775/168646889-21874524-fe74-458a-b858-f2d650a5e03b.PNG)

Enter the variable name e.g. **`client_secret`**. select type as string and provide the client secretId which is fetched from the azure active directory.



The three variable will look like as below :

![66](https://user-images.githubusercontent.com/100709775/168743081-f70552c0-8e26-4a6d-915b-5cb479ca598b.PNG)

***Save** these steps before moving to next step*

---------------

## Step 5: Get Access Token from Graph API

Now Create **Next Step** of an **HTTP Post** request to **https://login.mircosoftonline.com/{TenantID_we_created}/oauth2/** token
- Now we will pass these variables values in **headers** tags as below. we will use this value in later steps
    ```json
    // headers Key
    Content_Type
    // Headers Value
    application/x-www-form-urlencoded
    ```
- We will pass `client ID` and `client_secret` in the **Body** by this code
    ```
    grant_type=client_credentials&client_id=@{variables('Client_ID')}
    &resource=https://graph.microsoft.com&client_secret=@{variables('Client_Secret')}
    ``` 

![77](https://user-images.githubusercontent.com/100709775/168744910-9f1d73cb-c8ee-4abd-9310-10e4913f9548.PNG)

- Now we will create a similar POST request in **Postman** to fetch the token. For this pass similar values **Headers** and **Body**. 

- In body we will also pass **Scope Key as `scope`** and **Scope Value as `https://graph.microsoft.com/.default`**

- **POST request** will be to this URL
https://login.microsoftonline.com/your-tenant-id/oauth2/v2.0/token

- After adding all values **Send** request and copy your response
![](/img/2.jpg)

- **Paste** response and **create schema** through https://jsonschema.net/app/schemas/0 
![](/img/3.jpg)
- **Copy this Schema** to use in the next step


## Step 6: Parse JSON and Fetch Access Token

Now come back to Azure Logic APP and Create a **parse Json** action, Select **Body** tag in content.
**Paste** the token response schema in parse json schema body as below 


![99](https://user-images.githubusercontent.com/100709775/168748034-98119230-e663-44d4-83e1-e059aca89ecf.PNG)

**Initialize** a new **variable** to get the access token from Json response as below. 

![10](https://user-images.githubusercontent.com/100709775/168748421-7bd87002-0971-4bdb-abd0-3afd163d85b2.PNG)


## Step 7: Fetch Users using Graph Api

In this step we will pass this access token to fetch all users information.

For this **Create** an **Http Get request** with url https://graph.microsoft.com/v1.0/users to get the users list. Pass the `access_token` in authorization header tag as below 

![11](https://user-images.githubusercontent.com/100709775/168748794-47fbd5c4-4bfc-416d-899e-46654925de7e.PNG)


Create a **response action** to get the response body as below 

![133](https://user-images.githubusercontent.com/100709775/168749468-ff82f157-fb8d-40a6-8794-f34d08f5380a.PNG)

------------------------
## FINAL OUTPUT

You can check working by 2 ways
1) Run this trigger manually as shown below and see the response in the last step. 
![](/img/5.png)

2) **Copy** the **URL** from the first step and create a **POST request** in **POSTMAN** and see the response as shown below
![](/img/6.png)










                  

 
