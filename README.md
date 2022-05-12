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

Previously we have already registered an App in Azure Active Directory.

## In this exercise

 * We will get client secret token, ClientId and TenantId of registered App from Azure portal.
 * We will create a logic app.
 * We will create an http action to get access token.
 * We will fetch Users information using access token.


 Here are the steps to begin with 

 ## Step 1: Getting Required Information
 
To get the Client secret token go to registered App and select `Certificates & Secrets` then click on the Client secret tab and copy token from value column as below
![2](https://user-images.githubusercontent.com/100709775/168145372-8f3868b1-2fc4-4364-a6d2-171c2345a029.PNG)


To Get the ClientId & TenantId go to registered App then click overview and copy the ClientId & TenantId values as below 

![3](https://user-images.githubusercontent.com/100709775/168145782-67887034-4979-478d-98ea-9edfa04d7c30.PNG)

## Step 2: Create Azure Logic App

Create a new resource and select Logic App. Select the relevant Subscription and resource group, enter the logic app name and click `Review + Create` button as below 

![4](https://user-images.githubusercontent.com/100709775/168146805-87cb30dc-3c23-4068-b6fb-60487a67355f.PNG)



                  

 
