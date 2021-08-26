# Lab 3 – Customizing Azure Policy

WELCOME! Here are some tips-and-tricks you can use while in the workshop
1.	Open a Notepad text document and save it. This is where you can copy-and-paste variables, connection string, credentials vs. having to refer to another web page for those values
2.	Open a browser session to have as your Azure portal and Azure Cloud Shell
3.	Have a different browser session open to the GitHub repo with all the lab instructions

## Lab Goals

Create a custom Azure policy and assign it to a policy set.

### Learning Objectives

- Understand the structure of an Azure policy
- Create a custom Azure Policy based on an existing built-in policy
- Publish a custom Azure Policy and assign it to a policy set

## Requirements

- An Azure subscription for which you have the Owner role
- A PC which you can install applications on

### Clone this GitHub Repository

If you haven't cloned the repository already, please follow these instructions.

There are several files you will use from this repository, so to ease access you will clone the repository to your local machine.  In this workshop, you will not be creating your own GitHub repository from the workshop cloned repository; you can do that work later if desired.
1. Go to https://aka.ms/mtcazurepolicysamples
2. Select `Clone or download` from the left side
3. You can either download the .ZIP file and extract locally or you can copy the link or clone the repository on the command line.  If you are using GitHub Desktop, you can clone it by selecting `Open in Desktop`
4. To clone using the command line, open a CMD shell and change directory to where you want the repository cloned

`git clone <paste in the copied repository link from the above step>`

### Configure Visual Studio Code for Azure Policy

In a later lab, you'll have the option of writing Python code to interact with Azure Policy; however, there are extensions to Visual Studio Code that can interact with Azure Policy that can help in your policy governance.  You'll install the extensions for Azure Account, Azure Policy, and Azure Resource Manager.  Azure Account is a commong sign-in and subscription management extension, Azure Policy allows you to search for resources, resource properties, and view policies and their assignments, and Aure Resource Manager extension allows you to easily create and edit Azure ARM templates.  There is also a web-based Azure Resource Explorer that of this writing is in Preview; you'll use this explorer to understand the breadth of tools available to you.
1. Open Visual Studio Code and either go into the Extensions tab from the left-pane menu or by entering Ctrl-Shift-X
2. `Search` for 'Azure Account' and install it
    - While the extension installs, review the Extensions readme to understand the available commands
    - Note the ability to sign-in/out as a user or a device and select an active subscirption
    - Also note the ability to launch, or even upload a file, to an Azure Cloud Shell
    - Either open the command pallette or enter Ctrl-Shift-P
    - Search for 'Azure: sign in' and select it
    - A browser page will open asking for your Azure credentials, enter them and close the browser when it confirms you're authenticated
    - On the bottom-left of the status bar, select where it says 'Azure:'
    - Notice the command pallette highlights available subscriptions, select the appropriate subscription for this lab
    - Given you've authenticated to Azure, your credentials are now available as you manage Azure services in Visual Studio Code
3. In the Extensions tab, `search` for 'Azure Policy' and install it
    - While the extension installs, review the Extensions readme to understand the available commands
    - Note this extension is for read access and doesn't directly allow you to create or update Azure Policies; however in some cases, the contents displayed can be used in other Azure interfaces to create or modify resources
    - Pay attention to the `Settings` under `Features`.  Open these settings by selecting the gear icon in the lower-left corner, select `Settings`, select `Extensions`, and then select `Azure Policy`.  See there are both `User` settings and `Workspace` settings available
    - Recommend you keep the first two checkboxes selected - this will limit the amount of time it takes to populate resources and policies.  Close the `Settings` tab
    - Review [Use Azure Policy extension for Visual Studio Code](https://docs.microsoft.com/en-us/azure/governance/policy/how-to/extension-for-vscode)
4. In the Extensions tab, `search` for 'Azure Resource Manager (ARM) Tools' and install it
    - While the extension installs, review the Extensions readme to understand the available commands
    - This extension doesn't provide direct Azure create or update abilities for ARM templates; however, with its IntelliSense and ARM snippets, it accelerates editing ARM templates
    - In your `Lab3` folder, create a new file named 'myarm.json'
    - In your new JSON file, enter 'arm!' and select the `skeleton ARM template` option.  Notice it scaffolded an ARM template for you
    - Move your cursor below the ARM templates scaffold and enter 'armp!' and select the `skeleton ARM Parameters template` option.  Notice it scaffolded an ARM parameters template for you

Creating a custom Azure Policy requires a properly written JSON representation of the policy and its assignment.  As you read about the Azure Policy extension, it covered scoping to policies that have aliases. When you installed the Azure Policy extension, it had a setting to limit views to only resource providers and resource types that had policy aliases.  This made the queries efficient by limiting its scope.  Understanding policy aliases is important

### Understand the structure of an Azure policy

### Create a custom Azure Policy based on an existing built-in policy

### Publish a custom Azure Policy and assign it to a policy set


**Congratulations!**  You've made remarkable progress in understanding Azure Policy and its enterprise governance!


# Links
- [Postman](https://www.getpostman.com/)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/)
- [Azure Policy REST APIs](https://docs.microsoft.com/en-us/rest/api/resources/policydefinitions)
- [Azure Policy Insights REST APIs](https://docs.microsoft.com/en-us/rest/api/policy-insights/)
- [Understanding OID Connect and OAuth flows](https://docs.microsoft.com/en-us/azure/active-directory/develop/v1-protocols-openid-connect-code)
- [Azure Policy extension for Visual Studio Code](https://docs.microsoft.com/en-us/azure/governance/policy/how-to/extension-for-vscode)
- [Azure Policy definition structure](https://docs.microsoft.com/en-us/azure/governance/policy/concepts/definition-structure)
- [Tutorial: Create a custom policy definition](https://docs.microsoft.com/en-us/azure/governance/policy/tutorials/create-custom-policy-definition)


This sample doesn't cover the full breadth of Azure Policy automation, and there is often more than one way to complete a given task.  The information below partially expands on its breadth:
- [Create and assign a policy definition using ARMClient](https://docs.microsoft.com/en-us/azure/governance/policy/how-to/programmatically-create#create-and-assign-a-policy-definition-using-armclient)
- [Remediate non-compliant resources with Azure Policy](https://docs.microsoft.com/en-us/azure/governance/policy/how-to/remediate-resources)
- [Create a policy assignment to identify non-compliant resources by using a Resource Manager template](https://docs.microsoft.com/en-us/azure/governance/policy/assign-policy-template)
- [Starter Resource Graph queries](https://docs.microsoft.com/en-us/azure/governance/resource-graph/samples/starter)


# Contributing
This project welcomes contributions and suggestions. Most contributions require you to agree to a Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact opencode@microsoft.com with any additional questions or comments.


