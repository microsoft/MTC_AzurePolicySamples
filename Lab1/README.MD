# Lab 1 – Configure Services and Use Postman
WELCOME! Here are some tips-and-tricks you can use while in the workshop
1.	Open a Notepad text document and save it. This is where you can copy-and-paste variables, connection string, credentials vs. having to refer to another web page for those values
2.	Open a browser session to have as your Azure portal and Azure Cloud Shell
3.	Have a different browser session open to the GitHub repo with all the lab instructions

## Lab Goals
The goals of this lab are to configure services and the Postman desktop application for automating Azure Policy

### Learning Objectives
- Use the Azure Portal to create resources
- Configure Postman for accessing the Azure Policy APIs 
- Familiarize yourself with the REST APIs and their structure
- Run and test various Azure Policy REST APIs

## Requirements
- An Azure subscription for which you have the Owner role
- A PC which you can install applications on

### Clone this GitHub Repository
There are several files you will use from this repository, so to ease access you will clone the repository to your local machine.  In this workshop, you will not be creating your own GitHub repository from the workshop cloned repository; you can do that work later if desired.
1.	Go to https://aka.ms/mtcazurepolicysamples
2.	Select `Clone or download` from the left side
3.	You can either download the .ZIP file and extract locally or you can copy the link or clone the repository on the command line.  If you are using GitHub Desktop, you can clone it by selecting `Open in Desktop`
4.	To clone using the command line, open a CMD shell and change directory to where you want the repository cloned

`git clone <paste in the copied repository link from the above step>`

### Create an App Registration 
The App Registration will provide access from your client code to the Azure Policy management APIs.  When you create an App Registration, it also creates a Service Principal.  In this case, you are creating an App Registration given it's flexible to allow multi-tenant access; whereas, a Service Principal alone is bound into the Azure tenant where it was created.
1.	From the Azure Portal, select `Azure Active Directory` and then select `App registrations`
2.	Select `+ New registration`
3.	Check the radio button for `Accounts in this organizational directory only`
4.	In the `Redirect URI`, add a URI of type `Web` with the value `https://www.getpostman.com/oauth2/callback`
5.	Select `Register`
6.	Once the App Registration has been created, select its `Authentication` 
7.	In `Implicit Grant`, select both `Access tokens` and `ID tokens`
8.	Leave `Default client type` to `No`
9.	Leave `Supported account types` to `Accounts in this organizational directory only`
10.	Select `Save`
11.	Select `Certificates & secrets`
12.	Select `+ New client secret`
13.	Leave it to expire in 1-year, provide a meaningful `Description`
14.	Select `Add`
15.	**Important!**  Your new client secret will be shown to you only once after which you will not be able to copy its value.  Select the Copy icon to the right of your client secret and store it in a Notepad file
16.	Select `Overview`
17.	Copy and paste the `Application (client) ID` into your Notepad file
18.	Copy and paste the `Directory (tenant) ID` into your Notepad file

You do not need to add any permissions to the API other than its default of User.Read, and you do not need to change the manifest.

### Add Subscription Role to the App Registration
1.	In the Azure Portal, select `Subscriptions` and then select your subscription
2.	Select `Access Control (IAM)` and then select `+ Add`
3.	Select `Add role assignment` and search for ‘Resource policy contributor’  
4.	Select this role and then chose to add to `Azure AD user, group, or service principal`
5.	Search for the name of the App Registration and select it
6.	Select `Save`
7.	Select `Overview`
8.	Copy and paste the `Subscription ID` into your Notepad file

Your App Registration can now act on Azure Policy

### Configure Postman
Postman is a great tool for API development, and you’ll be using it to call the Azure Policy REST APIs.  Once you have the right API calls with the right behavior, you can use them in your Python code for automating the process (a lab later in this workshop). 
1.	[Install Postman desktop]( https://www.getpostman.com/downloads/)
2.	You will be required to create an account, but you only need to use the free version for this workshop
3.	Once logged in, you will be presented with the Postman desktop
4.	On the top, select the Down Arrow next to the `Invite` icon
5.	Select `Create new` to create a new workspace.  A Postman workspace can have one or more collections of APIs organized into a logical group
6.	Give it a `Name` like ‘Azure Policy Workspace’.  Provide an optional `Summary` if you like
7.	Select the `Team` type of `Personal`.  Note if you select `Team` and invite people, Postman’s free version limits the number of collaborative team members you can have before you must upgrade to a paid version.  For this workshop, a personal workspace is all you need
8.	Select `Create Workspace`
Now that you have a workspace, you will import the Postman Environment and Collection for the Azure Policy API management.
1.	In Postman, select `Import` from your newly created workspace.  If you accidentally left your workspace or closed Postman, you can open your workspace using the Down Arrow at the top next to the `Invite` icon
2.	From `Import File`, select `Choose Files`
3.	Open the folder where you cloned this repository and then open the `Postman` folder
4.	Select the file named ‘Azure Policy.postman_environment’
5.	In the upper right of the Postman desktop, you’ll see a control that states `No environment`.  Select the down arrow and then select the Environment `Azure Policy`
6.	Select the `Environment quick look` icon next to the Environment drop down (it looks like an eye)
7.	Select `Edit`
8.	From your Notepad file, you will copy the saved values into the Environment variables
9.	Copy your Application (client) ID into ClientID `Current Value`
10.	Copy your App Registration client secret into ClientSecret `Current Value`
11.	Copy your Directory (tenant) ID into the DirectoryID `Current Value`
12.	Copy your Subscription ID into the SubscriptionId `Current Value`
13.	Leave all other values as they are
14.	Select `Update`

You now have created a Postman Environment that contains variables that will be used when making the REST API calls.  Next you will import the Postman Collection of pre-built API calls into your workspace.

1.	In Postman, select `Import` from your newly created workspace.  If you accidentally left your workspace or closed Postman, you can open your workspace using the Down Arrow at the top next to the `Invite` icon
2.	From `Import File`, select `Choose Files`
3.	Open the folder where you cloned this repository and then open the `Postman` folder
4.	Select the file named ‘Azure Policy Management.postman_collection.json’
5.	Once imported, select `Collections` on the left pane and you will see the Collection named ‘Azure Policy Management’
6.	Select the arrow next to your new Collection to see the pre-built API calls

### Understanding OAuth and the REST APIs
Now you have both the Environment and the Collection needed to query the Azure Policy Management APIs.  It’s helps to understand OAuth.  In calling the Azure Policy REST APIs, it uses an OAuthv2 implicit grant flow.  Recall earlier when you created your App Registration, you set the token types that will be accepted for the Implicit grant, i.e. access tokens and ID tokens.  Even though you’ve configured a Postman and an App Registration, you don’t have an access token yet to make any REST API calls.  Fortunately, this has been built for you, and it is part of the Postman Collection you just imported.  

Look at how this works and know that you can use this same method in other Collections to setup your access tokens.
1.	In Postman, hover over your ‘Azure Policy Management’ Collection and select the ellipses `…` button
2.	Select `Edit`
3.	In the `Description`, you’ll see documentation for the Collection
4.	Select the `Pre-request Scripts` tab
5.	This code uses the Environment variables you just set to get an access token the REST API calls will use.  The access token is checked every 1-hour to see if it’s expired after which it will get a new access token – the value for the access token is updated in the ‘Azure Policy’ environment
6.	This code is run prior to any API REST call you make in this Collection ensuring your REST API calls will always have a current access token
7.	Select `Cancel`

To understand how this access token works, you’ll inspect one of the REST API calls in the Collection.
1.	Expand your ‘Azure Policy Management’ Collection to see the REST API calls
2.	Select the call `GET All Policy Definitions`
3.	In the REST API pane to the right, select `Headers`
4.	Look at the key named `Authorization` and its value ‘Bearer {{AccessToken}}’
5.	This value forms what the REST API expects.  It needs to be called with a key named ‘Authorization’ that contains a value in the format ‘Bearer <the access token>’
6.	Hover over {{Access Token}} and you’ll see it’s a value in the Environment ‘Azure Policy’ and you’ll see its current values
7.	Select `Send` on the REST API call
8.	JSON will be returned that forms the output of the REST API call
9.	Hover over {{Access Token}} and you’ll see its value has changed to the actual access token value.  The Pre-request Scripts code you looked at earlier ran and updated the AccessToken value in the ‘Azure Policy’ Environment
10.	On the far-right side, select `Code`
11.	A panel will popup showing the code that actually ran the REST API call.  You’ll see the AccessToken in the call
12.	You’ll see the use of Postman variables, e.g. {{variableName}}, in the REST API calls in this Collection.  It’s much better to use variables to access common values vs.  having to update many REST API calls with a new value vs. 

Although this seems like a great deal of work, once you get used to Postman and understand how OAuth works, it becomes much easier.  Plus you now have the benefit of a working environment that you can use to create new Postman Environments and Collections.

### Break down the REST API calls and their structure
The pre-built REST API calls illustrate how to work with the Azure Policy management layer, but they are far from complete.  It’s important you understand the structure of the calls so you can expand their use.
1.	You just ran the `GET All Policy Definitions` REST API call.  Now you’ll investigate it a bit deeper to understand what it is doing.  If you changed from this REST API call, that’s OK.  Just select the `GET All Policy Definitions` call again and select `Send`
2.	First look at the actual URL

    https://management.azure.com/providers/Microsoft.Authorization/policyDefinitions?$select=displayName&api-version={{AuthorizationAPIVersion}}

3.	What stands out immediately is another variable, {{AuthorizationAPIVersion}}.  If you hover over it, you’ll see it’s just a date.  All the Azure REST APIs have version numbers, and to make it easy, the version number is hosted in a Postman Environment variable
4.	On the URL line, highlight just {{AuthorizationAPIVersion}} and select Ctrl-X to copy-and-cut it
5.	Select `Send` and see the error that the version number is required
6.	Enter the value `2021-07-01` after the `api-version=` in the URL string.  It should look like `api-version=2021-07-01`
7.	Select `Send` and see that the version number you set isn’t supported and it tells you the supported values
8.	Remove `2021-07-01` from the URL and right after the `api-version=` start typing `{{AuthorizationAPIVersion}}`
9.	It will come up well before you have to type it out suggesting variables that look like what you’re typing
10.	Select the suggested variable named `{{AuthorizationAPIVersion}}` and select `Send` again and the REST API call works again
11.	From the REST APIs on the left, select `PUT Trigger Policy Evaluation` and look at the URL (do not send it).  You’ll see this version also contains `-preview` next to the date.  This means the REST API being called is a preview version, i.e. not GA.  Just like before change the version on the URL and select `Send`
12.	You’ll see that there are supported versions of the REST API beyond the preview version this call is using.  Although beyond the scope of this workshop, it’s important you understand REST API versions as it will impact your automation code

A couple more steps are needed to understand the REST API calls.  Next, you’ll look at the providers as they’re important to understand what permissions your App Registration needs.
1.	Again select the `GET All Policy Definitions` REST API call and inspect the URL

        https://management.azure.com/providers/Microsoft.Authorization/policyDefinitions?$select=displayName&api-version={{AuthorizationAPIVersion}}

2.	The ‘Microsoft.Authorization’ provider holds the permissions needed to make the call to query policy definitions (in this example)
3.	Earlier you gave the App Registration the ‘Resource Policy Contributor’ role to the subscription you’re using.  In this role, it supports the RBAC Actions:
    - Microsoft.Authorization/policysetdefinitions
    - Microsoft.Authorization/policydefinitions
    - Microsoft.Authorization/policyassignments
    
    Therefore, your App Registration is authorized to make these REST API calls; however, this isn’t complete
4.	From your REST APIs, select `PUT Trigger Policy Evaluation` and inspect its URL

        https://management.azure.com/subscriptions/{{SubscriptionID}}/providers/Microsoft.PolicyInsights/policyStates/latest/triggerEvaluation?api-version=2018-07-01-preview 

    Instead of Microsoft.Authorize, you see Microsoft.PolicyInsights as the provider.  Just because you’ve assigned Azure Policy permissions to one provider, doesn’t mean you get permissions to another even though it seems like the REST APIs are all doing the same thing.

    Fortunately, the role ‘Resource Policy Contributor’ also supports the Action: 
    - Microsoft.PolicyInsights/*
    
    Therefore your App Registration is authorized to make these REST API calls
5.	The lesson learned is you need to know the provider the REST API call lives in so you can assign the right roles to your App Registration

Wrapping up the understanding of the REST API calls, next you’ll look at the management endpoint.
1.	Again select the `GET All Policy Definitions` REST API call and inspect the URL

        https://management.azure.com/providers/Microsoft.Authorization/policyDefinitions?$select=displayName&api-version={{AuthorizationAPIVersion}} 

2.	The start of the URL contains ‘management.azure.com’ and this is the host that exposes the provider Mirosoft.Authorization (next on the URL) for its supported REST APIs
3.	In Postman, select the `Code` icon on the far-right side, and you’ll see the `host:` is ‘management.azure.com’.  For comparison, if you are calling the Azure Map APIs, the host is ‘atlas.microsoft.com’.  
4.	As previously seen in the Pre-request Scripts of the Postman Collection, the access token comes from the authorization URL.  We must use this URL in order to get an access token for the host 'management.auzre.com'

        https://login.microsoftonline.com/${tenantName}/oauth2/token

### Run the Azure Policy REST APIs
What about the actual REST API calls and what they are used for?  This is a good time to run the various calls and inspect their output, and it’s suggested you first read up on the [Azure Policy REST APIs](https://docs.microsoft.com/en-us/rest/api/resources/policydefinitions).  Pay attention to the left-hand navigation as you’ll see these sections:
- Policy Assignments
- Policy Definitions
- Policy Set Definitions

1.	Select the `Policy Definitions` section, and then select `List`
2.	Read through the page noting the `Security` section where you’ll see the authorization URL discussed earlier, the OAuth2 and implicit flow, and examples
3.	Select `Try It` towards the top of the page, and login using the credentials you’re using to access the Azure Portal
4.	If your `Subscription` isn’t the one listed, use the dropdown to select the right one
5.	Select `Run`
6.	Inspect the returned values and compare to the `GET All Policy Definitions` REST API call in your Postman Collection.  They’re the same
7.	This web-based way of testing the REST APIs is both helpful and the page for each REST API tells what you need to authorize the call
8.	Go back to the Postman Collection and run each REST API and then look it up on the documentation link from above
9.	Try adding other REST API calls to your Postman Collection, but remember you need to set the Authorization information along with its Content-Type (and perhaps other settings)
10.	Remember the providers from before, e.g. Microsoft.Authorize?  There’s a reason you cannot find the REST API call for the `PUT Trigger Policy Evaluation` in the link provided above.  Why?  What provider does it live in?
11.	The REST APIs for Microsoft.PolicyInsights are located in [Azure Policy Insights REST APIs](https://docs.microsoft.com/en-us/rest/api/policy-insights/).  There’s a reason we spent time going through these things, and now you understand why they’re important!
12. The [Azure Policy definition structure](https://docs.microsoft.com/en-us/azure/governance/policy/concepts/definition-structure) is also helpful to understand and you should take time to review its details; this will be useful as you move into Lab 2 and visualize the Azure Policy using Power BI


**Congratulations!**   You have taken the first big, important step in your Azure Policy journey.  In [Lab 2](../Lab2/README.MD), you will use Power BI to visualize Azure Policy.


# Links
- [Postman](https://www.getpostman.com/)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/)
- [Azure Policy REST APIs](https://docs.microsoft.com/en-us/rest/api/resources/policydefinitions)
- [Azure Policy Insights REST APIs](https://docs.microsoft.com/en-us/rest/api/policy-insights/)
- [Azure Policy Definition Structure](https://docs.microsoft.com/en-us/azure/governance/policy/concepts/definition-structure) 
- [Understanding OID Connect and OAuth flows](https://docs.microsoft.com/en-us/azure/active-directory/develop/v1-protocols-openid-connect-code)


# Contributing
This project welcomes contributions and suggestions. Most contributions require you to agree to a Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact opencode@microsoft.com with any additional questions or comments.


