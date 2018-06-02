![](images/HeaderPic.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Intelligent analytics
</div>

<div class="MCWHeader2">
Hands-on lab step-by-step
</div>

<div class="MCWHeader3">
March 2018
</div>


Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.
© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [Intelligent analytics hands-on lab step-by-step](#intelligent-analytics-hands-on-lab-step-by-step)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Overview](#overview)
    - [Solution architecture](#solution-architecture)
    - [Requirements](#requirements)
    - [Exercise 1: Environment setup](#exercise-1--environment-setup)
        - [Task 1: Connect to the lab VM](#task-1--connect-to-the-lab-vm)
        - [Task 2: Download and open the ConciergePlus starter solution](#task-2--download-and-open-the-conciergeplus-starter-solution)
        - [Task 3: Create App Services](#task-3--create-app-services)
        - [Task 4: Provision Service Bus](#task-4--provision-service-bus)
        - [Task 5: Provision Event Hubs](#task-5--provision-event-hubs)
        - [Task 6: Provision Azure Cosmos DB](#task-6--provision-azure-cosmos-db)
        - [Task 7: Provision Azure Search](#task-7--provision-azure-search)
        - [Task 8: Create Stream Analytics job](#task-8--create-stream-analytics-job)
        - [Task 9: Start the Stream Analytics Job](#task-9--start-the-stream-analytics-job)
        - [Task 10: Provision an Azure Storage Account](#task-10--provision-an-azure-storage-account)
        - [Task 11: Provision Cognitive Services](#task-11--provision-cognitive-services)
    - [Exercise 2: Implement message forwarding](#exercise-2--implement-message-forwarding)
        - [Task 1: Implement the event processor](#task-1--implement-the-event-processor)
        - [Task 2: Configure the Chat Message Processor Web Job](#task-2--configure-the-chat-message-processor-web-job)
            - [Event Hub connection string](#event-hub-connection-string)
            - [Event Hub name](#event-hub-name)
            - [Storage account](#storage-account)
            - [Service Bus connection String](#service-bus-connection-string)
            - [Chat topic](#chat-topic)
            - [Text Analytics API settings](#text-analytics-api-settings)
    - [Exercise 3: Configure the Chat Web App settings](#exercise-3--configure-the-chat-web-app-settings)
        - [Task 1: Event Hub connection String](#task-1--event-hub-connection-string)
        - [Task 2: Event Hub name](#task-2--event-hub-name)
        - [Task 3: Service Bus connection String](#task-3--service-bus-connection-string)
        - [Task 4: Chat topic path and chat request topic path](#task-4--chat-topic-path-and-chat-request-topic-path)
    - [Exercise 4: Deploying the App Services](#exercise-4--deploying-the-app-services)
        - [Task 1: Publish the ChatMessageSentimentProcessor Web Job](#task-1--publish-the-chatmessagesentimentprocessor-web-job)
        - [Task 2: Publish the ChatWebApp](#task-2--publish-the-chatwebapp)
        - [Task 3: Testing hotel lobby chat](#task-3--testing-hotel-lobby-chat)
    - [Exercise 5: Add intelligence](#exercise-5--add-intelligence)
        - [Task 1: Implement sentiment analysis](#task-1--implement-sentiment-analysis)
        - [Task 2: Implement linguistic understanding](#task-2--implement-linguistic-understanding)
        - [Task 3: Implement speech to text](#task-3--implement-speech-to-text)
        - [Task 4: Re-deploy and test](#task-4--re-deploy-and-test)
    - [Exercise 6: Building the Power BI dashboard](#exercise-6--building-the-power-bi-dashboard)
        - [Task 1: Create the static dashboard](#task-1--create-the-static-dashboard)
        - [Task 2: Create the real-time dashboard](#task-2--create-the-real-time-dashboard)
    - [Exercise 7: Enabling search indexing](#exercise-7--enabling-search-indexing)
        - [Task 1: Verifying message archival](#task-1--verifying-message-archival)
        - [Task 2: Creating the index and indexer](#task-2--creating-the-index-and-indexer)
        - [Task 3: Update the Web App web.config](#task-3--update-the-web-app-webconfig)
        - [Task 4: Configure the Search API App](#task-4--configure-the-search-api-app)
        - [Task 5: Re-publish apps](#task-5--re-publish-apps)
    - [After the hands-on lab](#after-the-hands-on-lab)
        - [Task 1: Delete the resource group](#task-1--delete-the-resource-group)

<!-- /TOC -->

# Intelligent analytics hands-on lab step-by-step

## Abstract and learning objectives

This package is designed to facilitate learning real-time analytics without IoT. Participants will enable intelligent conversation in a machine learning-enabled, real-time chat pipeline to allow hotel guests to chat with one another, and to communicate directly with the concierge. They will also apply analytics to visualize customer sentiment in real-time. After completion, students will be better able to implement a lambda architecture, and enable web-based real-time messaging thru Web Sockets, Event Hubs, and Services Bus. In addition, participants will better understand how to:

-   Leverage Cognitive Services (LUIS & Text Analytics API)

-   Process Events with Web Jobs

-   Index with Search

-   Archive with Cosmos DB

-   Visualize with Power BI Q&A

## Overview

Adventure Works Travel specializes in building software solutions for the hospitality industry. Their latest product is an enterprise mobile/social chat product called Concierge+ (aka ConciergePlus). The mobile web app enables guests to easily stay in touch with the concierge and other guests, enabling greater personalization and improving their experience during their stay. Sentiment analysis is performed on top of chat messages as they occur, enabling hotel operators to keep tabs on guest sentiments in real-time.

## Solution architecture

Below are diagrams of the solution architecture you will build in this lab. Please study this carefully, so you understand the whole of the solution as you are working on the various components.

![The preferred solution is shown to meet the customer requirements. From right to left there is an architecture diagram which shows the connections from a mobile device to a Web Application. The Web Application is shown setting data to an Event Hub which is connected to a Web Job. From there Event Hub and Service Bus work together with Stream Analytics, Power BI and Cosmos DB to provide the full solution.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image2.png "Solution architecture")

## Requirements

-   Microsoft Azure subscription must be pay-as-you-go or MSDN.

    -   Trial subscriptions will not work.

-   A virtual machine configured with:

    -   Visual Studio Community 2017 or later

    -   Azure SDK 2.9 or later (Included with Visual Studio 2017)


## Exercise 1: Environment setup

Duration: 60 minutes

Synopsis: The following section walks you through the manual steps to provision the services required using the Azure Portal. Adventure Works has provided a starter solution for you. They have asked you to use this as the starting point for creating the Concierge Plus intelligent chat solution in Azure.

### Task 1: Connect to the lab VM

If you are already connected to your Lab VM, skip to Step 6.

1.  Navigate to the Azure portal, and select Resource groups from the left-hand menu, then enter intelligent-analytics into the filter box, and select the resource group from the list. 
    
    ![In the Azure Portal Resource groups pane, intelligent-analytics is typed in the Subscriptions search field. Under Name, intelligent-analytics is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image10.png "Azure Portal Resource groups")

2.  Next, select **LabVM** from the list of available resources. 
    
    ![In the List of available resources, the virtual machine LabVM is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image11.png "List of available resources")

3.  On the LabVM blade, select **Connect** from the top menu, which will download an RDP file. 
    
    ![The Connect button is circled on the LabVM blade.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image12.png "LabVM blade")

4.  Open the downloaded RDP file.

5.  Select Connect on the Remote Desktop Connection dialog. 
    
    ![The Remote Desktop Connection window states that the publisher of the remote connection can\'t be identified, and asks if you want to connect anyway. The Connect button is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image13.png "Remote Desktop Connection")

6.  Enter the following credentials (or the non-default credentials if you changed them):

    a.  User name: **demouser**

    b.  Password: **Password.1!!**

    ![The Windows Security window asks you to enter the credentials for demouser.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image14.png "Windows Security window")

7.  Select Yes to connect, if prompted that the identity of the remote computer cannot be verified. 

    ![The Remote Desktop Connection window states that the identity of the remote computer can't be identified, and asks if you want to connect anyway. The Yes button is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image15.png "Remote Desktop Connection window ")

8.  Once logged in, launch the **Server Manager**. This should start automatically, but you can access it via the Start menu if it does not.

9.  Select Local Server, then select **On** next to **IE Enhanced Security Configuration**. 

    ![On the Server Manager Start menu in the left pane, Local Server is selected. In the right, Properties pane, IE Enhanced Security Configuration is set to On, and is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image16.png "Server Manager Start menu")

10. In the Internet Explorer Enhanced Security Configuration dialog, select **Off under Administrators**, then select **OK**. 

    ![In the Internet Explorer Enhanced Security Configuration dialog box, the Administrators Off radio button is selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image17.png "Internet Explorer Enhanced Security Configuration dialog box")

11. Close the Server Manager.

### Task 2: Download and open the ConciergePlus starter solution

1.  From your Lab VM, download the starter project by copying and pasting the URL, <http://bit.ly/2wMsqW4>, into a web browser.

2.  Unzip the contents of the downloaded ZIP file to the folder **C:\\ConciergePlus**\\.

    ![In the Extract Compressed (Zipped) Folders window, files will be extracted to C:\\ConciergePlus\\.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image18.png "Extract Compressed (Zipped) Folders window")

3.  Open **ConciergePlusSentiment.sln** with Visual Studio 2017.

4.  Sign in to Visual Studio or select create account, if prompted.

5.  If presented with the Start with a familiar environment dialog, select Visual C\# from the Development Settings drop down list, and select Start Visual Studio.

    ![Development Settings are set to Visual C\# and are circled in the Visual Studio Start with a familiar environment dialog box.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image19.png "Visual Studio Start with a familiar environment dialog box")

6.  If the Security Warning window appears, uncheck Ask me for every project in this solution, and select OK. 

    ![In the Security Warning window, under the Would you like to open this project? prompt, the Ask me for every project in this solution checkbox is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image20.png "Security Warning window")

**Note**: If you attempt to build the solution at this point, you will see many build errors. This is intentional. You will correct these in the exercises that follow.

### Task 3: Create App Services

In these steps, you will provision two Web Apps and an API App within a single App Service Plan.

1.  Sign in to the Azure Portal (<https://portal.azure.com>).

2.  Select +Create a resource, then select Web + Mobile, and finally select Web App.

    ![Under the Azure Marketplace in the Azure Portal, New pane, Web + Mobile is selected. Under Featured, Web App (Quickstart tutorial) is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image21.png "Azure Markeplace create a resource")

3.  On the Create Web App blade, enter the following:

    -   App Name: Provide **a unique name** that is indicative of this resource being used to host the Concierge+ chat website. (e.g., conciergepluschatapp)

    -   Subscription: **Select your subscription**.

    -   Resource Group: Select Use existing, and select the **intelligent-analytics** resource group created previously.

    -   OS: **Windows**

    -   App Service plan/Location: Select **Create new**, and enter **awchatplus** for the App Service plan name, select the location you used for the resource group created previously, and choose a Pricing tier of **S1 Standard**.

    -   Select **OK** on the New App Service Plan blade.

    -   Select **Create** to provision both Web App and the App Service Plan.

    ![The Create web app blade fields display the previously mentioned settings. The App Service plan blade and the New App Service plan blade also display.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image22.png "Create web app blade")

4.  When provisioning completes, navigate to your new Web App in the portal by clicking on **App Services**, and then selecting your web app. 

    ![In the Azure Portal, in the App Services pane, under Name, conciergepluschatapp has a status of running, and is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image23.png "Azure Portal, App Services pane")

5.  On the App Service blade, select **Application settings**.

    ![Under Settings, Application settings is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image24.png "Settings section")

6.  Select the toggle for **Web Sockets** to **On**.

    ![The Web sockets toggle has the On button selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image25.png "Web sockets toggle")

7.  Select **Save**.
    
    ![Save is selected](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image26.png "Save option")

8.  You are now ready to provision the other Web App using this same service plan.

9.  Select **+Create a resource, Web + Mobile**, then **Web App**, like you did in Step 2 above.

10. On the Create Web App blade enter the following:

    -   App Name: Provide a **unique name** that is indicative of this resource being used to host the Event Processor WebJob (e.g., chatprocessorwebjob).

    -   Subscription: Select the same subscription used previously.

    -   Resource Group: Select Use existing, and select **the intelligent-analytics** resource group.

    -   OS: Select **Windows**.

    -   App Service plan/Location: Select the **awchatplus** App Service Plan, created above.

11. Select **Create** to provision the Web App. 
    
    ![The Create web app blade fields display the previously mentioned settings. The App Service plan blade also display.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image27.png "Create web app blade")

12. When the provisioning completes, navigate to your new Web App in the portal.

13. On the App Service blade, select Application settings.

    ![Under Settings, Application settings is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image24.png "App Service blade Settings section")

14. Set the **Always On** toggle to the **On** position. You want to make sure Always On is enabled for this so that the Web App hosting the Web Job never goes to sleep, and is always processing chat messages. 

    ![The Always on toggle has the On button selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image28.png "Always on toggle")

15. Select **Save**.
    
    ![Save is Selected](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image26.png "Save option")

16. Now, it's time to create an API App.

17. Select **+Create a resource, Web + Mobile, API App**. (Sometimes API App does not appear on the list. If that happens, simply click + New and search for API App.) 

    ![The Api App selection is shown](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image29.png "API App")

18. On the Create API App blade enter the following:

    -   App name: Provide a **unique name** for this API app that reflects it will host the Chat Search API (e.g., ChatSearchApi).

    -   Subscription: Select the same subscription as used previously.

    -   Resource Group: Select the **intelligent-analytics** Resource Group.

    -   App Service plan/Location: Select the **awchatplus** App Service plan.

    -   Select **Create**. 

        ![The Create API app blade fields display the previously mentioned settings. The App Service plan blade and the New App Service plan blade also display.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image30.png "Create API app blade")

### Task 4: Provision Service Bus 

In this section, you will provision a Service Bus Namespace and Service Bus Topic.

1.  Continuing within the Azure Portal, select +Create a resource.

2.  Select Enterprise Integration, then select Service Bus.

    ![The Azure Portal New blade has Enterprise Integration, and Service Bus circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image31.png "New blade")

3.  On the Create namespace blade enter the following:

    -   Name: Provide a **unique name** for the namespace (e.g., awhotel-namespace).

    -   Pricing tier: Select **Standard**.

    -   Subscription: Select the same subscription as used previously.

    -   Resource Group: Select the **intelligent-analytics** Resource Group.

    -   Location: Select the same Location you have been using.

        ![The Create namespace blade fields display the previously mentioned settings. ](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image32.png "Create namespace blade")

4.  Select **Create**.

5.  Once provisioning completes, navigate to your new Service Bus in the portal by clicking on Resource Groups in the left menu, the selecting intelligent-analytics, and selecting your Service Bus. 

    ![In the Azure Portal Resource Groups pane, under Name, the awhotel-namespace-1 Service bus is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image33.png "Azure Portal Resource Groups")

6.  On the Overview blade, click on Topic under Entities on the left-hand side of the blade.

    ![In the Overview blade Entities section, under Entities, Topics is selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image34.png "Overview blade Entities section")

7.  Add a new Topic by selecting +Topic.
    
    ![The Add Topic dialog is shown.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image35.png "Topic option")

8.  On the Create topic blade, enter the following:

    -   Name: Enter **awhotel**. This represents that this topic will handle the messages for a particular hotel.

    -   Max topic size: Leave set to **1 GB**.

    -   Message time to live: Set to **1 day**.

    -   Enable partitioning: **Uncheck this checkbox**. Chat will not function properly if this is left checked.

    -   Select **Create**.

        ![The Create topic blade fields display the previously mentioned settings. In addition, the following fields are circled: Name, which is set to awhotel, Message time to live in Days, which is set to 1, and the Enable partitioning check box.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image36.png "Create topic blade")

### Task 5: Provision Event Hubs

In this task, you will create a new Event Hubs namespace and instance.

1.  In the Azure Portal, select **+Create a resource**, then select **Internet of Things**, and select **Event Hubs**.

    ![In the Azure Portal, New pane, Internet of Things and Event Hubs (learn more) are both circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image37.png "New event hub")

2.  On the Create namespace blade enter the following:

    -   Name: Provide a **unique name** for the namespace (e.g., awhotel-events-namespace).

    -   Pricing tier: Select **Standard**.

    -   Subscription: Select the same subscription as used previously.

    -   Resource Group: Select **the intelligent-analytics** resource group.

    -   Location: Select the **same location** you have been using.

    -   Throughput Units: Leave at **1**

    -   Enable auto-inflate: **uncheck**

    -   Select **Create** to provision the Event Hubs namespace. 

        ![The Create namespace blade fields display the previously mentioned settings.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image38.png "Create namespace blade")

3.  When provisioning completes, navigate to your new Event Hub namespace in the portal by clicking on Resource Groups in the left menu. Select intelligent-analytics followed by your Event Hub. 
  
    ![Under Name, in the Resource pane, the awhotel-events-namespace Event Hub is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image39.png "Azure Portal Resource pane")

4.  On the Overview blade, click +Event Hub to add a new Event Hub.
    
    ![The add Event Hub is shown](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image40.png "Add event hub")

5.  On the Create Event Hub blade, enter the following:

    -   Name: Enter **awchathub**.

    -   Partition Count: Set to the **max value of 32**. This will enable you to significantly scale up the number of downstream processors on the Event Hub, where each partition consumer (as handled by the EventProcessorHost) can reach up to 1 Throughput Unit per partition should the need arise. You cannot change this value later.

    -   Message Retention: **Leave set to 1**.

    -   Capture: Leave set to **Off**.

    -   Leave the remaining values as their defaults.

    -   Select **Create**. 

        ![The Create Event Hub blade fields display the previously mentioned settings.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image41.png "Create Event Hub blade")

6.  Repeat steps 5a through 5f to create another Event Hub. This one will store messages for archival and be processed by Stream Analytics. Name it awchathub2.

### Task 6: Provision Azure Cosmos DB

In this section, you will provision an Azure Cosmos DB account, a DocumentDB Database, and a DocumentDB collection that will be used to collect all the chat messages.

1.  In the Azure Portal, select +Create a resource, Databases, then select Azure Cosmos DB.

    ![In the Azure Portal, New pane, both Databases and Azure Cosmos DB (Quickstart tutorial) are circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image42.png "Azure portal new databases")

2.  On the Azure Cosmos DB blade, enter the following:

    -   ID: Provide a **unique name** for the Azure Cosmos DB account (e.g., awhotelcosmosdb).

    -   API: Select **SQL**.

    -   Subscription: Choose the same subscription you used previously.

    -   Resource Group: Choose the **intelligent-analytics** resource group.

    -   Location: Choose the **same location** you used previously. If the region you've been using isn't available, select a different location for this resource.

    -   Enable geo-redundancy: Ensure this **box is Checked**.

    -   Select awhotelcosmosdb to provision the Azure Cosmos DB instance. 

        ![The Azure Cosmos DB blade fields display the previously mentioned settings. ](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image43.png "Azure Cosmos DB blade")

3.  When the provisioning completes, navigate to your new Azure Cosmos DB account in the portal.

4.  Select the Overview blade, then select **+Add Collection**. 

    ![In the Azure Portal, Azure Cosmos DB account blade, the Add Collection button is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image44.png "Azure Portal, Azure Cosmos DB account")

5.  On the Add Collection blade, enter the following:

    -   Database id: Enter **awhotels**.

    -   Collection Id: Enter **messagestore**.

    -   Storage Capacity: Select **Fixed (10 GB).**

    -   Throughput: Set to **1000**.

    -   Select OK to add the collection.

        ![The Add Collection blade fields display the previously mentioned settings. ](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image45.png "Add Collection blade")

### Task 7: Provision Azure Search

In this section, you will create an Azure Search instance.

1.  Select **+Create a resource, Web + Mobile**, the select **Azure Search**.

    ![In the Azure Portal, New pane, Web + Mobile and Azure Search (Learn More) are circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image46.png "Azure Search create a reource")

2.  On the New Search Service blade, enter the following:

    -   URL: Provide a **unique name** for the search service (e.g., conciergeplusapp).

    -   Subscription: Choose the subscription used previously

    -   Resource Group: Choose the **intelligent-analytics** resource group.

    -   Location: Choose the location used previously, or the next closest location if your location is unavailable in the list.

    -   Pricing Tier: Select **Basic**.

    -   Select **Create**.

        ![The New Search Service bladefields display the previously mentioned settings. ](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image47.png "New Search Service blade")

### Task 8: Create Stream Analytics job

In this section, you will create the Stream Analytics Job that will be used to read chat messages from the Event Hub and write them to the Azure Cosmos DB.

1.  Select **+Create a resource, Data + Analytics**, the select **Stream Analytics** **job**.

    ![In the Azure Portal, New pane, Data + Analytics and Stream Analytics job (Learn more) are circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image48.png "Azure create a stream analytics job")

2.  On the New Stream Analytics Job blade, enter the following:

    -   Job Name: Enter **MessageLogger**.

    -   Subscription: Choose the same subscription you have been using thus far.

    -   Resource Group: Choose the **intelligent-analytics** Resource Group.

    -   Location: Choose the **same Location** as you have for your other resources.

    -   Hosting environment: Select **Cloud**.

    -   Select **Create** to provision the new Stream Analytics job. 

        ![The New Stream Analytics Job blade fields display the previously mentioned settings. ](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image49.png "New Stream Analytics Job blade")

3.  When provisioning completes, navigate to your new Stream Analytics job in the portal by selecting Resource Groups in the left menu, and selecting intelligent-analytics, then selecting your Stream Analytics Job. 

    ![In the Azure Portal Resource Groups pane, under Name, the MessageLogger Stream Analytics job is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image50.png "Azure Portal Resource Groups pane")

4.  Select **Inputs** on the left-hand menu, under Job Topology.
    
    ![Under Job Topology, Inputs is selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image51.png "Job Topology section")

5.  On the Inputs blade, select **+Add stream input** and then click **Event Hub**.

    ![The Add Stream Input is shown, and the Event Hub has been selected from the options.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image52.png "Add stream input")

6.  On the New Input blade, enter the following:

    -   Input Alias: Set the value to **eventhub**.

    -   Choose: **Select Event Hub from your subscriptions**

    -   Subscription: Choose the same subscription you have been using thus far.

    -   Event Hub namespace: Choose the Namespace which contains **your Event Hubs instance** (e.g., awhotelevents-namespace).

    -   Event hub name: Choose the second Event Hub instance you created (**awchathub2**).

    -   Service bus namespace:

    -   Event hub policy name: Leave as **RootManageSharedAccessKey.**

    -   Event hub consumer group: Leave this **blank** (\$Default consumer group will be used).

    -   Event serialization format: Leave as **JSON**.

    -   Encoding: Leave as **UTF-8**.

    -   Event compression type: Leave set to **None**.

    -   Select **Save**.

        ![The Event Hub New input blade fields display the previously mentioned settings. ](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image53.png "Event Hub New input")

7.  Now, select **Outputs** from the left-hand menu, under Job Topology.

    ![Under Job Topology, Outputs is selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image54.png "Job Topology section")

8.  In the Outputs blade, click **+Add**, then click **Cosmos DB.**

    ![The Add New Outputs is shown with the CosmosDB option selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image55.png "Add New Outputs")

9.  On the Cosmos DB New output blade, enter the following:

    -   Output alias: Enter **cosmosdb**.

    -   Import Option: Leave set to Select Cosmos DB from your subscriptions.

    -   Subscription: Choose the same subscription you have been using thus far.

    -   Account Id: Select your Account id (e.g., awhotel-cosmosdb).

    -   Database: Select your database, **awhotels**.

    -   Collection name pattern: Set to the name of your single collection, **messagestore**.

    -   Document Id: Set to **messageid** (all lowercase).

    -   Select **Save**.

        ![The CosmosDB New Output blade fields display the previously mentioned settings. ](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image56.png "CosmosDB New Output")

10. Create another Output, this time for Power BI.

    ![The Add New Outputs is shown with the Power BI option selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image57.png "Add New Outputs")

11. On the New output blade, enter the following:

    -   Output alias: Enter **powerbi**

    -   Group workspace: **Authorize connection to load workspaces**.

    -   Dataset Name: Set to **Messages**

    -   Table Name: Set to **Messages**

    -   Select **Authorize**. This will authorize the connection to your Power BI account. When prompted in the popup window, enter the account credentials you used to create your Power BI account in [Before the Hands-on Lab, Task 1](#task-1-provision-power-bi). You may have to enter your Username and Password.

        ![The Power BI New output screen is shown configured. The Authorize connection has been clicked.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image58.png "POwer BI new output")

12. Select **Save**.

13. Next, select Query from the left-hand menu, under Job Topology.

    ![Under Job Topology, Query is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image59.png "Job Topology section")

14. In the query text box, enter the following query for Cosmos DB:
    ```
    SELECT
    *
    INTO
    cosmosdb
    FROM
    eventhub
    ```

15. Select Save, and Yes when prompted with the confirmation.

    ![Select save option.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image60.png "Save option")

16. Now, modify your query to add the following Power BI query. Enter or paste the following code below the first query:
    ```
    SELECT
    *
    INTO
    powerbi
    FROM
    eventhub
    ```

17. Your query text should now look like the following: 

    ![Screenshot of the Power BI Query text. At this time, we are unable to capture all of the information in the text window. Future versions of this course should address this.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image61.png "Power BI Query text window")

18. Select Save again, and select Yes when prompted with the confirmation.
    
    ![The Save button is selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image60.png "Save button")

### Task 9: Start the Stream Analytics Job

1.  Navigate to your Stream Analytics job in the portal by selecting Resource Groups in the left menu, and selecting intelligent-analytics, then selecting your Stream Analytics Job. 

    ![In the Azure Portal, in the Resource Groups pane, under Name, the MessageLogger Stream Analytics Job is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image50.png "Azure Portal, Resource Groups pane")

2.  From the Overview blade, select **Start**.
    
    ![The Now button is selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image62.png "Now button")

3.  In the Start job blade, select **Now** (the job will start processing messages from the current point in time onward).

    ![For Job output start time, the New button is selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image63.png "Start job blade, Now button")

4.  Select **Start**.

5.  Allow your Stream Analytics Job a few minutes to start. Once the Job starts it will move to a state of Running.

    ![The Stream Analytics job is shown in the Azrue portal with a state of Running.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image64.png "Stream analytics job running")

### Task 10: Provision an Azure Storage Account

The EventProcessorHost requires an Azure Storage Account that it will use to manage its state among multiple instances. In this section, you create that Storage Account.

1.  Using the Azure Portal, select **+Create a resource, Storage, the select Storage account -- blob, file, table, queue.**

    ![In the Azure Portal, in the New pane, Storage, and Storage account - blob, file, table queue (Quickstart tutorial) are circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image65.png "Azure new storage account")

2.  In the Create storage account blade, enter the following:

    -   Name: Provide a **unique name** for the account (e.g., awhotelchatstore).

    -   Deployment model: Leave **Resource Manager** selected.

    -   Account kind: Leave **General purpose** selected.

    -   Performance: Set to **Standard**.

    -   Replication: Set to **Locally Redundant Storage (LRS)**.

    -   Secure transfer required: Select **Disabled**.

    -   Subscription: Choose the Subscription used previously.

    -   Resource Group: Choose the **intelligent-analytics** resource group.

    -   Location: Choose the location used previously.

    -   Configure virtual networks: Leave set to **Disabled**.

    -   Select **Create**.

        ![The Create storage account blade fields display the previously mentioned settings. ](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image66.png "Create storage account blade")

### Task 11: Provision Cognitive Services

To provision access to the Text Analytics API (which provides sentiment analysis features), you will need to provision a Cognitive Services account.

1.  In the Azure Portal, select +Create a resource, then AI + Cognitive Services, Text Analytics API.

    ![The New Azure Resource menu is shown, after clicking AI + Congnitive Services and then Text Analytics API.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image67.png "New Azure resource AI + Cognitive Services")

2.  On the Create blade, enter the following:

    -   Name: Enter **awhotels-sentiment**.

    -   Subscription: Choose the subscription used previously.

    -   Location: Select the location you used previously.

    -   Pricing tier: Choose **F0**

    -   Resource group: Choose the **intelligent-analytics** resource group.

    -   Check the box to confirm you have read and understood the notice.

        ![The Create Text Analytics API blade is shown after completing the configurations.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image68.png "Create Text Analytics")

3.  Select **Create**.

4.  When it finishes provisioning, browse to the newly created cognitive service by selecting Resource Groups in the left menu, then select the **intelligent-analytics** resource group, and selecting the Cognitive Service, **awhotels-sentiment**.

5.  Acquire the key for the API by selecting Keys on the left-hand menu.

    ![Under Resource Management, Keys is selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image69.png "Resource Management section")

6.  Copy the value for Key 1, and paste it into a text editor, such as Notepad, for later reference in the ConciergePlusSentiment solution in Visual Studio. 

    ![In the Keys pane, the Key 1 value is circled, and a callout points to the copy button for this key.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image70.png "Keys pane")

7.  Repeat steps 1-7, this time selecting Bing Speech API.

    -   Enter the name speech-api.

    -   Take note of Key 1 for Speech.

        ![Bing Speeh API from the Azure Marketplace is shown. Locate and click to create this resource.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image71.png "Bing Speech API")

8.  Repeat steps 1-7, this time selecting Language Understanding Intelligent Service (LUIS) for the API Type on the Create blade.

    -   Enter the name **luis-api.**

    -   Take note of Key 1 for LUIS.

        ![Language Understanding from the Azure Marketplace is shown. Locate and click to create this resource.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image72.png "Language Understanding")

9.  Verify that you have captured all three of the API keys for later reference in this lab.

    ![The API keys for all three of the Cognitive Service APIs have been captured and are shown in notepad.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image73.png "API text keys")

## Exercise 2: Implement message forwarding

Duration: 45 minutes

In this section, you will implement the message forwarding from the ingest Event Hub instance to an Event Hub instance and a Service Bus Topic. You will also configure the web-based components, which consist of three parts: The Web App UI, a Web Job that runs the EventProcessorHost, and the API App that provides a wrapper around the Search API.

### Task 1: Implement the event processor

1.  On your Lab VM, open the ConciergePlusSentiment.sln file that you downloaded using Visual Studio, if it is not already open.

2.  Open **SentimentEventProcessor.cs** (found within the **ChatMessageSentimentProcessor** project in the Solution Explorer).

    ![Visual Studio is expanded as follows: ChatMessageSentimentProcessor\\SentimentEventProcessor.cs](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image74.png "Visual Studio")

3.  Scroll down to the IEventProcessor.ProcessEventsAsync method. This method represents the heart of the message processing logic utilized by the Event Processor Host running in a Web Job. It is provided a collection of EventData instances, each of which represent a chat message in the solution. 

    ![Screen capture of the I event proccessor . process events async method.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image75.png "I event processor")

4.  Locate TODO: 1 and replace the lines that follow the comment with the following:
    ```
    //TODO: 1.Extract the JSON payload from the binary message
    var eventBytes = eventData.GetBytes();
    var jsonMessage = Encoding.UTF8.GetString(eventBytes);
    Console.WriteLine("Message Received. Partition '{0}', SessionID '{1}' Data '{2}'", context.Lease.PartitionId, eventData.Properties["SessionId"], jsonMessage);
    ```

5.  Locate TODO: 2 and replace the line that follows with:
    ```
    //TODO: 2.Deserialize the JSON message payload into an instance of MessageType
    var msgObj = JsonConvert.DeserializeObject<MessageType>(jsonMessage);
    ```

6.  Locate TODO: 3 and replace the line that follows with:
    ```
    //TODO: 3. Create a BrokeredMessage (for Service Bus) and EventData instance (for EventHubs) from source message body
    var updatedEventBytes = Encoding.UTF8.GetBytes(JsonConvert.SerializeObject(msgObj));
    BrokeredMessage chatMessage = new BrokeredMessage(updatedEventBytes);
    EventData updatedEventData = new EventData(updatedEventBytes);
    ```

7.  Locate TODO: 4 and replace the lines that follow with:
    ```
    //TODO: 4.Copy the message properties from source to the outgoing message instances
    foreach (var prop in eventData.Properties)
    {
    chatMessage.Properties.Add(prop.Key, prop.Value);
    updatedEventData.Properties.Add(prop.Key, prop.Value);
    }
    ```

8.  Locate TODO: 5 and replace the line that follows with:
    ```
    //TODO: 5.Send chat message to Topic
    _topicClient.Send(chatMessage);
    Console.WriteLine("Forwarded message to topic.");
    ```

9.  Locate TODO: 6 and replace the line that follows with:
    ```
    //TODO: 6.Send chat message to next EventHub (for archival)
    _eventHubClient.Send(updatedEventData);
    Console.WriteLine("Forwarded message to event hub.");
    ```

10. Save the file, but clicking the Save button on the Visual Studio toolbar. 

    ![On the Visual Studio toolbar, the Save button is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image76.png "Visual Studio toolbar")

### Task 2: Configure the Chat Message Processor Web Job

1.  Within Visual Studio Solution Explorer, expand the **ChatMessageSentimentProcessor** project, and open **App.Config**.

    ![Visual Studio is expanded as follows: ChatMessageSentimentProcessor\\App.config](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image77.png "Visual Studio")

2.  You will update the appSettings in this file. The following sections walk you through the process of retrieving the values for the following settings:
    ```
      <add key="eventHubConnectionString" value="" />
      <add key="sourceEventHubName" value="" />
      <add key="destinationEventHubName" value="" />
      <add key="storageAccountName" value="" />
      <add key="storageAccountKey" value="" />
      <add key="serviceBusConnectionString" value="" />
      <add key="chatTopicPath" value="" /> 
      <add key="textAnalyticsAccountName" value="" />
      <add key="textAnalyticsAccountKey" value="" />
    ```

#### Event Hub connection string 

The connection string required by the ChatMessageSentimentProcessor is different from the typical Event Hub consumer, because not only does it need Listen permissions, but it also needs Send and Manage permissions on the Service Bus Namespace (because it receives messages, as well as creates Subscriptions).

1.  To get the eventHubConnectionString, navigate to the Event Hub namespace in the Azure Portal by selecting Resource Groups on the left menu, then selecting the intelligent-analytics resource group, and selecting your Event Hub from the list of resources. 

    ![In the Name section of the Resource Groups pane, the awhotel-events-namespace Event Hub is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image39.png "Azure Portal, Resource Groups pane")

2.  Select Shared access policies, under Settings, within the left-hand menu.

3.  In the Shared access policies, you are going to create a new policy that the ChatConsole can use to retrieve messages. Click **+Add**. 

    ![The Add button is circled in the Shared access policies pane.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image78.png "Shared access policies pane")

4.  For the New Policy Name, enter ChatConsole.

5.  In the list of Claims, select Manage. Send and Listen will be automatically selected when you select Manage.

    ![In the Add SAS Policy dialog box, Policy name is set to ChatConsole. Three check boxes are selected for Manage, Send, and Listen.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image79.png "Add SAS Policy dialog box")

6.  After the **ChatConsole** shared access policy is created, select it from the list of policies, and then copy the Connection string--primary key value. 

    ![Two panes display: Shared access policies, and SAS Policy: Chat Console. In the Shared access policies pane, ChatConsole is selected. In the SAS Policy: ChatConsole pane, the Connection string-primary key is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image80.png "Shared access policies, and SAS Policy: Chat Console panes")

7.  Return to the **app.config** file in Visual Studio, and paste this as the **value** for **eventHubConnectionString**.

#### Event Hub name

Your event hubs can be found by going to your Event Hub overview blade, and selecting Event Hubs from the left menu.

![Event hubs overview blade](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image81.png "Event hubs")

1.  For the **sourceEventHubName** setting in **app.config**, enter the name of your first Event Hub, **awchathub**.

2.  For the **destinationEventHubName**, enter the name of your second Event Hub, **awchathub2**.

#### Storage account

Your storage accounts can be found by going to the intelligent-analytics resource group, and selecting the Storage account.

1.  For the **storageAccountName** enter the name of the storage account you created.

2.  For the **storageAccountKey** enter the Key for the storage account you created (which you can retrieve from the Portal).

    -   From your storage account's blade, select Access Keys from the left menu, under Settings.

        ![Under Settings, Access Keys is circled](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image82.png "Settings section")

    -   Copy the Key value for key1, and paste that into the value for storageAccountKey in the App.Config file. 
        ![In the Access Keys section, the value for Key1 and its copy button are circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image83.png "Access Keys section")

#### Service Bus connection String 

The namespace, and therefore connection string, for the service bus is different from the one for the event hub. As we did for the event hub, we need to create a shared access policy to allow the ChatMessageSentimentProcessor Manage, Send, and Listen permissions.

1.  To get the **serviceBusConnectionString**, navigate to the **Service Bus namespace** in the Azure Portal.

2.  Select **Shared access policies** within the left menu, under Settings.

    ![Under Settings, Shared access policies is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image84.png "Settings section")

3.  In the Shared access policies, you are going to create a new policy that the ChatConsole can use to retrieve messages. Click +Add.

    ![The Azure Portal is shown with the Shared Access polices blade of the Service Bus Namespace open. Add is being selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image85.png "Azure Shared Access policies blade")

4.  For the New Policy Name, enter ChatConsole.

5.  In the list of Claims, select Manage, Send, and Listen.
    
    ![In the New policy section, Policy name is set to ChatConsole, and three check boxes are selected: Manage, Send, and Listen.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image86.png "New policy section")

6.  Select Create.
    
    ![Screenshot of the Create button.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image87.png "Create button")

7.  After the **ChatConsole** shared access policy is created, select it from the list of policies andcopy the Primary Connection String value.

    ![In the SAS Policy: ChatConsole blade, the Primary Connection String value and its corresponding copy button are circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image88.png "SAS Policy: ChatConsole blade")

8.  Return to the **app.config** and paste this as the value for **serviceBusConnectionString**.

#### Chat topic 

1.  For the **chatTopicPath**, enter the name of the Service Bus Topic you had created (e.g., awhotel). This can be found under Topics on the Service Bus overview blade.

    ![Service bus entities overview blade with topics selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image89.png "Service bus entities overview blade")

#### Text Analytics API settings

1.  Using the Azure Portal, open the Text API (awhotels-sentiment), copy the value under Endpoint into the **textAnalyticsBaseUrl** setting. Be sure to include a trailing slash in the URL (e.g. <https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/)>. 

    ![In the Cognitive Service account blade, the Endpoint value is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image90.png "Cognitive Service account blade")

2.  On the left-hand menu of the Text APInt blade, select Keys. 

    ![On the Cognitive Service account blade, the Name value (awhotels-sentiment) and its copy button are circled, as is the Key 1 value and its copy button.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image91.png "Cognitive Service account blade")

3.  Copy the value of **Account Name** into the value attribute of **textAnalyticsAccountName** in the **app.config**.

4.  Copy the value of **Key 1** from the blade into the value attribute of the **textAnalyticsAccountKey** in the **app.config**.

5.  Save the app.config file. It should now resemble the following:

    ![A sample of the type of code from the app.config is shown.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image92.png "App.config code")

## Exercise 3: Configure the Chat Web App settings

Duration: 10 minutes

Within Visual Studio Solution Explorer, expand the **ChatWebApp** project and open **Web.Config**. You will update the in this file. The following sections walk you through the process of retrieving the values for the following settings:
    ```
    <add key="eventHubConnectionString" value=" "/>
    <add key="eventHubName" value=" "/>
    <add key="serviceBusConnectionString" value=" "/>
    <add key="chatRequestTopicPath" value=" "/>
    <add key="chatTopicPath" value=" "/>
    ```

### Task 1: Event Hub connection String 

1.  Use the same connection string you used for the **eventHubConnectionString** in the **App.Config** file of the **ChatMessageSentimentProcess** Web Job project.

### Task 2: Event Hub name

1.  For the **eventHubName** setting in **Web.config**, enter the name of your first Event Hub (**awchathub**). This event Hub will receive messages from the website chat clients.

### Task 3: Service Bus connection String 

1.  Use the same connection string you used for the **serviceBusConnectionString** in the **app.Config** file of the **ChatMessageSentimentProcess** Web Job project.

### Task 4: Chat topic path and chat request topic path

1.  For the **chatRequestTopicPath** and the **chatTopicPath**, enter the name of the Service Bus Topic you created, **awhotel**. The value is the same for both settings in this case.

2.  The **web.config** should resemble the following. Click Save in Visual Studio.

    ![A web.config code window displays. ](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image93.png "web.config code window ")

## Exercise 4: Deploying the App Services

Duration: 15 minutes

With the App Services projects properly configured, you are now ready to deploy them to their pre-created services in Azure.

### Task 1: Publish the ChatMessageSentimentProcessor Web Job

1.  Within Visual Studio Solution Explorer, right-click the **ChatMessageSentimentProcessor** project in the Solution Explorer, and select **Publish as Azure Web Job**.

    ![In Solution Explorer, the sub-menu for ChatMessageSentimentProcessor displays, with Publish as Azure WebJob selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image94.png "Solution Explorer")

2.  In the Publish dialog, select Microsoft Azure App Service as the publish target.
    
    ![In the Publish dialog box, Select a publish target is set to Microsoft Azure App Service.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image95.png "Publish dialog box")

3.  In the App Service dialog, choose the Subscription that contains your Web Job Web App you provisioned earlier. Expand your Resource Group (e.g., **intelligent-analytics**), then select the node for your Web Job Web App in the tree view to select it. 

    ![In the App Service dialog box, the tree view is expanded to: intelligent-analytics\\cpchatprocessorwebjob.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image96.png "App Service dialog box")

4.  Select **OK**.

5.  Select Publish. 

    ![In the Publish dialog box, the Publish button is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image97.png "Publish dialog box")

6.  When the publish completes, the Output window should indicate success similar to the following:
    
    ![The Output window is set to show output from Build. Output indicates it is adding ACL\'s for ConciergePlusProcessorWebJob, and that Publish Succeeded.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image98.png "Output window")
    
**Note**: If you receive an error in the Output window, as a result of the publish process failing (The target \"MSDeployPublish\" does not exist in the project), you need to update the Microsoft.Web.WebJobs.Publish NuGet package. To do this, click on Tools NuGet Package Manager Package Manager Console.
    
![NuGet package Manager and Package Manager Console are selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image99.png "NuGet Package Manager")

7.  In the Package Manager Console, type the following and hit Enter:
    ```
    Update-Package Microsoft.Web.WebJobs.Publish -Reinstall
    ```

**Note**: If prompted to replace a Swagger file, enter N, and press Enter.

8.  Repeat steps 1-5 to publish.

### Task 2: Publish the ChatWebApp

1.  Within Visual Studio Solution Explorer, right-click the ChatWebApp project and select Publish.
    
    ![In the Visual Studio Solution Explorer ChatWebApp sub-menu, Publish is selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image100.png "Visual Studio Solution Explorer")

2.  In the Publish document, select **Microsoft Azure App Service**, and choose the **Select** existing radio button. Select **Publish**.
    
    ![In the Publish window, the Microsoft Azure App Service option is selected, as is the Select Existing radio button.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image101.png "Publish window")\
    
    You may see a different dialog than what is shown above. If so, select Microsoft Azure App Service:
    
    ![In the Publish window, under Select a publish target, Microsoft Azure App Service is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image102.png "Publish window")

3.  In the App Service dialog, choose your Subscription that contains your Web App you provisioned earlier. Expand your Resource Group (e.g., **intelligent-analytics**), then select the node for your **Web App** in the tree view to select it.

    ![In the App Service dialog box, the tree view is expanded as follows: awchat\\conciergeplus.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image103.png "App Service dialog box")

4.  Select **OK**.

5.  When the publishing is complete, a browser window should appear with content like the following.
    
    ![The Browser window displays the Contoso Hotels webpage, with a Join Chat window open below.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image104.png "Browser window")

### Task 3: Testing hotel lobby chat

1.  Open a browser instance (Chrome is recommended for this web app), and navigate to the deployment URL for your Web App.

    -   If you are unsure what this URL is, it can be found in two places:

        i.  First, you can find it on the ChatWebApp document in Visual Studio, that was opened when you published the Web App. ![In the Visual Studio ChatWebbApp tab, under Summary, the Site URL.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image105.png "Visual Studio ChatWebbApp tab")

        ii. Alternatively, this can be found in the Azure Portal on the Overview blade for your Web App ![In the Essentials section of the Overview blade, the deployment URL is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image106.png "Azure Portal Overview blade, Essentials section")

2.  Under the Join Chat area, enter your username (anything will do).

3.  Leave Hotel Lobby selected.

1.  Select **Join**.
    
    ![The Join Chat window displays.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image107.png "Join Chat window")

5.  The Live Chat should appear. (Notice it auto-announced you joining to the room; this is the first message. Note, this may take a few seconds to appear.)

    ![The Live Chat window displays, showing that it is connected to the chat service.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image108.png "Live Chat window")

6.  Open another browser instance. (You could try this from your mobile device.)

7.  Enter another username, and Select Join.

8.  From either session, fill in the Chat text box and select Send. You can try using @ and \# too, just to seed some text for search.

    ![The Live Chat window shows a chat going on between two users.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image109.png "Live Chat window")

9.  You can join with as many sessions as you want. (The Hotel Lobby is basically a public chat room.)

## Exercise 5: Add intelligence

Duration: 60 minutes

In this exercise, you will implement code to activate multiple cognitive intelligence services that act on the chat messages.

### Task 1: Implement sentiment analysis

In this task, you will add code that enables the Event Processor to invoke the Text Analytics API using the REST API and retrieve a sentiment score (a value between 0.0, negative, and 1.0, positive sentiment) for the text of a chat message.

1.  In the Solution Explorer in Visual Studio, open **SentimentEventProcessor.cs** in **ChatMessageSentimentProcessor** project.

2.  Scroll down to the method **GetSentimentScore**.

3.  Replace the code following TODO: 7 with the following:
    ```
    //TODO: 7.Configure the HTTPClient base URL and request headers
    client.BaseAddress = new Uri(_textAnalyticsBaseUrl);
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", _textAnalyticsAccountKey);
    client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    ```

4.  Replace the code following TODO: 8 with the following:
    ```
    //TODO: 8.Construct a sentiment request object
    var req = new SentimentRequest()
    {
    documents = new SentimentDocument[]
    {
    new SentimentDocument() { id = "1", text = messageText }
    }
    };
    ```

5.  Replace the code following TODO: 9 with the following:
    ```
    //TODO: 9.Serialize the request object to a JSON encoded in a byte array
    var jsonReq = JsonConvert.SerializeObject(req);
    byte[] byteData = Encoding.UTF8.GetBytes(jsonReq);
    ```

6.  Replace the code following TODO: 10 with the following:
    ```
    //TODO: 10.Post the rquest to the /sentiment endpoint
    string uri = "sentiment";
    string jsonResponse = "";
    using (var content = new ByteArrayContent(byteData))
    {
    content.Headers.ContentType = new MediaTypeHeaderValue("application/json");
    var sentimentResponse = await client.PostAsync(uri, content);
    jsonResponse = await sentimentResponse.Content.ReadAsStringAsync();
    }
    Console.WriteLine("\nDetect sentiment response:\n" + jsonResponse);
    ```

7.  Replace the code following TODO: 11 with the following:
    ```
    //TODO: 11.Deserialize sentiment response and extract the score
    var result = JsonConvert.DeserializeObject<SentimentResponse>(jsonResponse);
    sentimentScore = result.documents[0].score;
    ```

8.  Finally, navigate to the IEventProcessor.ProcessEventsAsync and replace the line following TODO: 12 with the following code:
    ```
    //TODO: 12 Append sentiment score to chat message object
    msgObj.score = await GetSentimentScore(msgObj.message);
    ```

9.  Save the file.

### Task 2: Implement linguistic understanding

In this task, you will create a LUIS app, publish it, and then enable the Event Processor to invoke LUIS using the REST API.

1.  Using a browser, navigate to <http://www.luis.ai>.

2.  Select **Sign in** **or create an account**. 

    ![The Language Understanding Intelligent Service webpage with a Sign in or create an account button displays.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image110.png "Language Understanding Intelligent Service webpage")

3.  Sign in using your Microsoft account (or \@Microsoft.com account if that is appropriate to you). The new account startup process may take a few minutes.

4.  Click **Accept**.

    ![The Accept button is clicked to agree to the LUIS Service App being connected to your account.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image111.png "Accept button")

5.  You should be redirected to the LUIS Welcome page at <https://www.luis.ai/welcome>. Scroll down and click **Create LUIS app.**

    ![The Create LUIS app botton is clicked form the Welcome page of the LUIS AI Service.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image112.png "Create LUIS app")

6.  Complete the additional info and terms of use form and select **Continue**.
    
    ![The Welcome to Language understanding webpage displays.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image113.png "Welcome to Language")

7.  Under My Apps, select **Create New App**.

8.  Complete the Create a new app form by providing a name for your LUIS app, the culture and select **Done**.

    ![In the Create a new app dialog box, the Name field is set to awchat, and Culture is set to English.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image114.png "Create new app")

9.  In a moment, your new app will appear. Click the app to see the details.

10. In the menu bar, select **Publish**, select the appropriate region, and select Add Key.

    ![The Publish dialog is shown with the region selected and the Add Key button clicked.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image115.png "Publish dialog")

11. In the Assign a key to your app dialog, select your Tenant name and Subscription, and then select the luis-api key from the list.

    ![The Assign a key to your app dialog box displays.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image116.png "Assign a key to your app dialog box")

12. Select **Add Key**.

13. Select My Apps from the menu bar and choose your app from the list.

14. Select **Intents** on the left-hand menu, then **Create new intent**.

    ![The Intents menu for the awchat application and the Create new intent has been selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image117.png "awchat application")

15. In the Intents dialog, for the Intent Name enter **OrderIn** and select **Done**.
    
    ![The OrderIn intent has been entered into the Create new intent diaglog and the Done button is clicked.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image118.png "Create a new intent")

16. In the Utterances text box, enter "order a pizza". Press Enter to add the utterance.
    
    ![The utterence order a pizza has been typed into the OrderIn Intent.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image119.png "Orderin utterance")

17. Select Entities from the menu on the left.
    
    ![The Entities menu item has been selected for the awchat application.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image120.png "Entities menu")

18. Select Create new entity.

    ![The Create new entity has been clicked for the awchat application.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image121.png "Create new entity")

19. For the Entity name specify "**RoomService**" and set the Entity Type to **Hierarchical**.
    
    ![In the Add Entity dialog box, Entity name is set to RoomService, and Entity type is set to Hierarchical.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image122.png "Add Entity dialog box")

20. Select **+ Add child entity**.

21. For Child Name provide a name of **FoodItem**.
    
    ![A child entity has been added to the RoomService entity.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image123.png "Child name entity")

22. Select +Add a child entity and provide a name of **RoomItem**. Select **Done**.

    ![The completed Entity name is shown with all of the selections completed. The done button has been selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image124.png "Completed entity name")

23. Select **Intents** from the menu on the left and select the **OrderIn** intent you created.

24. In the utterance, select the word pizza so it becomes highlighted.

    ![In the Utterances (1) section, order a \[pizza\] is selected, and below that, RoomService displays with a chevron next to it.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image125.png "Utterances order a pizza")

25. Under Entities select **RoomService**, then select **FoodItem**.

    ![The expanded chevron displays, with two choices: RoomService::FoodItem, which is selected, and RoomService::RoomItem, which is not.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image126.png "Entities options")

26. In the Type a new utterance text box, enter the following utterance:

    -   Utterance: **bring me toothpaste**

    -   Text to select: **toothpaste**

    -   Drop-down: **OrderIn**

    -   Entity: **RoomService:RoomItem**

27. Repeat this process for the following phrases (text to select is in bold):

    -   **Bring me towels \| RoomService:RoomItem**

    -   **Bring me blankets \| RoomService:RoomItem**

    -   **Order a soda \| RoomService:FoodItem**

    -   **Order me a hamburger \| RoomService:FoodItem**

        ![The utterences after they have been entered and aligned to the proper entities.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image127.png "Utterance list of room service items")

28. Select Train from the menu bar.

    ![Train menu bar selected/](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image128.png "Train menu bar")

29. Click Test and experiment with the by writing some utterances and pressing enter to see the interpretation.
    
    ![An interactive test showing that searching for where can i buy a hamburger will invoke the entity RoomService::FoodItem. ](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image129.png "Test of utterances")

30. Select Publish App from the menu on the top. Then select the proper Timezone, and select Publish to production slot.

    ![The Publish app screen is showing that they timezone has been selected and the Publish to production slot has been selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image130.png "Publish app")

31. When the publish completes, click the URL displayed next to the **luis-api** key at the bottom of the Publish App screen.

    ![At the bottom of the Publish App dialog box, the luis-api Endpoint URL is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image131.png "Publish App dialog box")

32. This will open a new tab in your browser. Modify the end of the URL (the text following q= ) so it contains the phrase "order a pizza," and press ENTER. You should receive output similar to the following. Observe that it correctly identified the intent as OrderIn (in this case with a confidence of 0.9999995 or nearly 100%) and the entity as pizza having an entity type of RoomService::FoodItem (in this case with a confidence score of 96.1%)
    
    ![The code window displays code as previously described.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image132.png "Code window")

33. In the URL, take note of two things:

    -   The base URL, *highlighted in green*. Copy this value, and paste it into a text editor, such as Notepad, for later reference.

    -   The GUID following apps/GUID/, *highlighted in yellow*. This is your App ID and you will need to use it in configuration later, it looks like the following:
        https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/e49117d8-0275-4319-8fed-698ff6dc8192?subscription-key=08a82755f5f040ae9b4376ccab5fa6bc&verbose=true&timezoneOffset=-300&q=

34. You can add more utterances as desired by repeating the above steps to add new utterances, indicate the entity, train the model, and then update the publish application using the button in the Publish App screen.

35. When you are ready to integrate LUIS into your app, go to the Publish App screen, and locate the luis-api key under Resources and Keys. Copy the first Key String for luis-api. Copy this to your notepad.

    ![The luis-api key in the Publish App screen is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image133.png "Reserouces and keys add key")

**Note**: This is the same key you can obtain on the Keys blade for the luis-api Cognitive Service in the Azure portal.

36. You will enter this into the configuration of the Event Processor.

37. In Visual Studio, open the **App.config** for the **ChatMessageSentimentProcessor** project.

38. Within the appSettings section, for the key **luisAppId** set the text of the value attribute to the App ID of your LUIS App (this value should be a GUID you obtained from the URL and not the name of your LUIS app). For the key **luisKey**, set the text of the value attribute to the Endpoint key used by your LUIS app (as you acquired it from the Azure Portal).
    
    ![In Visual Studio, the following code displays: \<add key=\"luisAppId\" value=\"\" /\> \<add key=\"luisKey\" value=\"\" /\> ](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image134.png "Visual Studio")

39. Save the **app.config**. The Event Processor is pre-configured to invoke the LUIS API using the provided App ID and key.

40. Open SentimentEventProcessor.cs and navigate to IEventProcessor.ProcessEventsAsync.

41. Locate TODO: 13 and replace it with the following:
    ```
    //TODO: 13.Respond to chat message intent if appropriate
    var intent = await GetIntentAndEntities(msgObj.message);
    HandleIntent(intent, msgObj);
    ```

42. At the top of the file, locate the variable named \_luisBaseUrl. 

    ![In Visual Studio, the URL for luisBaseURL is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image135.png "Visual Studio")

43. You will replace the value of this variable with the base URL you copied from the LUIS site above (the part highlighted in green).

44. Take a look at the implementation of both methods if you are curious how the entity and intent information is used to generate an automatic chat message response from a bot.

45. Save the file.

### Task 3: Implement speech to text

There is one last intelligence service to activate in the application---speech recognition. This is powered by the Bing Speech API, and is invoked directly from the web page without going through the web server. In the steps that follow, you insert your Cognitive Services Speech API key into the configuration to enable speech to text.

1.  Within Visual Studio Solution Explorer, expand **ChatWebApp**, **Scripts**, and open **chatClient.js**.

2.  At the top, locate the variable **speechApiKey**, and update its value with the Key 1 you acquired in [Exercise 1, Task 11, Step 8](#task-11-provision-cognitive-services), when you provisioned your Speech API in the Azure Portal.
    
    ![The following variable code displays: //TODO: Enter your Speech API Key here var speechApiKey = \"\";](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image136.png "variable")

3.  Save chatClient.js.

**Note**: Embedding the API Key as shown here is done only for convenience. In a production app, you will want to maintain your API Key server-side.

### Task 4: Re-deploy and test

Now that you have added sentiment analysis, language understanding, and speech recognition to the solution, you need to re-deploy the apps so you can test out the new functionality.

1.  Publish the **ChatMessageSentimentProcessor** Web Job using Visual Studio just as you did in [Exercise 4, Task 1](#task-1-publish-the-chatmessagesentimentprocessor-web-job).

2.  Publish the **ChatWebApp** just as you did in [Exercise 4, Task 2](#task-2-publish-the-chatwebapp).

3.  When both have published, navigate to your deployed web app making sure to use HTTPS. (This is required for most browsers to support the microphone needed for speech recognition.)

4.  Join a chat with the Hotel Lobby.

5.  Type a message with a positive sentiment, like "I love this weather." Observe the "thumbs-up" icon that appears next to the chat message you sent. Next, types something like, "This weather is terrible," and observe the thumbs-down icon. These are indicators of sentiment (as applied by your solution in real-time). \`\`

    ![In the Live Chat window, callouts point to the thumbs-up and thumbs-down icons.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image137.png "Live Chat window")

6.  Next, try ordering some items from room service, like "bring me towels" and "order a pizza." Observe that you get a response from the ConciergeBot, and that the reply indicates whether your request was sent to Housekeeping or Room Service, depending on whether the item ordered was a room or food item. 

    ![In the chat window, Kyle is having a conversation with a ConciergeBot. At first he asks for towels, and the ConciergeBot says they are forwarding the request to Housekeeping. Then Kyle wants to order a pizza, and ConciergeBot says they are forwarding his request to Room Service.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image138.png "Live Chat window")

7.  Finally, instead of typing your text, select the microphone to the left of the text box and speak for 2 to 3 seconds. Your spoken message should appear. Select the paper airplane icon to send it. 

    ![In the Live Chat window, a callout arrow points to the microphone icon.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image139.png "Live Chat window")

## Exercise 6: Building the Power BI dashboard

Duration: 30 minutes

Now that you have the solution deployed and exchanging messages, you can build a Power BI dashboard that monitors the sentiments of the messages being exchanged in real time. The following steps walk through the creation of the dashboard.

### Task 1: Create the static dashboard

1.  Sign in to your Power BI subscription (<https://app.powerbi.com>).

2.  Select My Workspace on the left-hand menu, the select the Datasets tab. 

    ![In the Power BI window, on the left menu, My Workspace is circled. In the right pane, Datasets is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image140.png "Power BI window")

3.  Under the **Datasets** list, select the **Messages** dataset. Search for the Messages dataset, if there a too many items in the dataset list. 

    ![On the Datasets tab, under Name, Messages is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image141.png "Datasets tab")

4.  Select the **Create Report** button under the Actions column. 

    ![On the Datasets tab, under Actions, the Create Report button is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image142.png "Datasets tab")

5.  On the Visualizations palette, select **Gauge** to create a semi-circular gauge.
    
    ![On the Visualizations palette, the Gauge (donut) icon is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image143.png "Visualizations palette")

6.  In the Fields listing, select and drag the **score** field and drop it onto the **Value** field.
    
    ![The Visualizations and Fields listings display. In the Fields listing, under Messages, the ??? score check box is selected. An arrow points from this to the Value field in the Visualizations listing, where score is now listed.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image144.png "Visualizations and Fields listings")

7.  Select the drop-down menu that appears where you dropped score and select **Average**.
    
    ![Average is selected and a green checkmark displays next to it on the Drop-down menu.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image145.png "Drop-down menu")

8.  You now should have a gauge that shows the average sentiment for all the data collected so far, which should look similar to the following:
    
    ![A semi-circle gauge graph displays for Average of score, which is 0.62.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image146.png "Gauge graph")

9.  From the File menu, select Save to save your visualization to a new report.

    ![On the File menu, Save (Save this report) is selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image147.png "File menu")

10. Enter **ChatSentiment** for the report name, and select **Save**. 

    ![In the Save your report window, ChatSentiment is typed in as the name of the report.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image148.png "Save your report window")

### Task 2: Create the real-time dashboard

This gauge is currently a static visualization. You will use the report just created to seed a dashboard whose visualizations update as new messages arrive.

1.  Select the Pin Live Page icon located near the top right of the Gauge control. 

    ![On the Gauge control menu bar, Pin Live Page is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image149.png "Gauge control menu bar")

2.  Select New **dashboard**, enter **Real-time Sentiment** as the name, and select Pin Live. 

    ![On the Pin to dashboard dialog box, on the left, a Preview of the ChatSentiment Gauge graph displays. On the right, under Where would you like to pin to, the New dashboard radio button is selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image150.png "Pin to dashboard dialog box")

3.  Return to the **My Workspace** page, and select your newly created dashboard from the list of dashboards. 

    ![My Workspace dashboards with Real-time Sentiment selected](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image151.png "My Workspace dashboards")

4.  Real-time dashboards are created in Power BI using the Q&A feature, by typing in a question to visualize in the space provided. In the "Ask a question about your data" field, enter: "average score created between yesterday and today".
    
    ![\"average score created between yesterday and today\" is typed in the Ask question about your data field. An average of score (0.62) displays below.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image152.png "Ask question about your data field")

5.  Next, convert this to a Gauge chart by expanding the Visualizations palette at right, and selecting the Gauge control.

    ![Visualizations palette with the Gauge control selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image153.png "Visualizations palette")

6.  Format the Gauge control so it ranges between 0.0 and 1.0 and has an indicator at 0.5. To do this, select the brush icon in the Visualization palette, expand the Gauge axis, and for Min enter 0, Max enter 1, and Target enter 0.5. 

    ![In the Visualizations list, on the Visualizations palette, the Gauge graph icon is selected. Beneath that, the brush icon is selected. Under Gauge axis, the following values are defined: Min, 0. Max, 1.0. Target, 0.5.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image154.png "Visualizations list")

7.  Your gauge should now look similar to the following:
    
    ![The Gauge graph for average score created between yesterday and today displays with an average of score of 0.62.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image155.png "Gauge graph")

8.  In the top-right corner, select **Pin visual.**
    
    ![Pin visual option](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image156.png "Pin visual option")

9.  In the dialog that appears, select the dashboard you recently created and select **Pin**. 

    ![On the Pin to dashboard dialog box, on the left, a Preview of the Gauge graph displays. On the right, under Where would you like to pin to, the Existing dashboard radio button is selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image157.png "Pin to dashboard dialog box")

10. In the list of dashboards, select your Real-time Sentiment dashboard. Your new gauge should appear next to your original gauge. If the original gauge fills the whole screen, you may need to scroll down to see the new gauge. You can delete the original gauge if you prefer. (Select the top of the visualization, then ellipses that appear, and the, the trash can icon.)
    
    ![Two Average of score Gauge graphs display, and both are the same.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image158.png "Gauge graphs")

11. Navigate to the chat website you deployed and send some messages and observe how the sentiment gauge updates with moments of you sending chat messages.

12. Try building out the rest of the real-time dashboard that should look as follows. We provide the following Q&A questions you can use to get started.
    
    ![The Power BI dashboard has four panes: two Count of Messages panes, an Average Sentiment, and Upset Users. The first Count of Messages pane displays a number (18). The second Count of Messages is a pie chart broken out by username. The Average Sentiment is a donut chart displaying the Average Sentiment (0.58) in the past 24 hours. Upset Users chart is a horizontal bar chart displaying the average of upset users (0.25) in the past 24 hours.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image159.png "Power BI Dashboard")

    -   Count of Messages (Card visualization): count of messages between yesterday and today

    -   Count of Messages by Username (Pie chart visualization): count of messages by username between yesterday and today

    -   Upset Users (Bar chart visualization): Average score by username between yesterday and today

13. Invite some peers to chat and monitor the sentiments using your new, real-time dashboard.

## Exercise 7: Enabling search indexing

Duration: 30 minutes

Now that you have primed the system with some messages, you will create a Search Index and an Indexer in Azure Search upon the messages that are collected in Azure Cosmos DB.

### Task 1: Verifying message archival

Before going further, a good thing to check is whether messages are being written to Azure Cosmos DB from the Stream Analytics Job.

1.  In the Azure Portal, navigate to your **Azure Cosmos DB account**.

2.  On the left-hand menu, select **Data Explorer**.
    
    ![The Data Explorer sections from within the Azure portal, Cosmos DB has been selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image160.png "Data explorer menu")

3.  Under the **awhotels** Cosmos DB, click **messagestore**, then Documents. You should see some data here.

    ![Documents has been selected from within the Data Explorer in the Azure Portal.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image161.png "Documents selected in Data Explorer")

4.  If you want to peek at the message contents, select any document in the listing.
    ![Message contents display.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image162.png "Message contents")

### Task 2: Creating the index and indexer

1.  Select Resource Groups from the left menu, then select the **intelligent-analytics** resource group.

2.  Select your **Search** **service** instance from the list.

3.  Select **Import data**.

    ![Intelligent-analytics resource group search service instance with Import data selectd. ](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image163.png "Intelligent-analytics resource group")

4.  On the Import data blade, select **Connect to your data**.

    ![The import data blade is shown with the Connect to your data selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image164.png "Implort data blade")

5.  On the Data Source blade, select **Cosmos DB**.

    ![The Cosmos DB link is selected](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image165.png "Cosmos DB link")

6.  Enter **messagestore** for the name of the data source.

    ![New data spirce b;ade with messagestore entered in the Name box. The Cosmos DB accounts blade with an account selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image166.png "New data source and cosmos DB accounts")

7.  Select your **Cosmos DB** account.

8.  Choose your **awhotels** database.

9.  Choose your **messagestore** collection.

10. Select **OK** to complete the Data Source configuration.

11. Select **Customize target index**, and observe that the field list has been pre-populated for you based on data in the collection.

12. Enter **chatmessages** for the name of the index.

13. Leave the Key set to id.
    
    ![Screenshot of the Key ID field.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image167.png "Key ID field")

14. Select the Retrievable check box for the following fields: **message, createDate**, **and username** (id will be selected automatically). Only these fields will be returned in query results.

15. Select the Filterable check box for **createDate, username**, **and sessionId**. These fields can be used with the filter clause only (not used by this Tutorial, but useful to have).

16. Select the Sortable check box for **createDate**, **username**, and **sessionId**. These fields can be used to sort the results of a query.

17. Select the Searchable check box for message. Only these fields are indexed for full text search.

18. Confirm your grid looks similar to the following, and select OK. 

    ![On the Import data blade, Customize target index is selected. The Index blade fields and check boxes are set to previously mentioned settings.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image168.png "Import data and Index blades")

19. Select **Import your data**.
    
    ![Import your data selected](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image169.png "Import your data")

20. On the Create an Indexer blade, enter **messages-indexer** as the name.

21. Set the Schedule toggle to **Custom**.

22. Enter an interval of **5** minutes (the minimum allowed).

23. Set the Start time to **today's date**.

24. The description and other fields can be ignored.

25. Select **OK**. 
    
    ![On the Import data blade, Import your data (Indexer) is selected. On the Create an Indexer blade, fields are set to the previously mentioned settings.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image170.png "Import data and Create an Indexer blades")

26. Select **OK** once more to begin importing data using your indexer.

27. After a few moments, examine the Indexers tile for the status of the Indexer.
    
    ![The Indexers tile shows that there was one success, and zero failed.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image171.png "Indexers tile")

### Task 3: Update the Web App web.config

1.  On your Lab VM, within Visual Studio Solution Explorer, expand the **ChatWebApp** project.

2.  Open **Web.config**.

3.  For the **chatSearchApiBase**, enter the URI of the Search API App (e.g., <http://awchatsearch.azurewebsites.net)>. This value should not be the URL to your instance of Azure Search.

    -   You can find this by going to Resource Groups, selecting the **intelligent-analytics** resource group, and selecting your search app service from the list. 
        
        ![Under Name, the ConciergePlusAppSearchAPI App Service is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image172.png "Name section")

    -   On the Essentials blade for your service, you will find the URL value. 

        ![In the Essentials section of the Essentials blade, the URL value is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image173.png "Essentials blade")

4.  Copy the URL value, and paste it into the value setting for the **chatSearchApiBase** key. 

    ![chatSearchApiBase key displays the URL value copied from the Essentials blade.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image174.png "chatSearchApiBase key")

5.  Save **Web.config**.

### Task 4: Configure the Search API App

1.  Within Visual Studio Solution Explorer, expand the **ChatAPI** project.

2.  Open **web.config**.

3.  This project needs the following three settings configured to capitalize on Azure Search, all of which you can get from the Azure Portal.
    
    ![Code displays in the Web.config window, showing that the following three key settings are being added: SearchServiceName, SearchServiceQueryApiKey, and SearchIndexName.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image175.png "Web.config")

4.  Using the Azure Portal, navigate to the blade of your **Search** service.

5.  For the **SearchServiceName**, enter the name of your Search service (e.g., **awchatter**).

6.  For the **SearchServiceQueryApiKey**, do the following:

    -   On the Search service blade, select Keys on the left-hand menu.

        ![On the Search Service blade, Settings section, under Settings, Keys is selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image176.png "Search Service blade, Settings section")

    -   Select **Manage query keys**. 

        ![Manage query keys selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image177.png "Manage query keys ")

    -   On the Manage query keys blade, copy the \<empty\> key value. 
       
        ![On the Manage query keys blade, the value in the Key field is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image178.png "Manage query keys blade")

    -   Copy this value into the **SearchServiceQueryApiKey** setting.

7.  For the **SearchIndexName** setting, enter the name of the Index you created in Search, chatmessages.

8.  Save **Web.config**.

### Task 5: Re-publish apps

1.  Publish the updated **ChatWebApp** using Visual Studio, as was shown previously in [Exercise 4, Task 2](#task-2-publish-the-chatwebapp).

2.  Within Visual Studio Solution Explorer, right-click the **ChatAPI** project and select **Publish**.
    
    ![In Visual Studio Solution Explorer, the ChatAPI sub-menu displays, with Publish selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image179.png "Visual Studio Solution Explorer")

3.  Select Microsoft Azure App Service, choose the Select **existing** radio button and select **Publish**.

4.  If prompted, sign in with your credentials to your Azure Subscription.

5.  In the App Service dialog, choose your Subscription that contains your API App you provisioned earlier. Expand your Resource Group (e.g., **intelligent-analytics**), then select the node for your API App in the tree view to select it.

    ![In the App Service dialog box, in the tree view, awchat is expanded, and ConciergePlusSearchApi1 is selected.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image180.png "App Service dialog box")

6.  Select **OK**.

7.  When the publishing is complete, a browser window should appear with content similar to the following.
    
    ![A Browser window displays with the message, \"Your App Service app has been created,\" and links to a Quick Start guide, and deployment documentation.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image181.png "Browser window")

8.  Navigate to the Search tab on the deployed Web App and try searching for chat messages. (Note that there is up to a 5-minute latency before new messages may appear in the search results.

    ![In the Search Messages box, in the Search messages for text box, chat is typed. Below that, 6 results have been found.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image182.png "Search Messages box")

## After the hands-on lab 

Duration: 10 minutes

In this exercise, attendees will deprovision any Azure resources that were created in support of the lab. You should follow all steps provided *after* attending the Hands-on lab.

### Task 1: Delete the resource group

1.  Using the Azure portal, navigate to the Resource group you used throughout this hands-on lab by selecting Resource groups in the left menu.

2.  Search for the name of your research group and select it from the list.

3.  Select Delete in the command bar and confirm the deletion by re-typing the Resource group name and selecting Delete.

