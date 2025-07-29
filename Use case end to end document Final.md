# **Exercise 4 \- Integration of UI**

### **Overview**

This section provides an overview of **UI integration in SAP Build Apps**, focusing on how to connect and align the app’s user interface with backend systems, services, and business logic to deliver a fully functional, interactive application.

SAP Build Apps enables users to visually design responsive UIs and seamlessly integrate them with **external data sources**, **SAP systems**, **APIs**, and **automation workflows**. UI integration ensures that screens and components not only display information but also interact with real-time data, respond to user actions, and reflect business processes.

The guide covers:

* Binding UI components (e.g., input fields, lists, dropdowns) to **data variables**, **formulas**, or **backend services**

* Integrating with **SAP BTP destinations**, **OData services**, or **REST APIs** to fetch and push data

* Using **events and logic flows** to trigger backend calls (e.g., button click → create record)

* Displaying dynamic content based on data responses (e.g., conditional rendering, status messages)

* Managing **UI states** (loading, error, empty) for a smoother user experience

This overview enables users to understand how to build apps where the UI is not static but is deeply integrated with business logic and backend systems, forming the core of intelligent and responsive enterprise applications.

Step 1: Click on Browse Files Button to add custom logic.  
![][image43]

Step2: To Add logic please click on Logic editor with name “Button 3”

The above are the components I added for browsing File.

Now Let us add one by one

1. In Logic Canvas (left side-bottom), go to Installed tab and select **“Pick Files**” and drop it in Logic editor.

![][image44]

2. Now we need to connect the node, select the end of first component and drag it and drop it at second component start node.

3. Next drag **HTTP destination** from installed Logic canvas and connect the pick files node to it

4. Click on Component **HTTP destination request** and on the left side you can view the Properties Panel.

![][image45]

##### **Now we need to fill the details in it**

1. Destination: On click of destination drop down you see the list already available which we added via integrations. Select the **DocX\_destination** which we created in SAP BTP Cockpit destination and add in integration in SAP Build Apps.

![][image46]

2. HTTP Method: Select the method **“POST”** from drop down as we upload the invoice to DocX Service.

3. Request Body Type: select type **“multipart/form-data”**

4. Request Body: Click on Custom List a window opens.

![][image47]

Click on Add Value:

![][image48]

Please add below values:

##### **First Value**:

**Key:** file

**Value**: MERGE(\[outputs\["Browse Files Custom Logic"\].files\[0\], {"mimeType": outputs\["Browse Files Custom Logic"\].files\[0\].mimeType}\])

First Click on Value \--\> Select Formula \--\> Paste given Value  
![][image49]

##### **Second Value:**

**Key**: Options

**Value:** 

ENCODE\_JSON({"schemaId":"cf8cc8a9-1eee-42d9-9a3e-507a61baac23","schemaVersion":"1","clientId":"default","documentType":"invoice","enrichment":{}})

In Optional Inputs, give the **relative path \--\> /document/jobs**

5. In Core, select **show spinner** and connect it to the previous component:

Note: Since we’re posting file content, in background we’re fetching the file content and saving it in some variables we’re adding the busy indicator

Add delay 1 min and connect to previous component (The reason why we need to add delay is , when we upload the file , the DocX service tries to get the content of it and during this time the status of file is ”Pending” even though it is posted , so if we immediately look for file content we will not get the full content so this delay helps to get full file content as response)  
![][image50]

Now click on **delay Component** and add Properties for it

![][image51]

Now from Logic Canvas, select the **JavaScript component**. Drag it and drop it to the logic editor and connect it to the previous component.

**Double click on JS component,** Now this dialog will open, and we must write the script task in it

![][image52]

First let's fill the input: Click on **“ABC” box**

Select the output value of another node

![][image53]

Select the **POST file to DocX Service**  
![][image54]

After selecting it, you will get another node output option to select. Please select the

##### **Parsed response body.**

![][image55]

Now you will see the screen like this, please save it.  
![][image56]

Add the snippet below, which helps to save the unique ID generated for our invoice which got uploaded to DocX service. Through this ID only we will be able to fetch the invoice details

##### 

##### 

##### **Code Snippet:**

**return { result: inputs.input1.id };**

As we’re trying to read the Id from response, this id is nothing but UUID type, which is unique key, so set the output type as UUID in right side Options panel

Please Select Value type \--\> Object and add new property result and add its type as  UUID and click \+ icon.

![][image57]

![][image58]  
![][image59]

##### **Creation of variable**

Now lets create few app variables: We need to create the below app variables, and I will explain why we need it in coming steps

Click on **ADD App Variable**

![][image60]

We need to give variable name, Value type from drop down

![][image61]

Below are the Variable names and Value Types that need to be provided.

| Key | Initial Value | Value Type |
| :---- | :---- | :---- |
| id |  | UUID |
| vendor |  | Text |
| invoiceNumber |  | Text |
| grossAmountText |  | Text |
| documentDateText |  | Text |
| processingInvoiceButton | False | True/False |

Now from Canvas choose **set app Variable**, drag it and drop it. Connect it to JS component.

##### 

![][image62]

Lets add properties to the variable  
![][image63]

In properties Panel, by default in variable name it will show the first variable in the list, but you can click on list and choose which variable you want to set now

On click of list you can see one dialog open like this  
![][image64]

Select **ID** from the list

![][image65]

Save it  
![][image66]

Let's assign actual ID value to this variable, please select the **”ABC” box** in assigned value.

![][image67]

Select the formula  
![][image68] the paste this code in Formula space 

**outputs\["Write JS to read the reonse from file post service"\].result**

Click on Save, from the POST service response we’re able to save the ID into variable which we use later to fetch the invoice details.

As we posted the invoice to DocX , now we can show the Proceed Invoice button which we hid via variable name “processingInvoiceButton”(by default we set value to false during variable declaration).

Select the **“Set App variable”** component, drag it and drop it in logic editor. Connect it to the previous component.

In properties tab , select variable **“processingInvoiceButton”** and assigned value to

##### **“True”**

![][image69]

Now lets get the file content and store main details like Vendor, Invoice Number, Invoice Date, Total Amount in variables

##### **Before that lets do one small step in Integration.**

**Go to Integration and select the destination we added \--\>docX\_destination**

Click on \+ more data entities, you will see the below screen and in the left pane select ”+”  
![][image70]

Now fill in the details in the pop up: Give Service name as GET and select only retrieve ![][image71]

Now you will see the capabilities for this service, as we selected retrieve only retrieve is enabled for now.  
![][image72]

Now we need to fill the Config details: In Relative path we need to give the path so Click on

“ABC” option and **select the formula**

![][image73]

Now we need to give the relative path \+ uniquely generated id which comes as response from DocX service when we “Post” Invoice file.

ADD the below snippet and save: **"/document/jobs/" \+ query.identifier.id**

![][image74]

Now inside identifiers id we must enter id and when we click the **“Run test”** we can see the response from DocX service.

Now In logic canvas, select the **Get record**

![][image75]

Connect it to the previous node and go to Properties of this component

Now lets select the Resource as **“GET”** which we created in integration Tab and select the app variable id from list **(appVars.id)**  
![][image76]

Click on “ABC” option and **select the formula and** paste this formula **appVars.id** and save it

##### **Now let's write JavaScript to read the response and store the invoice details in app variables.**

Under advanced there is a java script

Firstly, select the “ABC” option in input1 and select the Formula  
![][image77]

![][image78]  
![][image79]

Paste the code: outputs\["Get file data from DocX service"\].record.extraction.headerFields 

 Save it

Note: Here my previous component to get record I renamed it to “Get file data from DocX service “ in Advanced properties, if you didn’t rename it then will be displayed in suggestion list by default name “Get Record”  
![][image80]

Add the below code snippet to JS:

const headerFields \= inputs.input1; const getValue \= (name) \=\> {

const match \= headerFields.find(field \=\> field.name \=== name); return match ? match.value : "";  
};

let invoiceNumber \= getValue("invoiceId"); if(\!invoiceNumber) {  
invoiceNumber \= getValue("documentNumber");

}

const invoiceDate \= getValue("documentDate"); let vendorName \= getValue("vendorName"); if(\!vendorName) {  
vendorName \= getValue("receiverName");

}

const totalDue \= getValue("grossAmount");

return {

invoiceNumberText: String(invoiceNumber), documentDateText: String(invoiceDate), vendorNameText: String(vendorName), grossAmountText: String(totalDue)  
};

We need to make sure we add Output object new properties and value type “Text” in Output (right side).Every time you add one property select Add new property.

![][image81]

Now lets drag and drop set app variables and connect to previous node. Since we need 4 values – Vendor, Invoice Number, Invoice Date, Total due. Lets add 4 set app variables and connect each one with the previous node.  
![][image82]

In first set app variable, select variable invoice Number and for assigned value value click on ”ABC” box and then select output value from another node

![][image83]

Select the JS function which returns the invoice Number and then it shows the drop down of values returning from that JS function in which we need to select the invoice Number and save it. Do the same for Vendor, Invoice date and total amount.  
![][image84]

Now lets hide the busy indicator, First select the Hide spinner from Logic Canvas.Drag and drop it to Logic editor and connect to previous node.  
![][image85]

To make each component have some basic info why we’re using it , after selecting each component you can go to properties Panel and in advanced you can mention in Name. what ever I mention in Name will appear on Component just like above screenshot

##### **Now lets go to next screen Invoice Processing:**

##### 

Let’s select each field and in properties lets bind the app variable we defined for it (in screen 1 we saved value in this variables). Under Properties click on Value **ABC** icon. A fragment is  poped up select **Data and variables** followed by **App Variable** and finally select the value required for binding. One example is shown below. Similarly for other inputs also we can bind.

![][image86]![][image87]![][image88]![][image89]![][image90]![][image91]

Let us create process workflow and email trigger then we will integrate the logic for submit button.

# **Exercise 5 \- Creation of Process Workflow**

#### Overview

This section introduces **creating a process flow in SAP BTP**, which involves modeling and automating a sequence of business activities using tools such as **SAP Build Process Automation**, **SAP Workflow Management**, or **SAP Business Process Model and Notation (BPMN)** tools.

Creating a process flow allows organizations to standardize, automate, and monitor key business processes—such as approvals, order management, customer onboarding, or service requests—across departments and systems.

The guide focuses on:

* Defining the process purpose and participants (e.g., initiator, approver, processor)

* Creating a new process in SAP Build Process Automation or Workflow Editor

* Designing the flow using drag-and-drop elements (Start, User Tasks, Conditions, Service Calls, End)

* Assigning roles and decision logic to each task

* Connecting steps using conditions and transitions

* Deploying and testing the process for real-time execution

This overview helps users understand how to convert manual, repetitive workflows into structured, digital processes that improve efficiency, transparency, and compliance within the SAP ecosystem.

Step1: Create a Process Workflow Project

Step2: Select the Project Created and Select the Create button

Step3: Create a Form for Finance Approval

1. ##### **Click Create → Choose Form**

   This is the approval form that the finance team will see.

   2. **Name it** → e.g., FinanceApprovalForm

   3. Add the following **form fields**:

   Invoice Number – *Text* (Read-Only) Merchant/Vendor Name – *Text* (Read-Only) Invoice Date – *Date Picker* (Read-Only) Gross Amount – *Number* (Read-Only)

   4. Click **Save**.

![Picture][image92]

![Picture][image93]

![Picture][image94]

## Step4: Add a Process Artifact

1. Now click Create → **Process**

2. Name it: travelInvoiceApprovalProcess

3. In the opened process editor:

   1. You'll see a **Start** node.

   2. Click the \+ below Start → **Add API Trigger** → Name it → Save it

   3. Add Process Inputs (will bind the context values to this inputs)

![Picture][image95]

![Picture][image96]

![Picture][image97]

![Picture][image98]

## Step5: Add New Step Approval Form

1. Click the \+ below Start → **Add Form** → Select FinanceApprovalForm

2. Give Subject

3. Give Users

   1. Double click on Input field

   2. Select the User ID from Context Values (for now let's assign to person who triggered the task)

   3. Go to Inputs section and bind the context values (double click on Input and select the context value which you want to bind)

![Picture][image99]

![Picture][image100]

![Picture][image101]

## Step6: Add Email Notification Step

1. Below the **Form node**, click \+ on the **Approve branch**

2. Choose Email (You can rename the step)

3. In the **Email Configuration**:

   1. To: Finance Approver or static email (We can bind it from Process Inputs, currently adding it to task initiator)  
   2. Add Subject

   3. Open mail body editor and give mail template (you can style the mail body just with styling options available and can bind values from workflow context)

   4. ![Picture][image102]

   Repeat the same for Rejection mail Step too.

![Picture][image103]

![][image104]

## Step7: Save the Work

1. Click on Release button

2. Select the version released

3. Deploy the Project to Shared Environment

![][image105]

![][image106]  
![][image107]

## Step8: Publish the project to SAP Build Apps library

1. Go to SAP Build Apps lobby

2. Go to project details

3. Select version section

4. Click on **...** available for deployed version

5. Select publish to Library

![][image108]  
![][image109]

# **Exercise 6 \- Trigger Workflow from Build Apps**

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

3. Go to Integrations Section \--\> Click ADD INTEGRATION \--\> Select the Process Workflow published to Build Apps Library \--\> Click Enable Process \--\> Click Save

![][image110]

![][image111]

#### Step2: Trigger the Workflow

1. Go to Screen 2 and click the button (Button to Process Invoice for Approval)

2. Go to Logic Editor (Available at bottom of display editor screen)

3. Go to Canvas Logic \--\> Go to Installed \--\> Select the HTTP Destination Request \--\>

   Drag the component and connect the nodes

4. If HTTP destination Request is not available in installed tab \--\> Click on Market Place \--\> Search for HTTP destination \--\> Select the tile \--\> Click on Install \--\> Exit Market Place

5. Before going to Properties, you can to Api trigger in Control tower to get the sample payload \+ relative path needed to set it in properties **Request Body.**  
   1. Go to Control Tower \--\> Select Environments Tile \--\> Choose Shared Environment  
   2. Select the Process workflow \--\> Click on Actions \--\> Click on View \--\> Copy API end point and Payload  
6. Go to Properties

   1. Select the Destination \--\> Sap\_process\_automation\_shared

   2. Select Method \--\> POST

   3. Select Request Body type \--\> json

   4. Select the Request Body \--\> Select formula \--\> Copy Paste the workflow context details \--\> Select the variables from App variables (as in sample payloads by default values will empty) \--\> Save it

   5. Give the relative path in optional inputs (since in destination we already added till API end point we need to give remaining relative path here)

![][image112]

![][image113]

![][image114]

![][image115]

Step3: Add Message Toast

1. Add Message Toast

2. Give Toast Message

#### Step4: Add Navigate to Home Page

1. Add Navigate back

2. Connect to previous Node

3. Save the work
