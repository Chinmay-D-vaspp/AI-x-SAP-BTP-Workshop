# **Exercise 4 - Integration of UI**

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

![](./Exercise%204.img/ex4.img01.png)

Step2: To Add logic please click on Logic editor with name “Button 3”

![](./Exercise%204.img/ex4.img02.jpg)

The above are the components I added for browsing File.

Now Let us add one by one

1. In Logic Canvas (left side-bottom), go to Installed tab and select **“Pick Files**” and drop it in Logic editor.

![](./Exercise%204.img/ex4.img03.jpg)

2. Now we need to connect the node, select the end of first component and drag it and drop it at second component start node.

![](./Exercise%204.img/ex4.img04.png)

3. Next drag **HTTP destination** from installed Logic canvas and connect the pick files node to it

![](./Exercise%204.img/ex4.img05.png)

4. Click on Component **HTTP destination request** and on the left side you can view the Properties Panel.

![](./Exercise%204.img/ex4.img06.jpg)

##### **Now we need to fill the details in it**

![](./Exercise%204.img/ex4.img07.jpg)

1. Destination: On click of destination drop down you see the list already available which we added via integrations. Select the **DocX_destination** which we created in SAP BTP Cockpit destination and add in integration in SAP Build Apps.

![](./Exercise%204.img/ex4.img08.jpg)

2. HTTP Method: Select the method **“POST”** from drop down as we upload the invoice to DocX Service.

![](./Exercise%204.img/ex4.img09.jpg)

3. Request Body Type: select type **“multipart/form-data”**

![](./Exercise%204.img/ex4.img10.jpg)

4. Request Body: Click on Custom List a window opens.

![](./Exercise%204.img/ex4.img11.jpg)

Click on Add Value:

![](./Exercise%204.img/ex4.img12.png)

Please add below values:

##### **First Value**:

**Key:** file

**Value**: 

Code Snippet 1

```javascript 
MERGE([outputs["Pick files"].files[0], {"mimeType": outputs["Pick files"].files[0].mimeType}])
```

![](./Exercise%204.img/ex4.img13.png)

First Click on Value --> Select Formula --> Paste given Value 

![](./Exercise%204.img/ex4.img14.jpg)

##### **Second Value:**

**Key**: options

**Value:** 

Code Snippet 2

```javascript
ENCODE_JSON({"schemaId":"cf8cc8a9-1eee-42d9-9a3e-507a61baac23","schemaVersion":"1","clientId":"default","documentType":"invoice","enrichment":{}})
```

![](./Exercise%204.img/ex4.img15.jpg)

In Optional Inputs, give the **relative path -->** 

```javascript
/document/jobs
```

![](./Exercise%204.img/ex4.img16.jpg)


5. In Core, select **show spinner** and connect it to the previous component:

Note: Since we’re posting file content, in background we’re fetching the file content and saving it in some variables we’re adding the busy indicator

![](./Exercise%204.img/ex4.img17.jpg)

Add delay 1 min and connect to previous component (The reason why we need to add delay is , when we upload the file , the DocX service tries to get the content of it and during this time the status of file is ”Pending” even though it is posted , so if we immediately look for file content we will not get the full content so this delay helps to get full file content as response)  

![](./Exercise%204.img/ex4.img18.png)

Now click on **delay Component** and add Properties for it

![](./Exercise%204.img/ex4.img19.png)

Now from Logic Canvas, select the **JavaScript component**. Drag it and drop it to the logic editor and connect it to the previous component.

![](./Exercise%204.img/ex4.img20.jpg)

![](./Exercise%204.img/ex4.img21.jpg)

**Double click on JS component,** Now this dialog will open, and we must write the script task in it

First let's fill the input: Click on **“ABC” box**

![](./Exercise%204.img/ex4_9.png)

Select the output value of another node

![](./Exercise%204.img/ex4.img24.png)

Select the **POST file to DocX Service** 

![](./Exercise%204.img/ex4_8.png)

After selecting it, you will get another node output option to select. Please select the
##### **Parsed response body.**

![](./Exercise%204.img/ex4.img26.png)

Now you will see the screen like this, please save it.  

![](./Exercise%204.img/ex4.img27.jpg)

Add the snippet below, which helps to save the unique ID generated for our invoice which got uploaded to DocX service. Through this ID only we will be able to fetch the invoice details

##### **Code Snippet 3:**

```javascript
return { result: inputs.input1.id };
```

Please Select Value type --> Object

![](./Exercise%204.img/ex4_1.png)

Add new property result and add its type and click + icon.

![](./Exercise%204.img/ex4_2.png)

So we’re trying to read the Id from response, this id is nothing but UUID type, which is unique key, so we have set the output type as UUID in right side Options panel

![](./Exercise%204.img/ex4_3.png)


##### **Creation of variable**

Now lets create few app variables: We need to create the below app variables, and I will explain why we need it in coming steps

![](./Exercise%204.img/ex4.img32.jpg)

Click on **ADD App Variable**

![](./Exercise%204.img/ex4.img33.jpg)

Please select the option **From Scratch**

![](./Exercise%204.img/ex4_4.png)

We need to give variable name, Value type from drop down

![](./Exercise%204.img/ex4.img35.png)

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

![](./Exercise%204.img/ex4.img36.png)

![](./Exercise%204.img/ex4.img00.png)

Lets add properties to the variable  

![](./Exercise%204.img/ex4.img37.jpg)

In properties Panel, by default in variable name it will show the first variable in the list, but you can click on list and choose which variable you want to set now

![](./Exercise%204.img/ex4.img38.jpg)

On click of list you can see one dialog open like this  

![](./Exercise%204.img/ex4.img39.jpg)

Select **ID** from the list

![](./Exercise%204.img/ex4.img40.png)

Save it  

![](./Exercise%204.img/ex4.img41.jpg)

Let's assign actual ID value to this variable, please select the **”ABC” box** in assigned value.

![](./Exercise%204.img/ex4.img42.png)

Select the formula  

![](./Exercise%204.img/ex4.img43.png) 

then paste this code in Formula space 

Use the code snippet 4: 

```javascript
outputs["Function"].result
```

To Set the app variable

![](./Exercise%204.img/ex4_5.png)

Click on Save, from the POST service response we’re able to save the ID into variable which we use later to fetch the invoice details.

As we posted the invoice to DocX , now we can show the Proceed Invoice button which we hid via variable name “processingInvoiceButton”(by default we set value to false during variable declaration).

Select the **“Set App variable”** component, drag it and drop it in logic editor. Connect it to the previous component.

![](./Exercise%204.img/ex4.img44.jpg)

In properties tab , select variable **“processingInvoiceButton”** and assigned value to

##### **“True”**

![](./Exercise%204.img/ex4_6.png)

Go to Integrations we will add the Data entity to Docx destination

Click on Add Rest API Data Entity, to post the data from Build Apps to Documentation AI.
     ![](./Exercise%203.img/ex3.img05.jpg)
    
The Dialog is popped up. Fill the details as below:
    *   **Name**: GET
    *   Select the checkbox for **Autogenerate Configuration**
    *   **Entity path within the api**: `/document/jobs`
    *   Select the checkbox for **Retrieve Only**
    *   Click on **Add**
    ![](./Exercise%203.img/image_ex3_2.png)

Go to list and click on **Run Test** and you should get status 200.

Now lets get the file content and store main details like Vendor, Invoice Number, Invoice Date, Total Amount in variables

Now In logic canvas, select the **Get record**

![](./Exercise%204.img/ex4.img54.png)

Connect it to the previous node and go to Properties of this component

![](./Exercise%204.img/ex4.img55.png)

Now lets select the Resource as **“GET”** which we created in integration Tab and select the app variable id from list **(appVars.id)**  
![](./Exercise%204.img/ex4.img56.jpg)

Click on “ABC” option and **select the formula and** paste this formula **appVars.id** and save it

![](./Exercise%204.img/ex4.img57.jpg)

##### **Now let's write JavaScript to read the response and store the invoice details in app variables.**

Under advanced there is a java script

Firstly, select the “ABC” option in input1 and select the Formula  
![](./Exercise%204.img/ex4.img58.jpg)

![](./Exercise%204.img/ex4.img59.jpg)  

Paste the code snippet 5: 

```javascript
outputs["Get record"].record.extraction.headerFields
```

 Save it

![](./Exercise%204.img/ex4_10.png)

Add the below code snippet 6 to JS:

```javascript
const headerFields = inputs.input1; const getValue = (name) => {

const match = headerFields.find(field => field.name === name); return match ? match.value : "";  
};

let invoiceNumber = getValue("invoiceId"); if(!invoiceNumber) {  
invoiceNumber = getValue("documentNumber");

}

const invoiceDate = getValue("documentDate"); let vendorName = getValue("vendorName"); if(!vendorName) {  
vendorName = getValue("receiverName");

}

const totalDue = getValue("grossAmount");

return {

invoiceNumberText: String(invoiceNumber), documentDateText: String(invoiceDate), vendorNameText: String(vendorName), grossAmountText: String(totalDue)  
};
```

![](./Exercise%204.img/ex4.img62.jpg)

We need to make sure we add Output object new properties and value type “Text” in Output (right side).Every time you add one property select Add new property.

![](./Exercise%204.img/ex4.img63.jpg)

Now lets drag and drop set app variables and connect to previous node. Since we need 4 values – Vendor, Invoice Number, Invoice Date, Total due. Lets add 4 set app variables and connect each one with the previous node.  

![](./Exercise%204.img/ex4.img64.jpg)

In first set app variable, select variable invoice Number and for assigned value value click on ”ABC” box and then select output value from another node

![](./Exercise%204.img/ex4.img65.jpg)

![](./Exercise%204.img/ex4.img66.jpg)

Select the JS function which returns the invoice Number and then it shows the drop down of values returning from that JS function in which we need to select the invoice Number and save it. Do the same for Vendor, Invoice date and total amount.  

![](./Exercise%204.img/ex4.img67.png)

![](./Exercise%204.img/ex4.img68.jpg)

Now lets hide the busy indicator, First select the Hide spinner from Logic Canvas.Drag and drop it to Logic editor and connect to previous node.  

![](./Exercise%204.img/ex4.img69.jpg)

We are now going to create the capture image functionality.

First copy the Browse files button

![](./Exercise%204.img/Capture1.png)

Rename the copied entity as Capture Image, the logic of the browse files buttons will also be copied now we have to modify it slightly to have the capture image functionality.

![](./Exercise%204.img/Capture2.png)

Now delete the Pick file logic from the Capture Image button Logic.

![](./Exercise%204.img/Capture3.png)

Replace that with Core -> Device -> Take Photo button.

![](./Exercise%204.img/Capture4.png)

Connect the Take Photo button with Event component and the HTTP Request part similar to how Pick file is configured.

![](./Exercise%204.img/Capture5.png)

Add the appropriate configurations as done in Browse Files button.

![](./Exercise%204.img/Capture6.png)

For HTTP Request configure it for Formula

![](./Exercise%204.img/Capture7.png)

Now we have finished setting up the Capture Image button and the subsequent functionality.

To make each component have some basic info why we’re using it , after selecting each component you can go to properties Panel and in advanced you can mention in Name. what ever I mention in Name will appear on Component just like above screenshot.

Now Click on the Review Invoice Details button and add the Navigation Logic to it to Open up the Invoice Overview page

![](./Exercise%204.img/ex4_8.jpeg)

**Now lets go to next screen Invoice Processing:**

![](./Exercise%204.img/ex4.img70.jpg)

First lets set the Back button to navigate us back to the Upload Invoice page

![](./Exercise%204.img/ex4_11.png)

Let’s select each field and in properties lets bind the app variable we defined for it (in screen 1 we saved value in this variables). Under Properties click on Value **ABC** icon. A fragment is  poped up select **Data and variables** followed by **App Variable** and finally select the value required for binding. One example is shown below. Similarly for other inputs also we can bind. 

![](./Exercise%204.img/ex4.img71.png)

![](./Exercise%204.img/ex4.img72.png)

![](./Exercise%204.img/ex4.img73.png)

![](./Exercise%204.img/ex4.img74.png)

![](./Exercise%204.img/ex4.img75.png)

![](./Exercise%204.img/ex4.img76.png)

Let us create process workflow and email trigger then we will integrate the logic for submit button. 
