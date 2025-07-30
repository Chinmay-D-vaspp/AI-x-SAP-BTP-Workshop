# **Exercise 6 - Trigger Workflow from Build Apps**

### **Overview**

This section explains how to **trigger a workflow from SAP Build Apps**, enabling seamless integration between the app’s user interface and process automation using tools like **SAP Build Process Automation** or **SAP Workflow Management**.

By triggering workflows directly from the app, users can initiate structured business processes—such as approvals, service requests, or data validations—based on UI actions like button clicks or form submissions. This allows citizen developers and business users to extend application functionality with backend automation in a no-code/low-code environment.

The guide covers:

* Configuring the workflow trigger in SAP Build Apps using a **Data Resource** or **REST API integration**

  * Setting up the destination or endpoint for the workflow service (typically a BTP service URL)

  * Passing required input parameters (e.g., user input, form data) to the workflow

  * Executing the trigger using logic flows (e.g., on button tap → call workflow API)

  * Handling response and status updates within the app (e.g., success message, navigation)

This overview helps users understand how to connect the user interface with SAP's workflow capabilities, enabling dynamic and automated business processes directly from the SAP Build Apps environment.

#### Step1: Integrate Process Workflow to Build App Created.

1. Go to app

2. Go to Integration tab

3. Go to Integrations Section --> Click ADD INTEGRATION --> Select the Process Workflow published to Build Apps Library --> Click Enable Process --> Click Save

![](./Exercise%206.img/ex6.img01.jpg)

![](./Exercise%206.img/ex6.img02.png)

![](./Exercise%206.img/ex6.img03.png)

#### Step2: Trigger the Workflow

1. Go to Screen 2 and click the button (Button to Process Invoice for Approval)

2. Go to Logic Editor (Available at bottom of display editor screen)

3. Go to Canvas Logic --> Go to Installed --> Select the HTTP Destination Request -->

   Drag the component and connect the nodes

4. If HTTP destination Request is not available in installed tab --> Click on Market Place --> Search for HTTP destination --> Select the tile --> Click on Install --> Exit Market Place

5. Before going to Properties, you can to Api trigger in Control tower to get the sample payload + relative path needed to set it in properties **Request Body.**  
   1. Go to Control Tower --> Select Environments Tile --> Choose Shared Environment  
   2. Select the Process workflow --> Click on Actions --> Click on View --> Copy API end point and Payload  
6. Go to Properties

   1. Select the Destination --> Sap_process_automation_shared

   2. Select Method --> POST

   3. Select Request Body type --> json

   4. Select the Request Body --> Select formula --> Copy Paste the workflow context details --> Select the variables from App variables (as in sample payloads by default values will empty) --> Save it

   5. Give the relative path in optional inputs (since in destination we already added till API end point we need to give remaining relative path here)

![](./Exercise%206.img/ex6.img04.png)

![](./Exercise%206.img/ex6.img05.jpg)

![](./Exercise%206.img/ex6.img06.png)

![](./Exercise%206.img/ex6.img07.png)

![](./Exercise%206.img/ex6.img08.png)

![](./Exercise%206.img/ex6.img09.png)

![](./Exercise%206.img/ex6.img10.jpg)

![](./Exercise%206.img/ex6.img11.jpg)

![](./Exercise%206.img/ex6.img12.jpg)

![](./Exercise%206.img/ex6.img13.png)

Step3: Add Message Toast

1. Add Message Toast

2. Give Toast Message

![](./Exercise%206.img/ex6.img14.png)

#### Step4: Add Navigate to Home Page

1. Add Navigate back

2. Connect to previous Node

3. Save the work

![](./Exercise%206.img/ex6.img15.png)
