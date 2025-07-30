# Exercise 3 – Integration of SAP Document AI and Process Automation into Build Apps

1.  Select Integration Tab and click on “ADD INTEGRATION”.
    ![](./Exercise%203.img/ex3.img01.jpg)

2.  Under SAP Systems select BTP Destinations.
    ![](./Exercise%203.img/ex3.img02.jpg)

3.  We can see the list of Destinations that are set in our BTP Cockpit. Select the destination set for SAP Doc AI, in our case it is Docx_Destination and then click on Save.
    ![](./Exercise%203.img/ex3.img03.png)
    
5.  Select Install Integration.
    ![](./Exercise%203.img/ex3.img04.png)
    
7.  Click on Add Rest API Data Entity, to post the data from Build Apps to Documentation AI.
     ![](./Exercise%203.img/ex3.img05.jpg)
    
9.  The Dialog is popped up. Fill the details as below:
    *   **Name**: post
    *   Select the checkbox for **Autogenerate Configuration**
    *   **Entity path within the api**: `/document/jobs`
    *   Select the checkbox for **Enable all**
    *   Click on **Add**
    ![](./Exercise%203.img/ex3.img06.jpg)

10.  Click on **Run Test** and you should get status 200.
    ![](./Exercise%203.img/ex3.img07.jpg)

12.  In Logic Canvas (left side-bottom), go to Installed tab and select **“Pick Files”** and drop it in Logic editor.

13.  Next drag **HTTP destination** from installed Logic canvas and connect the pick files node to it.

14. Click on Component **HTTP destination request** and on the left side you can view the Properties Panel. Now we need to fill the details in it:
    *   **Destination**: Select the DocX_destination which we created in SAP BTP Cockpit destination.
    *   **HTTP Method**: Select the method “POST”.
    *   **Request Body Type**: select type “multipart/form-data”.
    *   **Request Body**: Click on Custom List a window opens. Click on Add Value and add the below values:
        *   **First Value:**
            *   **Key**: `file`
            *   **Value**: `MERGE([outputs["Browse Files Custom Logic"].files[0], {"mimeType": outputs["Browse Files Custom Logic"].files[0].mimeType}])`
            *   First Click on Value --> Select Formula --> Paste given Value.
        *   **Second Value:**
            *   **Key**: `Options`
            *   **Value**: `ENCODE_JSON({"schemaId":"cf8cc8a9-1eee-42d9-9a3e-507a61baac23","schemaVersion":"1","clientId":"default","documentType":"invoice","enrichment":{}})`
    *   In **Optional Inputs**, give the relative path: `/document/jobs`

15. In Core, select **show spinner** and connect it to the previous component.
    *Note: Since we’re posting file content, in background we’re fetching the file content and saving it in some variables we’re adding the busy indicator.*

16. Add **delay 1 min** and connect to previous component.
    *(The reason why we need to add delay is, when we upload the file, the DocX service tries to get the content of it and during this time the status of file is ”Pending” even though it is posted, so if we immediately look for file content we will not get the full content so this delay helps to get full file content as response)*

17. Now from Logic Canvas, select the **JavaScript** component. Drag it and drop it to the logic editor and connect it to the previous component.
    ![](./Exercise%203.img/ex3.img08.jpg)

18. Double click on JS component. First let's fill the input: Click on “ABC” box and select the output value of another node.
    ![](./Exercise%203.img/ex3.img09.jpg)

19. Select the **POST file to DocX Service**. After selecting it, you will get another node output option to select. Please select the **Parsed response body**.
    ![](./Exercise%203.img/ex3.img10.png)

20. Now you will see the screen like this, please save it.

21. Add the snippet below, which helps to save the unique ID generated for our invoice which got uploaded to DocX service. Through this ID only we will be able to fetch the invoice details.
    *   **Code Snippet**:
        `return { result: inputs.input1.id };`
    *   As we’re trying to read the Id from response, this id is nothing but UUID type, which is unique key, so set the output type as UUID in right side Options panel.
    *   Please Select **Value type** --> **Object** and add new property **result** and add its type as **UUID** and click **+** icon.
    ![](./Exercise%203.img/ex3.img11.jpg)
