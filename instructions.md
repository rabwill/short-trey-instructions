@lab.title

## Disclaimer
Please be advised that the live lab environment may be impacted by ongoing product/feature updates. This may result in discrepancies between lab instructions and the interface.

## Credentials
>[!Alert] This lab provides you with all resources, licenses, and credentials needed to complete the lab. 
>
> It is important that you complete all lab steps in the lab environment. Do not use personal or work/demo environments to complete the lab steps.

## Before you start

> [!TIP]
> As you follow the instructions in this pane, whenever you see a +++icon+++, you can use it to copy text from the instruction pane into the virtual machine interface. This is particularly useful to copy code; but bear in mind you may need to modify the pasted code to fix indent levels or formatting before running it!


## Sign into Windows

- [ ] In the virtual machine, sign into Windows using the following Windows credentials given on the "Resources" panel on your right where this instruction is,  specifically for **Win11-Pro-Base** :

- **Username**: +++LabUser+++
- **Password**: +++Pa$$w0rd+++

The same "Resources" panel also provides you with the credentials to login to Microsoft 365 tenant for this lab.

## Access Visual Studio Code

- [ ] Once signed into the machine, you will be able to access VS Code from the desktop. Open VS Code.

>[!TIP]
> When you open the folder in VS Code, you may get a prompt window asking if you trust the authors of the files in the folder. This is expected and you can safely select **Yes, I trust the authors**. The dialog is a security safeguard that helps you decide whether to run all features or limit execution based on the trustworthiness of the code authors. If you're opening your own code or from a reliable source, it's safe to trust.


===


@lab.title 

## Learning Objectives 

During this lab, you will construct a customized assistant tailored for a Human Resources department. The process will begin with understanding the fundamentals on creating a declarative agent, building custom APIs, the creation of a basic declarative agent, and progress towards developing fully skilled assistant that uses the APIs. 

The sections in this lab are:

- What are Declarative Agents?
- Pre-lab Confidence & Knowledge Survey
- Exercise 1:  Build a Backend API
- Exercise 2:  Build a declarative agent grounded with API you created as well as SharePoint files
- Post-lab Confidence & Knowledge Survey
- Wrap up


> [!knowledge] 
> All of these labs (and more) are available on [Copilot Developer Camp](https://aka.ms/copilotdevcamp). Subscribe to the [Microsoft365Developer YouTube channel](https://www.youtube.com/@Microsoft365Developer). 

**Note** There are "Pre-lab Confidence & Knowledge Survey" and  "Post-lab Confidence & Knowledge Survey". - [ ] Make sure you fill in and select "Submit your responses".

 !IMAGE[survey.jpg](instructions303310/survey.jpg)

=== 

# What are Declarative Agents? 

**Declarative Agents** leverage the same scalable infrastructure and platform as Microsoft 365 Copilot, tailored specifically to focus on a particular area of your needs. They function as subject matter experts in a specific domain or business need, allowing you to use the same interface as standard Microsoft 365 Copilot chat while ensuring they focus exclusively on the specific task at hand.

## Anatomy of a Declarative Agent

As you build more agents for Copilot, you'll notice that the final output is a set of files bundled into a zip file called an **app package** that you'll install and use. It's important to have a basic understanding of what the app package contains. The app package of a Declarative Agent is similar to a Teams app (if you've built one before) with additional elements. See the table below for all the core elements. You'll also notice that the app deployment process is very similar to deploying a Teams app.

| File Type                     | Description                                                                                                                                                     | Required |
|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| App manifest                  | A JSON file (**manifest.json**) that defines the standard Teams app manifest.                                                                                 | Yes      |
| Declarative agents manifest   | A JSON file containing the agent's name, instructions, capabilities, conversation starters, and actions (if applicable).                                      | Yes      |
| Plugin manifest               | A JSON file used to configure your action as an API plugin. Includes authentication, required fields, adaptive card responses, etc. Only needed if actions exist. | No       |
| App icons                     | A color and outline icon for your declarative agent.                                                                                                          | Yes      |

## Capabilities of a Declarative Agent

You can enhance the agent's focus on context and data by not only adding instructions but also specifying the knowledge base it should access. These are called **capabilities**. Below are the ones supported in a Declarative Agent today: 

- **Copilot Connectors** - let you centralize content on Microsoft 365. By importing external content to Microsoft 365, you not only make it easier to find relevant information, but you also let others in your organization discover new content.
- **OneDrive and SharePoint** - let you provide URLs of files/sites in OneDrive and SharePoint, which will become part of the agent's knowledge base.
- **Web search** - let you enable or disable web content as part of the agent's knowledge base. You can also pass around 4 websites URLs as source. 
- **Code interpreter** - enables you to build an agent with capabilities to better solve math problems and, when needed, leverage Python code for complex data analysis or chart generation.
- **GraphicArt** - enables you to build an agent for image or video generation using DALL·E.
- **Email knowledge** - enables you to build an agent to access a personal or shared mailbox, and optionally, a specific mailbox folder as knowledge.
- **People knowledge** - enables you to build an agent to answer questions about individuals in an organization.
- **Teams messages** - enables you to equip the agent to search through Teams channels, teams, meetings, 1:1 chats, and group chats.
- **Dataverse knowledge** - enables you to add a Dataverse instance as a knowledge source.
- **Scenario models** - enables you to add task-specific models.
- **Teams Meetings** - enables you to build an agent to search for information about meetings in the organization.

In this lab, we will add an action as an **API plugin** to our agent along with the **OneDrive and SharePoint** capability.

☑️  Well done, you've built a solid foundation in the theory of Declarative Agents. - Select **Next >** to go to the next page.

=== 

# Pre-lab Confidence & Knowledge Survey
@lab.ActivityGroup(initialsurvey)

===
# Exercise 1: Build a Backend API for the Solution

In this exercise you will set up REST API based on Azure Functions and test the API. 

## Introduction
You will set up a REST API for Trey Research, a hypothetical consulting company. It provides API's for accessing information about consultants (using the `/api/consultants` path) and about the current user (using the `/api/me` path). In this exercise the API doesn't support authentication, so the current user will always be "Avery Howard".

The code consists of Azure Functions written in TypeScript, backed by a database in Azure Table storage. When you run the app locally, table storage will be provided by the Azurite storage emulator.

> [!hint]  **How did you create this API?** The project was created using Microsoft 365 Agents Toolkit. You can create the same scaffolding for your own project by opening an empty folder in VS Code and going to Microsoft 365 Agents Toolkit. Create a new app project and select "New Agent", then "Declarative Agent" and "Add plugin". But you don't have to do it as we will download and run a starting application. 

## Step 1: Configure and run the starting application
### Download the starting application
Begin by downloading the source code zip file into the lab environment by following these steps:

1. - [ ] **Copy this download link**: Right-click on [this link](https://download-directory.github.io/?url=https://github.com/microsoft/copilot-camp/tree/main/src/extend-m365-copilot/trey-research-short-lab-start&filename=build-api-project) and select **Copy Link**

2. - [ ] **Download the project files**:
   - [ ] Open the Edge browser in the lab environment
   - [ ] Paste the copied link into the address bar and press Enter
   - [ ] The browser will automatically download a zip file named `build-api-project.zip`

3. - [ ] **Extract and open the project folder**:
   - [ ] Navigate to your Downloads folder (usually found in File Explorer)
   - [ ] Locate the downloaded `build-api-project.zip` file
   - [ ] Right-click on the zip file and select **Extract All** 

This extracted folder is where you will be working for this entire lab. These instructions will refer to this folder as the "working folder" going forward. The next several exercises build on this one, and you should be able to continue working in the same folder.
Close the browser and the file explorer.

### Set up the local environment files
1. - [ ] If not already open, open **Visual Studio Code** from the **Desktop**
2. - [ ] Open your working folder (the extracted **build-api-project** folder) in Visual Studio Code
3. - [ ] You might see a popup dialog asking you to "trust the authors of the files in this folder". If so, select **"Yes, I trust the authors"** to proceed
4. - [ ] Go to the **"Explorer"** view to see all files and folders in the working folder 
5. - [ ] Expand the **env** folder
6. - [ ] Copy and paste the `.env.local.user.sample` file and rename the copy to `.env.local.user`
7. - [ ] Ensure this line is present in the `.env.local.user` file:

```
SECRET_STORAGE_ACCOUNT_CONNECTION_STRING=UseDevelopmentStorage=true
```
### Install the dependencies
1. - [ ] Open a command line in your working folder by selecting **Terminal** > **New Terminal** in Visual Studio Code, then type and press Enter on the following command to install the required packages:

```
npm install
```

### Sign in to Microsoft 365 Agents Toolkit
You'll need to sign into the Microsoft 365 Agents Toolkit in order to upload and test your application from within it. 

1. - [ ] Within the project window, select the **Microsoft 365 Agents Toolkit** icon <img width="24" alt="m365atk-icon" src="https://github.com/user-attachments/assets/b5a5a093-2344-4276-b7e7-82553ee73199" /> from the left side menu. This will open the Agent Toolkit's activity bar with sections like Accounts, Environment, Development, etc.
2. - [ ] Under the **"Accounts"** section, select **Sign in to Microsoft 365**. This will open a dialog with options to sign in, create a Microsoft 365 developer sandbox, or Cancel. Select **Sign in**
3. - [ ] Use the credentials available to you from the **"Resources"** tab in Skillable:

Username: +++@lab.CloudPortalCredential(User1).Username+++

TAP Token:+++@lab.CloudPortalCredential(User1).AccessToken+++

4. - [ ] Once signed in, close the browser and return to the project window

### Run the application

Previously, you ensured you are logged into Microsoft 365 for the toolkit. If everything looks good, you will have both **Custom App Upload Enabled** and **Copilot Access Enabled** indicators showing green checkmarks.

!IMAGE[Visual Studio Code with the Agents Toolkit enabled and the accounts section with green checkmarks.](instructions315038/atk-accounts.png)

1. - [ ] Next, start debugging by using one of these methods:
 -  Press **F5** to debug using Microsoft Edge
 -  In the VS Code menu, select **Run > Start Debugging**
 -  Hover over the "local" environment and click the debugger symbol that appears 1️⃣, then select the browser of your choice 2️⃣


!IMAGE[Visual Studio Code with the Agents Toolkit enabled, the debug mode active for local environment, and the option to start debugging in the Microsoft Edge browser.](instructions315038/f5.png)


2. - [ ] Eventually a browser will open (it's faster after the first time) and ask to log in but you can skip logging in for now as we are only testing the API, but make sure you keep the browser minimized not closed.

> [!hint] If you get a Windows Security alert asking "Do you want to allow public and private networks to access this app?" just select **Allow**.  

## Step 2: Test the API

In this step you'll test the API manually and, in the process, learn about what it does.

### GET the /me resource
- [ ] With the debugger still running 1️⃣, switch to the code view in Visual Studio Code 2️⃣. Open the **http** folder and select the **treyResearchAPI.http** file 3️⃣.

- [ ] Now click the **"Send Request"** link in the treyResearchAPI.http file just above the link `{{base_url}}/me` 6️⃣.
!IMAGE[run-in-ttk04.png](instructions303310/run-in-ttk04.png)

You should see the response in the right panel, and a log of the request in the bottom panel. The response shows the information about the logged-in user, but since we haven't implemented authentication this time, the app will return information on the fictitious consultant "Avery Howard". Take a moment to scroll through the response to see details about Avery, including a list of project assignments.
!IMAGE[run-in-ttk05.png](instructions303310/run-in-ttk05.png)

### Try the other methods and resources
- [ ] Now try sending the POST request for `{{base_url}}/me/chargeTime`. This will charge 3 hours of Avery's time to the Woodgrove Bank project. This is stored in the project database, which is a locally hosted emulation of Azure Table Storage, so the system will remember that Avery has delivered these hours. (To test this, call the `/me` resource again and look at the "deliveredThisMonth" property under the Woodgrove project).

- [ ] Continue to try the various GET requests in the **.http** file to find consultants with various skills, certifications, roles, and availability. All this information will be available to Copilot so it can answer user prompts.
- [ ] Once done testing, stop the debugger by going to the VS Code menu **Run > Stop Debugging**. Also close all windows inside VS Code like .http file 1️⃣ as well as the Response view 2️⃣.

!IMAGE[close all windows](instructions315038/iyp3uzg9.jpg)


### Examine the database (optional)
- [ ] You can examine and modify the application's data. The data is stored in Azure Table Storage, which in this case is running locally using the Azurite emulator which is already installed for you in this environment.

> [!note] When you ran npm install in the previous exercise you installed the Azurite storage emulator. When you start the project, Azurite is automatically started up. So as long as your project is started successfully you can view the storage.

Within the Azure Storage Explorer, open the "Emulator & Attached" selection and pick the "(Emulator: Default Ports)" collection; then drill down to "Tables". You should see 3 tables:

**Consultant**: This table stores details about Trey Research consultants

**Project**: This table stores details about Trey Research projects

**Assignment**: This table stores consultant assignments to projects, such as Avery Howard's assignment to the Woodgrove Bank project. This table includes a "delivered" field that contains a JSON representation of the hours delivered by that consultant on the project over time.

!IMAGE[azure-storage-explorer01.png](instructions303310/azure-storage-explorer01.png)

☑️  You've successfully built the backend API! You can now proceed to make it into a  plugin, and expose it via a Declarative agent in the next exercise.

===


# Exercise 2: Build a Declarative Agent Grounded with API and SharePoint Files

In this exercise, you will build an API plugin using the API you created in the previous exercise. Next, you'll integrate a declarative agent that is grounded in both the API plugin and specific SharePoint files.

## Step 1: Upload Sample Documents
In this step you will upload sample documents which will be used by your declarative agent to respond to user prompts. These include some consulting documents such as Statements of Work, and a simple spreadsheet containing your hours as a consultant.

### Create a SharePoint site
- [ ] Go to the browser and open the link `https://m365.cloud.microsoft.com/apps/` and find the **"SharePoint"** app under **"Apps"** from the browser inside your Skillable environment.

!IMAGE[upload-docs-01.png](instructions303310/upload-docs-01.png)
1. - [ ] Click **"Create site"** 1️⃣ and choose **"Team site"** 2️⃣

!IMAGE[upload-docs-02.png](instructions303310/upload-docs-02.png)

2. - [ ] Select the **"Standard team"** site template; you will be shown a preview of the site. Click **"Use Template"** to continue
3. - [ ] Give your site a name  1️⃣, but keep it unique like **Trey Research Legal - (Your Initials)(Favorite 2 digit Number)**

How to create this:

- Use your first and last name initials (example: Sarah Johnson = SJ)
- Add your favorite 2 digit number right after (no spaces)
- Complete format: "Trey Research Legal - (Initials)(Number)"

**Examples:**

✅ "Trey Research Legal - SJ7" (Sarah Johnson, favorite number 7)

✅ "Trey Research Legal - MK42" (Mike Kim, favorite number 42)  

✅ "Trey Research Legal - AL99" (Anna Lopez, favorite number 99)

- [ ] Once done select **"Next"** 2️⃣

!IMAGE[upload-docs-05.png](instructions303310/upload-docs-05.png)
- [ ] Then select your privacy settings and language, and click "Create site"
- [ ] After a few moments you will be asked to complete by selecting **"Finish"**. Then you will be presented with a new SharePoint site.

### Upload the Sample Documents
1. - [ ] In the Documents web part, select **"See all"** to view the document library page

!IMAGE[upload-docs-07.png](instructions303310/upload-docs-07.png)

2. - [ ] Click the **"Upload"** 1️⃣ toolbar button and select **"Files"** 2️⃣

!IMAGE[upload-docs-08.png](instructions303310/upload-docs-08.png)

3. - [ ] Navigate to your working folder; you will find a directory called **sampleDocs** within it. Highlight all the sample documents 1️⃣ and click **"Open"** 2️⃣

4. - [ ] Make note of the site url, which will resemble "https://{{tenant}}/sites/TreyResearchLegal-RW33", as you will need it in the next step.

!IMAGE[upload-docs-09.png](instructions303310/upload-docs-09.png)

## Step 2: Create the Declarative Agent

### Add the Declarative Agent JSON to Your Project

1. - [ ] Close the browser and return to the working folder in VS Code
2. - [ ] Create a new file called `trey-declarative-agent.json` within your **appPackage** folder
3. - [ ] Copy the following JSON into this file and save it:
```
{
    "$schema": "https://developer.microsoft.com/json-schemas/copilot/declarative-agent/v1.6/schema.json",
    "version": "v1.6",
    "name": "Trey Genie Local",
    "description": "You are a handy assistant for consultants at Trey Research, a boutique consultancy specializing in software development and clinical trials. ",
    "instructions": "You are consulting agent. Greet users professionally and introduce yourself as the Trey Genie. Offer assistance with their consulting projects and hours. Remind users of the Trey motto, 'Always be Billing!'. Your primary role is to support consultants by helping them manage their projects and hours. Using the TreyResearch action, you can: You can assist users in retrieving consultant profiles or project details for administrative purposes but do not participate in decisions related to hiring, performance evaluation, or assignments. You can assist users to find consultants data based on their names, project assignments, skills, roles, and certifications. You can assist users to retrieve project details based on the project or client name. You can assist users to charge hours to a project. You can assist users to add a consultant to a project. If a user inquires about the hours they have billed, charged, or worked on a project, rephrase the request to ask about the hours they have delivered. Additionally, you may provide general consulting advice. If there is any confusion, encourage users to consult their Managing Consultant. Avoid providing legal advice.",
    "conversation_starters": [
        {
            "title": "Find consultants",
            "text": "Find consultants with TypeScript skills"
        },
        {
            "title": "My Projects",
            "text": "What projects am I assigned to?"
        },
        {
            "title": "My Hours",
            "text": "How many hours have I delivered on projects this month?"
        }
    ],
    "capabilities": [
        {
            "name": "OneDriveAndSharePoint",
            "items_by_url": [
                {
                    "url": "${{SHAREPOINT_DOCS_URL}}"
                }
            ]
        }
    ],
    "actions": [
        {
            "id": "treyresearch",
            "file": "trey-plugin.json"
        }
    ]
}
```
Notice that the file includes a name, description, and instructions for the declarative agent. Notice that as part of the instructions, Copilot is instructed to "Always remind users of the Trey motto, 'Always be Billing!'." You should see this when you prompt Copilot in the next step.
### Add the URL of Your SharePoint Site to the Declarative Agent
Under "Capabilities" you will notice a SharePoint file container. While Microsoft 365 Copilot may reference any documents in SharePoint or OneDrive, this declarative agent will only access files in the Trey Research Legal Documents site you created in earlier step.
```
"capabilities": [
    {
        "name": "OneDriveAndSharePoint",
        "items_by_url": [
            {
                    "url": "${{SHAREPOINT_DOCS_URL}}"
            }
        ]
    }
],
```
Notice that the SharePoint URL is actually an environment variable `SHAREPOINT_DOCS_URL`, so you need to add that to your **.env.local** file in the **env** folder. Add this on its own line at the end of the file, using your copied SharePoint URL:
```
SHAREPOINT_DOCS_URL=https://<mytenant>.sharepoint.com/sites/<TreyResearchLegaldocuments_NAME>
```
### Examine the API Plugin Files
Within the trey-declarative-agent.json file, you'll find an "actions" section, which tells the declarative agent to access the Trey Research API.
```
"actions": [
    {
        "id": "treyresearch",
        "file": "trey-plugin.json"
    }
]
```
In this step, we'll look at **trey-plugin.json** and how it and another file describe the API to Copilot so it can make the REST calls.

These two files are used to describe your API to Copilot. They were already included in the project you downloaded in the previous exercise, so you can examine them now:

 * [**appPackage/apiSpecificationFile/trey-definition.json**](https://github.com/microsoft/copilot-camp/blob/main/src/extend-m365-copilot/trey-research-short-lab-end/appPackage/apiSpecificationFile/trey-definition.json) - This is the [OpenAPI Specification (OAS)](https://swagger.io/specification/) or "Swagger" file, which is an industry standard format for describing a REST API
 * [**appPackage/trey-plugin.json**](https://github.com/microsoft/copilot-camp/blob/main/src/extend-m365-copilot/trey-research-short-lab-end/appPackage/trey-plugin.json)- This file contains all the Copilot-specific details that aren't described in the OAS file

 In this step, take a moment to examine these files. 

 In **appPackage/apiSpecificationFile/trey-definition.json**, you'll find the general description of the application. This includes the server URL; Agents Toolkit will create a [developer tunnel](https://learn.microsoft.com/azure/developer/dev-tunnels/) to expose your local API on the Internet, and replace the token `"${{OPENAPI_SERVER_URL}}` with the public URL. It then goes on to describe every resource path, verb, and paremeter in the API. Notice the detailed descriptions; these are important to help Copilot understand how the API is to be used.
 ```
 {
  "openapi": "3.0.1",
  "info": {
      "version": "1.0.0",
      "title": "Trey Research API",
      "description": "API to streamline consultant assignment and project management."
  },
  "servers": [
      {
          "url": "${{OPENAPI_SERVER_URL}}/api/",
          "description": "Production server"
      }
  ],
  "paths": {
      "/consultants/": {
          "get": {
              "operationId": "getConsultants",
              "summary": "Get consultants working at Trey Research based on consultant name, project name, certifications, skills, roles and hours available",
              "description": "Returns detailed information about consultants identified from filters like name of the consultant, name of project, certifications, skills, roles and hours available. Multiple filters can be used in combination to refine the list of consultants returned",
              "parameters": [
                  {
                      "name": "consultantName",
                      "in": "query",
                      "description": "Name of the consultant to retrieve",
                      "required": false,
                      "schema": {
                          "type": "string"
                      }
                  },
      ...
```
The **appPackage/trey-plugin.json** file has the Copilot-specific details. This includes breaking the API calls down into _functions_ which can be called when Copilot has a particular use case. For example, all GET requests for `/consultants` look up one or more consultants with various parameter options, and they are grouped into a function `getConsultants`:
 ```
   "functions": [
    {
      "name": "getConsultants",
      "description": "Returns detailed information about consultants identified from filters like name of the consultant, name of project, certifications, skills, roles and hours available. Multiple filters can be used in combination to refine the list of consultants returned",
      "capabilities": {
        "response_semantics": {
          "data_path": "$.results",
          "properties": {
            "title": "$.name",
            "subtitle": "$.id",
            "url": "$.consultantPhotoUrl"
          },
           "static_template": {
            "file": "adaptiveCards/getUserInformation.json"
          }
        }
      }
    },
```
Notice that responses are displayed using Adaptive Cards, which are rich, interactive cards. The card template is stored in **adaptiveCards/getUserInformation.json**. If you open this JSON file, you'll see the card's structure and how it uses data binding to connect with the API response. Template expressions in the card automatically populate with real data from your API, allowing your agent to present information in a polished, visually appealing format instead of plain text.

Scrolling down you can find the runtime settings:

```
"runtimes": [
  {
    "type": "OpenApi",
    "auth": {
      "type": "None"
    },
    "spec": {
       "url": "apiSpecificationFile/trey-definition.json"
    },
    "run_for_functions": [
      "getConsultants",
      "getUserInformation",
      "postBillhours"
    ]
  }
],
```
They include a pointer to the trey-definition.json file, and an enumeration of the available functions.
### Add the Declarative Agent to Your App Manifest
1. - [ ] Open the **manifest.json** file within the **appPackage** directory
2. - [ ] Add a new `"copilotAgents"` object with a `"declarativeAgents"` object inside, just before the `"staticTabs"` object, as follows. This references the declarative agent JSON file you created in the previous step:
```
  "copilotAgents": {
    "declarativeAgents": [
      {
        "id": "treygenie",
        "file": "trey-declarative-agent.json"
      }
    ]
  },
  ```
Be sure to save your work.
### Remove the Dummy Feature from the App Manifest
The initial solution that you ran in the previous exercise didn't have a declarative agent yet, so the manifest would not install because it had no features. We added a "dummy" feature, which is a static tab pointing to the Copilot Developer Camp home page. This would allow users to view the Copilot Developer Camp website in a tab within Teams, Outlook, and the Microsoft 365 app ([https://office.com](https://office.com)).

If you've ever tried [Teams App Camp](https://aka.ms/app-camp), you would know all about them. If not, don't worry about it-just delete these lines from **manifest.json** as they aren't needed anymore:
```
"staticTabs": [
  {
    "entityId": "index",
    "name": "Copilot Camp",
    "contentUrl": "https://microsoft.github.io/copilot-camp/",
    "websiteUrl": "https://microsoft.github.io/copilot-camp/",
    "scopes": [
      "personal"
    ]
  }
],
"validDomains": [
  "microsoft.github.io"
],
```
## Step 3: Run and Test the Declarative Agent
### Run the New Project
1. - [ ] If you're still in the debugger, stop it to force a complete re-deployment
2. - [ ] Start the debugger by pressing **F5** or selecting **Run > Start Debugging** from the VS Code menu 

### Test the Declarative Agent

You should be automatically redirected to a browser tab where the "Trey Genie local" agent is launched in an immersive experience. 

If not you have probably hit an issue as below:

!IMAGE[launch-error.png](instructions315038/launch-error.png)

- [ ] You can just select **Cancel** to ignore it. Then open the Copilot chat by opening `https://m365.cloud.microsoft/chat/?auth=2` in the browser session. Then use the left flyout 1️⃣ to show your previous chats and declarative agents, then select the **Trey Genie Local** agent 2️⃣.

!IMAGE[flyout.png](instructions315038/flyout.png)

- [ ] Try a prompt such as: `Please list my projects along with details from the Statement of Work doc` 
- [ ] When you query your agent and it attempts to access an API like in this case getting list of projects, you'll receive a prompt requesting permission to perform this action. To proceed, select either "Allow once" or "Always allow" as shown below.


!IMAGE[allow.png](instructions315038/allow.png)

Once allowed, you should see a list of your projects from the API plugin 1️⃣, enhanced with details from each project's Statement of Work 2️⃣. 
Scroll to the end of the response to find source or citations. Notice that Copilot includes these citations as references to the documents 3️⃣. Click one of the references to check out the document.

!IMAGE[response-sow.png](instructions315038/response-sow.png)

- [ ] Let's also see how the API is getting called. Try to send another prompt: `List my information`. 



Once allowed, the agent will call the API "api/me" and return information based on the query for the logged-in user, which in our case is Avery Howard 1️⃣.  
!IMAGE[avery-text.png](instructions315038/avery-text.png)

Scroll to the end of the response to find source or citations. Select Source 1️⃣. In the Source tab, under citations expand Avery Howard and you will see the rich card response using adaptive cards 2️⃣.

!IMAGE[final-me-response.png](instructions315038/final-me-response.png)

Avery Howard is the logged in user as we have not yet implemented Auth.



- [ ] If you go back to your VS Code project under "Terminal", you will also see how the agent called the API as shown below:

!IMAGE[api-call-code.png](instructions315038/api-call-code.png)

**CONGRATULATIONS!**

You've completed adding a declarative agent to your API plugin. Proceed to wrap up. 


===

# Post-lab Confidence & Knowledge Survey
@lab.ActivityGroup(completionsurvey)

===

# Wrap Up

---
## Next Steps

* Customize your agent's tone, instructions, or capabilities
* Experiment with new API, adaptive cards for all operations etc.
* Fork the starter repo and keep building 👉 [Copilot Developer Camp](https://aka.ms/copilotdevcamp)

---
## Resources

- [Copilot Developer Camp](https://aka.ms/copilotdevcamp)

---

## Congratulations! You've completed the lab
- [ ] End this lab by selecting **End** at the upper right of the instructions pane. 

!IMAGE[e8avjirx.jpg](instructions315038/e8avjirx.jpg)


>[!Alert] **IMPORTANT:** These labs are hosted on the Skillable platform. Completion data is collected and then exported to Success Factors every Monday. Success Factors requires another 1-3 days to process that data. The status for this lab will be visible in Viva and Learning Path the following week.