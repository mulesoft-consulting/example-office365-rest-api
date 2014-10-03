# Example: Office365 REST API

## Content

+ [Summary](#summary)
+ [About the example](#about-the-example)
+ [Demo environment configuration](#demo-environment-configuration)-
	* [Set up your Office 365 APIs Preview development environment](#set-up-your-office-365-apis-preview-development-environment)
		* [Get Office 365 developer site ](#get-office-365-developer-site)
		* [Set up azure subscription ](#set-up-azure-subscription)
	* [Configure Mule project ](#configure-mule-project)		
* [Run it](#run-it)		
* [Technical dives](#technical-dives)		
* [Notes](#notes)		
		
		
## Summary
This is an example that shows how to interact with **Office365** using the **official REST Api**.

Key value of this example:

- How to authenticate against Azure AD without user interaction need
- How to request authorization to interact with Office365
- How to interact with the API. In this case, we are gonna work with **Contacts**

##About the example
In order to make easier the example testing the project has been implemented with API Kit with examples for each operation.

The implemented operations to interact with contacts are:

<table cellspacing="0" cellpadding="5" border="1">
<tr><th>Operation</th><th> Http Method</th></tr>
<tr><td>Create a new contact</td><td> POST</td></tr>
<tr><td>Update an existing contact</td><td> PATCH</td></tr>
<tr><td>Delete an existing contact</td><td> DELETE</td></tr>
<tr><td>List all the existing contacts</td><td> GET</td></tr>
</table>


**NOTE**: the reason why the **update** operation was implemented using **PATCH** method is because that one in Office365 REST Api is implemented with that method. So, no because is required but to keep the same HTTP method in Mule inbounds and outbounds.  

## Demo environment configuration

### Set up your Office 365 APIs Preview development environment

1. Get Office 365 developer site
2. Set up Azure subscription

Follow the steps in the official site: http://msdn.microsoft.com/en-us/library/office/dn605899(v=office.15).aspx

#### Get Office 365 developer site
Basically you need to get a Office 365 developer account which will allow you two things:
- Allow you to interact with the REST API
- The user interface where you usually can interact to manage contacts, calendars, etc.

For the registration you will have to select a company name and a username.

With the company name you will have registred a domain for https://{company_name}.onmicrosoft.com.

#### Set up azure subscription

Once you have your Office 365 developer account, you need to configure an Azure subcription. This is where you will configure a new application which will be the bridge to interact with the REST API to the Office 365 account.

Follow the steps to register the account. 

Now you need to **register the application**, to put it simple, follow the next steps:

1. Go to active directory
2. Enter to the subscription
3. Go to applications
4. Click on "add" a new application  
5. Select the option "Add an application my organization is developing"
6. Type the name of your application. Since now **{app_name}**.
7. Select for the application type **WEB APPLICATION AND/OR WEB API**
8. Next step are the App properties:
	- SIGN-ON URL: https://{company_name}.onmicrosoft.com/{app_name}
	- API ID URI: https://{company_name}.onmicrosoft.com/{app_name}

Next step to configure the application:

1. Go to the application you already created
2. In **permissions to other applications** section you have to add permission for Office 365
	- Select **Office 365 Exchange Online**
	- In **Delegated permissions** check the options:
		- Have full access to users' contacts (preview)
		- Read users' contacts (preview)
3. Save the changes
4. Copy the client_id which we will need later.
5. In **keys** section select duration 1 year and save the changes
6. It will appear the client_secret, copy it.


### Configure Mule project
Open the mule project located in /code

So far, we have this information we got from Office 365 account and Azure subscription:
- username
- company_name
- password
- client_id
- client_secret

Go to mule-app.properties and replace the values for ones you have and save it.

## Run it

You are ready to execute the example. 

Run it and enjoys! 

## Technical dives

There are three flows in the project:

- **api.xml**, contains the inbound for the Mule API which we will use to interact with
- **office365.xml**, contains the flows that interact with Office365 Rest API
	- For each one, first we retrieve an access token which will give us authorization to interact with Office365 API. Specifically we are requesting access for the resource **https://outlook.office365.com**
	- The we will send the access token in the header of each request for each operation.
	- For the proposal of the demo we request the access token all the time, but you can store it and use it until it expires. That information comes from the access token request.

## Notes

- This example was built without any extra connector more than Http, so you don't need to install anything extra
- This was tested on Mule ESB 3.5.1 and Cloudhub
- The Office365 has an error because we are using in a http outbound the http method "PATCH". Ignore it.