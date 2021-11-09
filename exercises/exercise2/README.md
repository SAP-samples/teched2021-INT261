# Build a digital bot with SAP Intelligent Robotic Process Automation

< Back to [Exercise0: Introduction and Setup](../exercise0/README.md)

< Back to [Exercise1: SAP Workflow Management](../exercise1/README.md)

## Table of contents

<!-- TOC -->

  - [Introduction](#introduction)
  - [Overview](#overview)
  - [Pre-requisites](#pre-requisites)
  - [Factory Settings](#factory-settings)
    - [Agent connection check](#agent-connection-check)
    - [Create a Test Environment](#create-a-test-environment)
    - [Add an Agent to the Environment](#add-an-agent-to-the-environment)
  - [Build your Bot in Cloud Studio Project](#build-your-bot-in-cloud-studio-project)
    - [Import the provided Project](#import-the-provided-project)
    - [Create a Project from scratch](#create-a-project-from-scratch)
    - [Create an Automation, configure the Agent version and build the automation](#create-an-automation-configure-the-agent-version-and-build-the-automation)
    - [Test the automation project](#test-the-automation-project)
    - [Add a Project Pane for automations menu items](#add-a-project-pane-for-automations-menu-items)
  - [Get your project ready](#get-your-project-ready)
    - [Generate and release a Package from your Project](#generate-and-release-a-package-from-your-project)
    - [Add a Package to the Environment](#add-a-package-to-the-environment)
    - [Add a Trigger for the Package](#add-a-trigger-for-the-package)
    - [Check Agent Mode and Project Assignment](#check-agent-mode-and-project-assignment)
  - [Approve the capital expenditure request](#approve-the-capital-expenditure-request)
    - [Congratulations, you have done all the approvals for this new capital expenditure request, which has been started via a digital bot.](#congratulations-you-have-done-all-the-approvals-for-this-new-capital-expenditure-request-which-has-been-started-via-a-digital-bot)
  - [Conclusion](#conclusion)

<!-- /TOC -->

## Introduction

In this session, you will learn how to use the SAP Intelligent RPA to build your first bot that calls a Workflow Management service and triggers Capital Expenditure Approval Process.

![02-001](./images/02-001.png)

## Overview

Handling data from different sources to prepare them for business processes in several and heterogenous applications is a challenging task. Moreover, when the data are collected form the UIs of these applications. With SAP Intelligent RPA, you have a complete toolbox that allows you building all kind of automations to cope these challenges.

The challenge we will resolve in this session is related to Capital Expenditure Approvals in Workflow Management. The process starts when any company receives a funding request for capital expenditures. These requests are then thoroughly reviewed, evaluated, and finally approved or rejected based on the available budgets for the current period.

We assume all the approvals are already handled inside a Workflow Management use case and think how the workflow could be triggered using SAP Intelligent RPA. The bot will:

  - Read data from JSON file
  - Prepare a payload of the workflow service based on environment variables (like Workflow ID, API end point and Credentials) stored in the Cloud Factory in a secured way.
  - Handle the call of the web service authenticating and getting the required token.
  - Posting the CAPEX request with the needed payload.
  - Get the result of the request confirming that the workflow is triggered.

In addition to building your bot, you will experience some RPA Officer tasks and explore others on your own:

  - Creating an Environment and add your Desktop Agent.
  - Generate a Package and release it.
  - Deploy a Package to an Environment and add a Trigger.
  - Check your Desktop Agent mode.
  - Explore Monitoring Jobs, Agents Groups, Alerts, Content from the Store.


  [![02-003](./images/02-003.png)

**Pre-requisites**

To be able to execute the following Hands-on, you need:

1. SAP Intelligent RPA Cloud Factory access as IRPRA Officer
2. Windows machine with on-premise components installed

Please, check the technical prerequisites and System requirements in the official documentation [here](https://help.sap.com/viewer/6b9c8e86a0be43539b670de962834562/Cloud/en-US/0061438816a34fa78b77c99852318c70.html), and refer to the Setup Guide provided to you.

- JSON file [request.json](Files/request.json) containing the context of the CAPEX Request that will be handled in the business process.

This file should be in this folder `c:\users\Public\saprpa`

And should follow the following JSON template.

> **Important:** Make sure you are using the email address from your SAP BTP trial account for "Email" and "UserId" in this file.

![02-004](./images/02-004.png)


```json
{
  "RequestId": "Investment Approval 01",
  "Title": "App Creation",
  "Requester": {
    "FirstName": "John",
    "LastName": "Doe",
    "Email": "<your email address in SAP BTP trial>",
    "UserId": "<your email address in SAP BTP trial>",
    "Comments": "Please Approve"
  },
  "Investment": {
    "TotalCost": 50000,
    "Type": "Software",
    "CAPEX": 10000,
    "OPEX": 2000,
    "ROI": 5,
    "IRR": 5,
    "Country": "Germany",
    "BusinessUnit": "Purchasing",
    "Description": "Provide a fresh experience for our customers by providing new apps for our services"
  },
  "Sustainability": {
    "EnergyEfficiency": 10,
    "CO2Efficiency": 20,
    "EnergyCostSavings": 15,
    "WaterSavings": 10
  }
}
```

## Factory Settings

### Agent connection check

1. Go back to the **trial** subaccount.

2. Click on **Instances and Subscriptions**

3. Click on **SAP Intelligent Robotic Process Automation Trial**

    ![02-005](./images/02-005.png)

4. Click on **Agents** tab

    ![02-007](./images/02-007.png)

5. Check that your **Agent** is listed with status **Idle**

    ![02-008](./images/02-008.png)

### Create a Test Environment

1. Navigate to **Environments**

2. Click on **New Environment**

    ![02-009](./images/02-009.png)

3. Set a **Name** for the Environment (ex. **Test Environment**)

4.Choose **Test** as Type

5. Click on **Create**

    ![02-010](./images/02-010.png)

### Add an Agent to the Environment

1. In the Environment, click on **Add Agent**

    ![02-011](./images/02-011.png)

2. Select your Agent

3. Click on **Add agent**

    ![02-012](./images/02-012.png)

4. The Environment is now configured with your Agent

    ![02-013](./images/02-013.png)

## Build your Bot in Cloud Studio Project

> For this part we offer you two possibilities
>   - [Import the provided Project](#import-the-provided-project)
>   - [Create a Project from scratch](#create-a-project-from-scratch)
>
> Please select your desired possibility above
### Import the provided Project

1. Download the [CapexMananagement.zip](Files/CapexManagement.zip) containing the entire project

2. Before you can import the project file you need to get the Core SDK. To do this go to the **Store** tab, check the catalog **SAP Intelligent RPA SDK**, put **Core** in the search field, press the search button and select the **Core SDK**.

    ![02-014a](./images/02-014a.png)

3. Click on the **Get** button to download the SDK

    ![02-014b](./images/02-014b.png)

4. In the Factory, click on the **Projects** tab

    ![02-014](./images/02-014.png)

5. In the Projects page click on **Import**

    ![02-015a](./images/02-015a.png)

6. Select the downloaded file and press **Import**

    ![02-015b](./images/02-015b.png)

    > Note: You may get a error message that the project file requires a different SDK version. To fix this go back to the store, select **Show All Versions** and **Get** the version needed for the project.

    ![02-015e](./images/02-015e.png)

    ![02-015c](./images/02-015c.png)

    ![02-015d](./images/02-015d.png)

#### Congratulations! you have sucessfully imported the project.

![02-015f](./images/02-015f.png)

> #### You can now skip the next section and continue with section [Get your project ready](#get-your-project-ready)

### Create a Project from scratch

1. In the Factory, click on the **Projects** tab

    ![02-014](./images/02-014.png)

2. In the Projects tab click on **New Project**

    ![02-015](./images/02-015.png)

3. Set a **Project Name**

4. Click **Create**. This will open the IRPA Cloud Studio in the next tab.

    ![02-016](./images/02-016.png)

5. In the left panel, click on the **Gear** to open the Settings

    ![02-018](./images/02-018.png)

6. Select the **Environment Variables/Set Environment Variables**

    ![02-019](./images/02-019.png)

7. Click on **Create**

    ![02-020](./images/02-020.png)
8. Set **definitionID** as Identifier

    ![02-021](./images/02-021.png)

9.  Click on the **Type** list

    ![02-022](./images/02-022.png)

10. Select **String**

    ![02-023](./images/02-023.png)

11. Click on **Create**

    ![02-023](./images/02-023.png)

12. Follow the same step to create 4 variables similar to what is shown in the screenshot:
    - **serviceUrl** : as String
    - **uaaClientId** : as String
    - **uaaClientSecret** : as Password
    - **uaaUrl** : as String

    ![02-024](./images/02-024.png)

13. Once done, click on Close

    ![02-025](./images/02-025.png)

### Create an Automation, configure the Agent version and build the automation

1. In the left-hand Panel and click on the **+** button and select **Create**

2. Select **Automation**

    ![02-026](./images/02-026.png)

3. Click on the **Agent version** list

4. Select the latest localversion released

    > **Note** : if Local is not shown, choose the version of the Desktop agent installed in your machine.

    ![02-027](./images/02-027.png)

5. Click on **Confirm**

    ![02-028](./images/02-028.png)

6. Set a **Name** for the automation

7. Click on **Create**

    ![02-029](./images/02-029.png)

8. In the right-hand panel, Click on the **(Input/Output)** tab and click on **Add new output parameter**

    ![02-030](./images/02-030.png)

10.  Set the **Name** of the Parameter to **resCall**

11. Click on **Type** list

12. Choose type **Any**

    ![02-032](./images/02-032.png)

13. Click on **Tools**

    ![02-033](./images/02-033.png)

14. Search for the **Read File** activity

    ![02-034](./images/02-034.png)

15. Drag the **Read File** activity

16. Drop it on the automation flow

    ![02-035](./images/02-035.png)

17. Click on the **Read File** activity to edit the Step details

    ![02-036](./images/02-036.png)

18. Set the **filePath** to the json file of the Capex request you have prepared (e.g. `c:\users\Public\saprpa\request.json`)

19. Select the **proposed value** as String

20. Click on the **reset** button of the **content** output parameter to rename it

    > **Note** : Make sure you have a json file with name**request.json** with the required start context in**C:\Users\Public** . The bot will read the file to determine the payload to start the workflow.

    ![02-037](./images/02-037.png)

21. Set **Name** of the output parameter to **context**

    ![02-038](./images/02-038.png)

22. Click on Close

    ![02-039](./images/02-039.png)

23. Clear the **search field**

    ![02-040](./images/02-040.png)

24. Search the **Encode String** activity:

25. Drag it

26. Drop it on the automation flow

    ![02-041](./images/02-041.png)

27. Click on the activity to edit the details

    ![02-042](./images/02-042.png)

28. Click on the **Expression Editor**

    ![02-043](./images/02-043.png)

29. Set the value of the input to:

    `$.uaaClientId + ':' + $.uaaClientSecret.toString()`

30. Click on **Save Expression**

    ![02-044](./images/02-044.png)


31. Clear the **name** of the output parameter

    ![02-045](./images/02-045.png)

32. Set the name to: **clientInfo**

    ![02-046](./images/02-046.png)

33. Click on Close the step properties

    ![02-047](./images/02-047.png)

34. Clear the **search field**

    ![02-048](./images/02-048.png)

35. Search for **Custom Script** activity and drag and drop it to the workflow

    ![02-049](./images/02-049.png)

36. Click on the **Custom Script** activity

    ![02-050](./images/02-050.png)

37. Rename the Step name to: **Generate Options - token**

    ![02-051](./images/02-051.png)

38. Click on the **Edit Script** button or Double-click on the **Custom Script** to open the script editor.

    In addition to the Step Details on the right, a large panel will open in the middle for the custom code.

    ![02-052](./images/02-052.png)

39. Adding 2 input parameters: (**clientInfo** and **uaaUrl**) and 1 output parameter: **optionsGet**

40. In the right-hand panel, Click on **Add new input parameter**

    ![02-053](./images/02-053.png)

41. Set name to: **clientInfo**

    ![02-054](./images/02-054.png)

42. Click on **Add new input parameter**

    ![02-055](./images/02-055.png)

43. Set name to: **uaaUrl**

    ![02-056](./images/02-056.png)

44. Click on **Add new output parameter**

    ![02-057](./images/02-057.png)

45. Set name to: **optionGet**

    ![02-058](./images/02-058.png)

46. Click on the **Type** list

47. Select type **Any**

    ![02-059](./images/02-059.png)

48. Copy the following code into the script editor:

    ```javascript
    return {
        method: 'GET',
        url: uaaUrl + '/oauth/token?grant_type=client_credentials',
        responseType: 'json',
        resolveBodyOnly: true,
        headers:{
            Authorization:'Basic ' + clientInfo
        }
    };
    ```

    ![02-060](./images/02-060.png)

49. Click on Close the Editor

    ![02-061](./images/02-061.png)

50. Click on the **Generate Options -token** step

    ![02-062](./images/02-062.png)

51. Click on the **clientInfo** field, and Select the **clientInfo** form the available variables (result of step 2)

    ![02-063](./images/02-063.png)

52. Click on the **uaaUrl** field, and Select the **uaaUrl** form the available variables (environment variable)

    ![02-064](./images/02-064.png)

53. Click on Close step Details

    ![02-065](./images/02-065.png)

54. Clear the **search field** and Search for the **Call We Service** activity

    ![02-066](./images/02-066.png)

55. Drag the **Call Web Service** activity

56. Drop it on the automation flow

    ![02-067](./images/02-067.png)

57. Set Step name as: **Call Web Service – Get**

    ![02-068](./images/02-068.png)

58. Set the **options** to: **optionGet** (result of Step 3)

    ![02-069](./images/02-069.png)

59. Clear the output parameter

60. Set a name as: **resToken**

    ![02-070](./images/02-070.png)

61. Click on Close the Step details

    ![02-071](./images/02-071.png)

62. Clear the **search field** and Search for the **String** activity

    ![02-072](./images/02-072.png)

63. Drag the **String** activity from Data Type

64. Drop it on the workflow

    ![02-073](./images/02-073.png)

65. Click on the **Create string variable** to edit the **Step** **details**

    ![02-074](./images/02-074.png)

66. Open the **Expression Editor**

    ![02-075](./images/02-075.png)

67. Add
    ```javascript
    'Bearer ' + Step4.resToken.access_token
    ```

68. Click on **Save Expression**

    ![02-076](./images/02-076.png)

69. Clear the output parameter

    ![02-077](./images/02-077.png)

70. Set Name to: **authToken**

    ![02-078](./images/02-078.png)

71. Click on Close Step Details

    ![02-079](./images/02-079.png)

72. Clear the **search field** and Search for **Custom Script** activity

    ![02-080](./images/02-080.png)

73. Drag the **Custom script** activity
74. Drop it on the automation flow

    ![02-081](./images/02-081.png)

75. Double-click on the **Step** to show **details** panel and open **script editor**

    ![02-082](./images/02-082.png)

76. Add the following input parameters:

    - **token** as String
    - **serviceUrl** as String
    - **definitionID** as String
    - **context** : as String

    ![02-083](./images/02-083.png)

77. Add the output parameter: **optionsUpload** with type **Any**

    ![02-084](./images/02-084.png)

78. Copy the following code to the **script editor** :

    ```javascript
    let input = {
        "definitionId": definitionID=="default"?"highvalueinvestment" : definitionID,
        "context": irpa_core.unserialize(context, true)
    };return {
        method: 'POST',
        url: serviceUrl+"/v1/workflow-instances",
        responseType: 'json',
        resolveBodyOnly: true,    headers:{
            Authorization: token,
            'Content-Type': irpa_core.enums.request.content.json
        },
        json: input
    };
    ```

    >Remark: Ensure you have the same workflow definitionID as defined before for the new process variant in SAP Workflow Management.

    ![02-085](./images/02-085.png)

79. Close the **Script Editor** and Click on the **Custom Script** step

    ![02-086](./images/02-086.png)

80. Set the Step name to **Generate Options - upload**

81. Set the token to **authToken** (output of step 5)

82. Set the serviceUrl to **serviceUrl** (environment variable)

83. Set the definitionID to **definitionID** (environment variable)

84. Set the context to **context** (output of step 1)

    ![02-087](./images/02-087.png)

85. Check the final settings and Click on Close Step details

    ![02-088](./images/02-088.png)

86. Clear the **search field** and Search for **Call Web Service** activity

    ![02-089](./images/02-089.png)

87. Drag the **Call Web Service** activity

88. Drop it on the automation flow

    ![02-090](./images/02-090.png)

89. Click on **Call Web Service** activity to edit the Step details

    ![02-091](./images/02-091.png)

90. Set the **Step name** : **Call Web Service – POST**

    > **Note** : renaming the activities allows you to distinguish the steps. For example, in this automation, we have 2 Request Calls a GET for the Token and a POST of the request.

    ![02-092](./images/02-092.png)
|
91. Set input parameter options to **optionsUpload** (output of step 6)


92. Rename the output parameter to **resUpload**

93. Close the Step details

    ![02-093](./images/02-093.png)

94. Click on the **End** node of the automation

    ![02-094](./images/02-094.png)

95. Set the output parameter resCall to **resUpload** (output of step 7)

    ![02-095](./images/02-095.png)

96. Click on **Save** the automation

    ![02-096](./images/02-096.png)

### Test the automation project

1. Click on the **Play** button

    ![02-097](./images/02-097.png)

2. Choose the Environment you already created
    - uaaUrl
    - definitionID
    - serviceUrl
    - uaaClientId
    - uaaClientSecret


3. Click on the **Test** button

    > **Note** : use the information details of the service you created during the setup steps in section [Create Workflow Service Instance and Key](../1_GettingStarted/README.md#create-workflow-service-instance-and-key).
    >
    > Ensure that the *definitionID* it is the same as created by you in the exercise before (e.g. **highvalueinvestment**).

    ![02-098](./images/02-098.png)

4. The steps are displayed in the timeline, on the left of the automation canvas.

    If the automation is tested without errors:
    - Click on the **End** node in the **Tester**
    - Click to expand the **resCall** node to show the result of the test

    ![02-099](./images/02-099.png)

### Add a Project Pane for automations menu items

1. Click on **+** in the left-hand panel

2. Select the  **Project Pane**

    ![02-100](./images/02-100.png)

3. Set a **Name** as **Capex Menu**

4. Click on **Create**

    ![02-101](./images/02-101.png)

5. Drag the automation **Capex Call**

6. Drop it on the **Project Pane**

    ![02-102](./images/02-102.png)

7. Click on **Save**

    ![02-103](./images/02-103.png)

## Get your project ready

### Generate and release a Package from your Project

1. Check that the project is free of errors in the **Design Console**

    ![02-104](./images/02-104.png)

2. Click on **Generate Package**

    Here, you have 2 potential situations:

    1. If you generate the package for the first time like in the case of this hands-on
        - Set a **Name** for your Package: usually the default proposed
        - Click on **Generate Package**

            ![02-105](./images/02-105.png)

    2. If the project has already a generated package, you will be asked to choose among the 3 options.
        - Select the suitable option for your package and this will update the version number accordingly
        - Click on **Generate Package**.

             ![02-106](./images/02-106.png)

3. Navigate to the Cloud Factory and then go to the **Packages** tab

4. Click on the **>** arrow on Capex Management, select your version and click **...** for more options

5. Click on **Release**

    ![02-107](./images/02-107.png)

6. Click on **Release** to confirm

    ![02-108](./images/02-108.png)

7. Click on **Environment** tab

    ![02-109](./images/02-109.png)

8. Click on your **Environment**

    ![02-110](./images/02-110.png)

### Add a Package to the Environment

1. Click on **Add Package**

    ![02-111](./images/02-111.png)

2. Select the **Generated Package**

3. Click on **Next**

    ![02-112](./images/02-112.png)

4. Set Environment Variables:
    - In **definitionID** : Set the workflow definition id

        > **Remark** : ensure it is the same as created by you in the Workflow Management exercise before (e.g. **highvalueinvestment**)

    - In **uaaClientSecret** : Set the client secret value of the workflow service instance key.

    - In **uaaUrl:** set the authentication Url of the workflow service instance key.

    - Scroll to the following variables

      > **Note** : use the information details of service you created during the Onboarding steps in section [Create Workflow Service Instance and Key](../1_GettingStarted/README.md#create-workflow-service-instance-and-key)


  - ![02-113](./images/02-113.png)

5. In **serviceURL** : set the end point Url of the workflow service instance key.

6. In **uaaClientId** : set the client Id of the workflow service instance key.

7. Click **Ok**

    > **Note** : use the information details of service you created during the Onboarding steps in section [Create Workflow Service Instance and Key](../1_GettingStarted/README.md#create-workflow-service-instance-and-key)

    ![02-114](./images/02-114.png)

    Your package will be seen in your Environment **without trigger**

    ![02-115](./images/02-115.png)

### Add a Trigger for the Package

1. Click on **Add Trigger**

    ![02-116](./images/02-116.png)

2. Select the **Deployed Package** for which you will add the **Trigger**

3. Click on **Next**

    ![02-117](./images/02-117.png)

4. The Environment variables were set when the package was deployed into the environment, if you don't need to update them, Click on **Next**

    ![02-118](./images/02-118.png)

5. Choose **Attended** Trigger

6. Click on **Next**

    ![02-119](./images/02-119.png)

7. Add/tune your Trigger settings
    - Set a **Name**
    - **Define** **Date range** and **Timezone**
    - **Customize** you **daily availabilities** for the weekdays
    - Click on **Create**

    ![02-120](./images/02-120.png)

The environment is now set up with:
  - An agent
  - A Capex Management package
  - Five Environment variables
  - An Attended trigger for the package

![02-121](./images/02-121.png)

### Check Agent Mode and Project Assignment

1. Check if the **Desktop Agent** is started from the **Systray**
      - If yes, ignore the steps below
      - If not, do the steps below

      1. Search for **Desktop Agent**
      2. Click on **Desktop Agent**

      ![02-122](./images/02-122.png)

2. Click on the **Desktop Agent Systray**

3. Click on **Projects** to check the **Agent mode**

    ![02-123](./images/02-123.png)

5. Check that the agent is set to **Interactive (attended)** mode

    ![02-124](./images/02-124.png)

6. If the agent is in test mode click on the three dots to restart it

    ![02-125](./images/02-125.png)

11. Click on **Projects**

    ![02-123](./images/02-123.png)

12. Check the project **Capex Management** exists in the list

13. Click on **Start**

    ![02-128](./images/02-128.png)

    Your **Desktop Agent** will restart with your project ready to test!

14. Click on the **Desktop Agent Systray**

15. Click on your automation **Start Capex Call** to start

    ![02-129](./images/02-129.png)

## Approve the capital expenditure request

Now you have started the workflow via the RPA bot.

Let us now approve the tasks accordingly within SAP Workflow Management.

In this exercise, you will first approve the task created as a local manager. After the approval, the process moves to the next approval step to the CFO approval. You will again receive a task in My Inbox, where you can approve the task to complete the capital expenditure approval process.

1. Go back to your trial account in SAP BTP Cockpit.
Open the SaaS application" **Workflow Management**", click the three dots and select **Go to Application**.

    ![02-130](./images/02-130.png)

2. Navigate to Workflow Management home screen, choose  **My Inbox**  tile.
You can see that there is one task that requires your approval.

    ![02-131](./images/02-131.png)

3. Choose the approval task from the **All Tasks** list. You can view details of the task that requires your action such as, Investment Details, Sustainability, Investment Requester, History, and Comments.

    ![02-132](./images/02-132.png)

4. Choose  **Approve**  to approve the capital expenditure request.

    ![02-133](./images/02-133.png)

5. Similarly, you would have a new task in the  **My Inbox**  tile for your approval as a CFO. Act accordingly.

    ![02-134](./images/02-134.png)

#### Congratulations! you have done all the approvals for this new capital expenditure request, which has been started via a digital bot.

## Summary

You have now successfully created your Bot that triggers a Workflow for a Capital Expenditure Approval process.

After checking you agent connection, you created your Environment and added your agent to get ready to use the cloud studio.

You went through an automation building process, including the test of the automation and the creation a Project Pane for your Bot.

Last but not least, you did some Officer task where you generated a package from the project and released it. After deploying the package into the environment, you added an Attended trigger and customized the Date range, time zone and daily availabilities.

Starting the Desktop agent allowed you to discover the different Agent Modes and also test your bot in your desktop.

Finally, you approved the related tasks and combined SAP Intelligent Robotic Process Automation with SAP Workflow Management.

< Back to [Exercise0: Introduction and Setup](../exercise0/README.md)

< Back to [Exercise1: SAP Workflow Management](../exercise1/README.md)
