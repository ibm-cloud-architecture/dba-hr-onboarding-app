# Quickly create new automation solutions with low-code applications

Existing HR applications are not easily modified and often require the IT team to work with the vendor and make code changes.  There are often higher priority development projects that deliver customer value than HR.  The result: adhoc spreadsheets, emails and local file shares and a disorganized process inhibits the flow of applicants and HR teams are frustrated with slow results.  It's time for an easy to use low-code build environment where HR can create dynamic applications that connect to existing systems and deliver value quickly.

Are you ready to build low-code business applications?

---------

## HR Onboarding Application
**an IBM Cloud Pak for Business Automation entry point**

***

**Entry Point:** Quickly create new automation solutions with low-code applications

**Use Case Overview:** Existing HR applications are not easily modified and often require the IT team to work with the vendor and make code changes.  There are often higher priority development projects that deliver customer value than HR.  The result: adhoc spreadsheets, emails and local file shares and a disorganized process inhibits the flow of applicants and HR teams are frustrated with slow results.  It's time for an easy to use low-code build environment where HR can create dynamic applications that connect to existing systems and deliver value quickly.

  * **Cloud Pak for Business Automation as a Service demo environment (likely an IBMer):** your environment is predeployed, continue to the [Getting Started Lab](https://ibm-cloud-architecture.github.io/refarch-dba/use-cases/hr-onboard-app/).
  * **Install Yourself:** To deploy HR Onboarding App on your own environment, and technical architecture information, continue reading.

### Architecture Diagram

The following diagram illustrates the products involved and the solution components:

 ![0](./images/comp-view-hr-onboarding-app.png)

### Environment

We assume the following products are installed, up and running:

* IBM Cloud Pak® for Automation version 22.0.1
    * Automation Foundation on OpenShift
    * Business Automation Applications (including Studio and App Engine) on OpenShift
    * Business Automation Navigator on OpenShift
    * Business Automation Workflow (BAW) on OpenShift
    * Operational Decision Manager (ODM) on OpenShift

### Deploy the artifacts

1. Determine your credentials
    1. If using Cloud Pak for Business Automation as a Service (CP4BAaaS):
        * You will use a single login to access BAS, BAW, ODM and BPC
        * For BAW and ODM, you will need to create a service credential/account under Access Management to connect in the external automation service and invoke the API to Rule Execution Server, make sure to give the service credential an ODM role the allows execution in the environment that will run the ODM rules
    1. If deploying HR Onboarding App on your own OpenShift environment:
        * Make sure you have a login to all required components above
1. Deploy ODM artifacts
    1. Login to Decision Center Business console
    1. On Library, click the import icon (Note: if you are upgrading an environment that has an earlier version of HR Onboarding App installed, you can go into the existing decision service and import the below ZIP files within each branch, selecting to replace existing when prompted)
    1. Choose and upload `Calculate Candidate Salary Range [main] YYYY.MM.DD_XX.zip`
    1. Open the main branch, click Deployments -> Configurations, edit and adjust the server target within each deployment configuration if required
    1. Deploy to your preferred Rule Execution Server such as Production in CP4BAaaS (Note: if you are upgrading an environment that has an earlier version of HR Onboarding App installed, you should first delete the ruleapp deployed to RES)
1. Deploy Workflow artifacts
    1. Login to Workflow Authoring in Business Automation Studio and navigate to Business automations -> Workflows
    1. Import `HR_Onboarding_Application_Services - YYYY.MM.DD_XX.twx`
    1. Open the HR Onboarding Application Services workflow project and navigate to Process App Settings -> Servers
    1. Edit the settings for hostname, port, authentication and so forth for your ODM server
        1. If using CP4BAaaS: the hostname is based on the Rule Execution Server URL and follows the pattern `odm-<environment>-<tenant_hostname>` with no `https://` at the beginning and the port field left blank
        1. If deploying HR Onboarding App on your own OpenShift environment based on the starter pattern and running on IBM Red Hat OpenShift on IBM Cloud (ROKS): the hostname should be the ODM Decision Server Console route hostname with no `https://` at the beginning and the port left blank
    1. Create a new snapshot of the workflow project
    1. Publish the workflow project's new snapshot to make the automation services available to applications
    1. If more than one Workflow environment is present (such as CP4BAaaS with Development and Production or a custom OpenShift deployment with Workflow Authoring and a separate Workflow Server), install the new snapshot to any additional Workflow Servers desired
        1. If using CP4BAaaS: this is Production by default
        1. If deploying HR Onboarding App on your own OpenShift environment based on the starter pattern and running on IBM Red Hat OpenShift on IBM Cloud (ROKS): there is only one Workflow environment and no need to deploy
        1. If you are upgrading a Workflow environment that has an earlier version of HR Onboarding App installed, go to the Process Admin Console -> Installed Apps and make the new snapshot just deployed the default version, optionally deactivating any old snapshots
    1. Deploy Business Automation Studio artifacts (you can either stop here and complete the Getting Started Lab to create your own app or import the sample app as below)
        1. Import the HR Onboarding App application in Business applications using `HR_Onboarding_App - YYYY.MM.DD_XX.twx`
        1. No edit to the application should be required but if an edit is done, create a new snapshot
        1. Export the application from Business Automation Studio -> Business applications -> HR Onboarding App -> Versions as a ZIP
    1. Deploy Business Automation Navigator artifacts (again, you can either deploy the app that you created when you followed the Getting Started Lab or deploy the sample app you just imported above)
        1. Login to Business Automation Navigator's admin desktop
            1. If using CP4BAaaS: Production -> Manage solutions -> Publish
            1. If deploying Refund Request on your own OpenShift environment: use your Navigator URL with `?desktop=appDesktop1` added to the end and use the menu to go to Administration
        1. Select Connections on the left, edit the Application Engine Connection (generally called `APPENGO`) and connect
        1. Import the application ZIP file previously exported
        1. Edit Details from the application's menu and add appropriate teams to the Permissions table, such as `#AUTHENTICATED-USERS` to make the app available to everyone
        1. Edit the desktop of your choice (generally `appDesktop1`) and on the Layout tab, select the application

## Contributors
  * Lead content developer [Jeff Goodhue](https://www.linkedin.com/in/jeffreygoodhue/)
