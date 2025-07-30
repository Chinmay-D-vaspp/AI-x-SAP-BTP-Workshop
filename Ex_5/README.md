# **Exercise 5 - Creation of Process Workflow**

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

![](./Exercise%205.img/ex5.img01.png)

The configurations for the Process Workflow project are as shown below




![](./Exercise%205.img/ex5.img02.jpg)

Step2: Select the Project Created and Select the Create button

Step3: Create a Form for Finance Approval

1. ##### **Click Create → Choose Form**

   This is the approval form that the finance team will see.

   2. **Name it** → e.g., FinanceApprovalForm

   3. Add the following **form fields**:

   Invoice Number – *Text* (Read-Only) Merchant/Vendor Name – *Text* (Read-Only) Invoice Date – *Date Picker* (Read-Only) Gross Amount – *Number* (Read-Only)

   4. Click **Save**.

![](./Exercise%205.img/ex5.img03.jpg)

![](./Exercise%205.img/ex5.img04.jpg)

![](./Exercise%205.img/ex5.img05.png)

![](./Exercise%205.img/ex5.img06.png)

![](./Exercise%205.img/ex5.img07.png)

![](./Exercise%205.img/ex5.img08.jpg)

## Step4: Add a Process Artifact

1. Now click Create → **Process**

2. Name it: travelInvoiceApprovalProcess

3. In the opened process editor:

   1. You'll see a **Start** node.

   2. Click the + below Start → **Add API Trigger** → Name it → Save it

   3. Add Process Inputs (will bind the context values to this inputs)


![](./Exercise%205.img/ex5.img10.jpg)

![](./Exercise%205.img/ex5.img11.jpg)

![](./Exercise%205.img/ex5.img12.jpg)

![](./Exercise%205.img/ex5.img13.png)

![](./Exercise%205.img/ex5.img14.jpg)

![](./Exercise%205.img/ex5.img15.jpg)  

![](./Exercise%205.img/ex5.img16.jpg)

## Step5: Add New Step Approval Form

1. Click the + below Start → **Add Form** → Select FinanceApprovalForm

2. Give Subject

3. Give Users

   1. Double click on Input field

   2. Select the User ID from Context Values (for now let's assign to person who triggered the task)

   3. Go to Inputs section and bind the context values (double click on Input and select the context value which you want to bind)

![](./Exercise%205.img/ex5.img17.jpg)  

![](./Exercise%205.img/ex5.img18.jpg)

![](./Exercise%205.img/ex5.img19.jpg)

![](./Exercise%205.img/ex5.img20.jpg)

![](./Exercise%205.img/ex5.img21.jpg)

![](./Exercise%205.img/ex5.img22.jpg)

![](./Exercise%205.img/ex5.img23.jpg)

## Step6: Add Email Notification Step

1. Below the **Form node**, click + on the **Approve branch**

2. Choose Email (You can rename the step)

3. In the **Email Configuration**:

   1. To: Finance Approver or static email (We can bind it from Process Inputs, currently adding it to task initiator)  
   2. Add Subject

   3. Open mail body editor and give mail template (you can style the mail body just with styling options available and can bind values from workflow context)

   4. Repeat the same for Rejection mail Step too.

![](./Exercise%205.img/ex5.img24.png)

![](./Exercise%205.img/ex5.img25.png)

![](./Exercise%205.img/ex5.img26.jpg)

![](./Exercise%205.img/ex5.img27.png)

![](./Exercise%205.img/ex5.img28.png)

## Step7: Save the Work

1. Click on Release button

2. Select the version released

3. Deploy the Project to Shared Environment

![](./Exercise%205.img/ex5.img29.png)

![](./Exercise%205.img/ex5.img30.png)

![](./Exercise%205.img/ex5.img31.jpg)

## Step8: Publish the project to SAP Build Apps library

1. Go to SAP Build Apps lobby

2. Go to project details

3. Select version section

4. Click on **...** available for deployed version

5. Select publish to Library

![](./Exercise%205.img/ex5.img32.jpg)

![](./Exercise%205.img/ex5.img33.jpg)
