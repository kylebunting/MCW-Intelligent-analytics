
# Intelligent Analytics setup

## Requirements

-   Microsoft Azure subscription must be pay-as-you-go or MSDN.

    -   Trial subscriptions will not work.

-   A virtual machine configured with:

    -   Visual Studio Community 2017 or later

    -   Azure SDK 2.9 or later (Included with Visual Studio 2017)



## Before the hands-on lab

Duration: 20 minutes

Synopsis: In this exercise, you will set up your environment for use in the rest of the hands-on lab. You should follow all the steps provided in the Before the Hands-on Lab section to prepare your environment before attending the hands-on lab.

### Task 1: Provision Power BI

If you do not already have a Power BI account:

1.  Go to <https://powerbi.microsoft.com/features/>.

2.  Scroll down until you see the Try Power BI for free! section of the page, and click the Try Free\> button. ![Screenshot of the Power BI Try for free section.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image3.png "Power BI Try for free section")

3.  On the page, enter your work email address (which should be the same account as the one you use for your Azure subscription), and select Sign up. ![The Get started page has a field for entering your work email address.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image4.png "Get started page")

4.  Follow the on-screen prompts, and your Power BI environment should be ready within minutes. You can always return to it via <https://app.powerbi.com/>.

### Task 2: Setup a lab virtual machine (VM)

1.  In the [Azure Portal](https://portal.azure.com/), select +Create a resource, then type "Visual Studio" into the search bar. Select Visual Studio Community 2017 on Windows Server 2016 (x64) from the results. ![In the Azure Portal Everything section, under Results, under Name, Visual Studio Community 2017 on Windows Server 2016 is circled.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image5.png "Azure Portal Everything section")

2.  On the blade that comes up, at the bottom, ensure the deployment model is set to Resource Manager, and select Create.

    ![At the Bottom of the blade, Resource Manager is selected as the deployment model.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image6.png "Bottom of the blade")

3.  Set the following configuration on the Basics tab.

    -   Name: Enter **LabVM**

    -   VM disk type: Select **SSD**

    -   User name: Enter **demouser**

    -   Password: Enter **Password.1!!**

    -   Subscription: Select the subscription you are using for this hands-on lab.

    -   Resource Group: Select Create new, and enter **intelligent-analytics** as the name of the new resource group.

    -   Location: Select a region close to you. 
    
    ![The Basics blade fields fields display the previously mentioned settings.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image7.png "Basics blade")

4.  Select **OK** to move to the next step.

5.  On the Choose a size blade, ensure the Supported disk type is set to SSD, and select View all. This machine won't be doing much heavy lifting, so selecting DS2\_V3 Standard is a good baseline option. 

    ![The Choose a size blade has the D2S\_V3 Standard option circled. The circled fields are Supported disk type, which is set to SSD, and the View all button.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image8.png "Choose a size blade")

6.  Click **Select** to move on to the Settings blade.

1.  Accept all the default values on the Settings blade, and select **OK**.

8.  Select Create on the Create blade to provision the virtual machine. 
  
    ![The Create blade shows that validation passed, and provides the offer details.](images/Hands-onlabstep-by-step-Intelligentanalyticsimages/media/image9.png "Create blade")

9.  It may take 10+ minutes for the virtual machine to complete provisioning.
