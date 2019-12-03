# Lab 2 – Visual Azure Policy with Power BI
WELCOME! Here are some tips-and-tricks you can use while in the workshop
1.	Open a Notepad text document and save it. This is where you can copy-and-paste variables, connection string, credentials vs. having to refer to another web page for those values
2.	Open a browser session to have as your Azure portal and Azure Cloud Shell
3.	Have a different browser session open to the GitHub repo with all the lab instructions

## Lab Goals
Visual Azure Policy and Azure RBAC to better understand their relationships.

### Learning Objectives
- Use Power BI to visualize the Azure Policy and Azure RBAC REST APIs
- Understand how Azure Policy and Azure RBAC are related
- Visualize the CIS baseline and trace it to its Policy and RBAC

## Requirements
- An Azure subscription for which you have the Owner role
- A PC which you can install applications on

### Clone this GitHub Repository
If you haven't cloned the repository already, please follow these instructions.

There are several files you will use from this repository, so to ease access you will clone the repository to your local machine.  In this workshop, you will not be creating your own GitHub repository from the workshop cloned repository; you can do that work later if desired.
1.	Go to https://aka.ms/mtcazurepolicysamples
2.	Select `Clone or download` from the left side
3.	You can either download the .ZIP file and extract locally or you can copy the link or clone the repository on the command line.  If you are using GitHub Desktop, you can clone it by selecting `Open in Desktop`
4.	To clone using the command line, open a CMD shell and change directory to where you want the repository cloned

`git clone <paste in the copied repository link from the above step>`

### Install Power BI Desktop
Power BI Desktop is a client tool used to visualize data from many sources.  The Power BI service allows visualizations to be shared within an organization.  In this case, you'll only be using the desktop client.
1. Install [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).  You will be redirected to the Microsoft Store; accept the redirect if prompted.  Once installed, you can close the Microsoft Store application
2. Start Power BI and you can sign-in with your company credentials or you can bypass it.  If you don't have company credentials, select `Already have a Power BI account?  Sign in`  At the next dialogue, simply close it using the `X`
- You won't be publishing any Power BI visualizations to the Power BI service; rather, all work will be local
3. From Power BI, select `Ctrl-O` to open a .PBIX file.  Open the folder where you cloned this repo and then go into the `PowerBI` folder
4. Select the file 'Azure Policy Visualization' and then select `Open`

### Review and Update the Power BI Data/Queries
You will see a visualization that shows Azure Policy with options to select/filter on certain values. You'll dig into this more later in this lab, but for now you will look at the source of the data and how it was shaped to enable creating this and other visualizations.

1. From Power BI, select `Edit Queries`

`Notice the DataSource Errors.  Even though the visualizations looked right, the underlying data source for the queries need to be resolved.  This is where the work from Lab 1 will come in handy - we need to query the REST APIs to create the JSON files for these data sets.`

2. Leaving Power BI open, also open Postman and open your workspace/collection you created in Lab 1
3. You need to create and test the query to return all Azure Policy Definitions and all Azure Policy Set Definitions.  Select `GET All Policy Definitions` and then select `Send`
4. On the right side, select the `Save Response` drop down and then select `Save to a file`
5. Open the `Data` folder in the cloned repo, and save the file with the name 'AllPolicyDefinitions.JSON'
6. Repeat the same for `Get All Policy Set Defintions` and save the file with the name 'AllPolicySetDefinitions.JSON'
7. You have now saved the raw data needed for the Power BI queries, so go back to Power BI
8. From the Powery Query Editor window you opened previously, select `Data source settings` from the top menu
9. Select the PolicySet data source and then select `Change Source...`.  Select `Browse...` and open the Data folder where you just saved these results - select the file named 'AllPolicySetDefinitions.JSON'.  Select `OK`
10. Now select the Policy data source and then select `Change Source...`.  Select `Browse...` and open the Data folder where you just saved these results, and select the file named 'AllPolicyDefinitions.JSON'.  Select `OK`
11. Select Close
12. On the left side, you will see the two data sources: AllPolicyDefintions and AllPolicySetDefinitions.  Select `AllPolicyDefintions`
13. On the right side, select `Source` from the section on `Applied Steps` and see in the top that it's pointing the JSON document you just set
14. It's beyond the scope of this workshop to discuss how the queries were edited to get into a meaningful form so the data could be visualized.  Take time to go through each step in `Applied Steps` (top-down) and see what happened to the resulting data.  You can do the same for the AllPolicySetDefintions data source
15. Once you're done reviewing the queries, select `Close & Apply` from the top right and you'll be returned to the Power BI visualizations

`If you're asking why so many manual steps are needed, you're asking a good question.  In the labs so far, you are using client tools to create queries, save data, adjust Power BI queries, etc.  This can all be automated, but it's important you understand how the pieceparts fit together at the basic level.  At a later date, automation samples will be included in this repo.`

### Visualize Azure Policy
1. From Power BI, on the right side you will see 'AllPolicyDefintions' and 'AllPolicySetDefintions'.  In the tabs below the visualization, you'll see three tabs named: Instructions, All Policies, and All Policy Sets
2. Select the `All Policies` tab below, and then on the right expand `AllPolicyDefintions`
3. Azure Policy has an assigned category, as you see on the visualization. On the right, you'll see an option to select policies that are in Preview.  Select `False`
4. From the `Policy Definitions Categories`, select only `App Service`
5. Notice the `Policy Definitions` list changed to reflect all policies in the category `App Service`, and the `Policy Definition Details` list provides details about each policy defintion
6. From the `Policy Definitions` list, select only `API App should only be accessible over HTTPS`.  Notice the `Policy Defintion Details` list has changed to show only the policy defintion `API App should only be accessible over HTTPS`
7. From the `Policy Definitions` list, select again the line `API App should only be accessible over HTTPS` which will reset the scope to all policy definitions
8. In `Policy Defintion Categories`, select `Select all`, and in `In Preview`, select `True` to list only policy defintions that are in preview
9. Notice that the number of items in the `Policy Defintion Categories` list dropped because you filtered out to only see policy definitions that are in preview
10. Take time to look through thee policy sets and then look up more details online as appropriate.  You can reference your Postman collection to see the actual JSON details of any policy set


`Understanding the base defintions of Azure Policy is important, and it will be equally important to understand how they are reflected in mandatory settings like CIS compliance.  All these policies defintions are just  pieceparts that can be assembled into a compliance policy like CIS (which is already built for you in Azure) or even a custom set of policies like your enterprise security baseline.`

### Visualize Azure Policy Sets

Azure policy sets are also known as policy initiatives when viewed in the Azure portal.  A policy set is a logical group of Azure policies targeted towards a common objective, e.g. VM security Policies.  In this next activity, you will review policy set defintions and see how they're tied to the policy definitions.

1. From Power BI, select the `All Policy Sets` tab below, and then on the right expand `AllPolicySetDefintions`.  You can collapse `AllPolicyDefinitions` if it's still expanded
2. Azure policy sets have an assigned category, as you see on the visualization, but the category doesn't align one-to-one with Azure policy definition catagories
3. From the `Policy Set Categories`, select only `Guest Configuration`
4. Notice the `Policy Sets` list changed to reflect all policies in the category `Guest Configuration`, and the `Policy Set Details` list provides details about each policy defintion
5. The `Policy Defintions` list also changed to reflect only those policy defitions in the category `Guest Configuration`
6. From the `Policy Sets` list, select only `[Preview]: Audit VMs with insecure password security settings`.  Notice the `Policy Set Details` now only shows the single line for this policy set defintion, and the `Policy Defintions` list has changed to show only policy defintions that part of the policy set `[Preview]: Audit VMs with insecure password security settings`
7. From the `Policy Sets` list, select again the line `[Preview]: Audit VMs with insecure password security settings` which will reset the scope to all policy sets
8. Scroll down in the `Policy Sets` list, and select the line `Audit Windows VMs on which the specified services are not installed and 'Running'`
9. Review the `Policy Set Details` list specifically looking at the description.  It notes that 'specified services' are not running

`What services aren't running?  This is part of planning Azure Policy for guest configuration; you need to describe the required services as part of this policy.  Also note Audit and Deploy in the 'Policy Defintions' list.  The policies for this policy set support both auditing guest OS that isn't configured with the required services, and they support deploying any required services not installed on the guest OS.  In the description, you'll find the link https://aka.ms/gcpol which contains details on Azure Policy for Guest Configuration.  Take time now to read these details to better understand the controls.`

10. Take time to look through the policy sets and then look up more details online as appropriate.  You can reference your Postman collection to see the actual JSON details of any policy set

`Although it's beyond the scope of this workshop, it's worth mentioning the inclusion of 'preview' policy sets and the fact there was no Power BI filter to select only preview items like there was in the policy definitions.  This is because the JSON that describes policy sets doesn't contain a data element for preview, and the only way you know is seeing the description where it indicates '[Preview]'.  You can refer to your Postman collection and run the 'GET All Policy Set Defintions' and then search for 'preview' in the returned JSON body.`

`As a stretch goal, you can resolve this.  If you edit your Power BI query for the policy set defintions, you can create a new column from an example which means you can extract the term 'preview' from a description and add it as a new column.  You could then change the value 'preview' to 'True' and any other value to be False.  Change the name and change the column type to 'boolean' and close & save the query.  You will now have a preview data elements in the policy set defintion!  Again, this is a stretch goal for you to discover on your own, but knowing these Power BI skills will help you in your Azure governance journey.`



**Congratulations!**  Understanding the relationships between Azure Policy and Azure RBAC will help you in your automation journey!


# Links
- [Postman](https://www.getpostman.com/)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/)
- [Azure Policy REST APIs](https://docs.microsoft.com/en-us/rest/api/resources/policydefinitions)
- [Azure Policy Insights REST APIs](https://docs.microsoft.com/en-us/rest/api/policy-insights/)
- [Understanding OID Connect and OAuth flows](https://docs.microsoft.com/en-us/azure/active-directory/develop/v1-protocols-openid-connect-code)


# Contributing
This project welcomes contributions and suggestions. Most contributions require you to agree to a Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact opencode@microsoft.com with any additional questions or comments.


