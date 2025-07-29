# **Exercise 3: How to Integrate UI with Backend Services**

## Overview

This section provides an overview of **UI integration in SAP Build Apps**, focusing on how to connect and align the app’s user interface with backend systems, services, and business logic to deliver a fully functional, interactive application.

UI integration is a critical phase in app development where the visual front-end is linked to backend data sources, APIs, and business processes. In SAP Build Apps, this is achieved through no-code/low-code tools that allow users to bind UI components to data variables, configure REST API integrations, and trigger backend logic without writing extensive code.

The guide covers:

*   **Data Binding**: Connecting UI elements (e.g., lists, forms, text fields) to app variables or data resources.
*   **API Integration**: Configuring connections to REST APIs to fetch, create, update, or delete data.
*   **Logic Flows**: Designing client-side logic to handle user interactions (e.g., button clicks, data entry) and orchestrate backend calls.
*   **Authentication**: Setting up authentication methods to securely access backend services.

This overview helps users understand how to bridge the gap between the user interface and backend functionality, transforming a static design into a dynamic, data-driven application.

### **UI Integration**

This section provides an overview of **UI integration in SAP Build Apps**, focusing on how to connect and align the app’s user interface with backend systems, services, and business logic to deliver a fully functional, interactive application.

#### **Let’s start doing UI Integration.**

Step 1: Click on Browse Files Button to add custom logic.  
![](./Exercise%203.img/ex3.img01.jpg)

Step2: To Add logic please click on Logic editor with name “Button 3”

The above are the components I added for browsing File.

Now Let us add one by one

1.  In Logic Canvas (left side-bottom), go to Installed tab and select **“Pick Files**” and drop it in Logic editor.

    ![](./Exercise%203.img/ex3.img02.jpg)

2.  Let’s add another component from market place. Go to Market place and search for **“POST file to DocX Service”** and install it.

    ![](./Exercise%203.img/ex3.img03.png)

3.  Now go to Installed tab and select the component and drop it in logic editor. Connect the Pick files to this component.

    ![](./Exercise%203.img/ex3.img04.png)

4.  Let’s add properties to this component. In properties panel, we need to add **API Key, File to upload, Key and Value.**

    **API Key**: Please get it from Service Key of Document Extraction Service from BTP Cockpit.

    **File to upload**: Click on the option and select the output value of another node.

    ![](./Exercise%203.img/ex3.img05.jpg)

    Select the Picked file path.  
    ![](./Exercise%203.img/ex3.img06.jpg)

    Now you will see the screen like this.

    ![](./Exercise%203.img/ex3.img07.jpg)

    ##### **First Value:**

    **Key**: options

    **Value**: ENCODE_JSON({"schemaId":"cf8cc8a9-1eee-42d9-9a3e-507a61baac23","schemaVersion":"1","clientId":"default","documentType":"invoice","enrichment":{}})

    First Click on Value --> Select Formula --> Paste given Value  
    ![](./Exercise%203.img/ex3.img08.jpg)

    ##### **Second Value:**

    **Key**: Options

    **Value**:

    ENCODE_JSON({"schemaId":"cf8cc8a9-1eee-42d9-9a3e-507a61baac23","schemaVersion":"1","clientId":"default","documentType":"invoice","enrichment":{}})

    In Optional Inputs, give the **relative path --> /document/jobs**

    ![](./Exercise%203.img/ex3.img09.jpg)

5.  Let’s add another component from market place. Go to Market place and search for **“Get file data from DocX service”** and install it.

    Now go to Installed tab and select the component and drop it in logic editor. Connect the POST file to DocX Service to this component.

    Let’s add properties to this component. In properties panel, we need to add **API Key, Job ID.**

    **API Key**: Please get it from Service Key of Document Extraction Service from BTP Cockpit.

    **Job ID**: Click on the option and select the output value of another node.

    ![](./Exercise%203.img/ex3.img10.png)

    Select the **POST file to DocX Service**  
    ![](./Exercise%203.img/ex3.img11.jpg)

    After selecting it, you will get another node output option to select. Please select the

    ##### **Parsed response body.**
