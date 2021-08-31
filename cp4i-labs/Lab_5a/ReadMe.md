# IBM Cloud Pak for Integration

## Asset Repository

[Return to main lab page](../sfdc-experience.md)
---

# Table of Contents 
- [1. Introduction](#introduction)
 
- [2. Creating a Designer Flow in IBM Cloud Pak for Integration](#creating_a_designer_flow_in_cp4i)
  * [2a Testing the API flow](#testing_the_API_flow)
  
- [3. Deploying Your Designer Flow to App Connect Dashboard](#deploying_your_designer_flow_to_app_connect_dashboard)

- [4. Populating the Asset Repository](#populating_asset_repository)
	* [4a Storing a YAML File in the Asset Repository](#yaml_asset_repository)
	* [4b Storing a BAR File in the Asset Repository](#bar_asset_repository)

- [5. Removing Your Designer Flow](#removing_designer_flow)

- [6. Importing a Flow From the Asset Repository](#import_a_designer_flow_in_cp4i)
  * [6a Testing the imported flow](#testing_the_imported_flow)

- [7. Lab Cleanup](#lab_cleanup)
	* [7a Removing Your Deployment](#removing_deployment)
	* [7b Removing Your Flow in Designer](#removing_designer_flow2)
	* [7c Removing Assets from the Asset Repository](#removing_assets)
    
---

# 1. Introduction <a name="introduction"></a>

The purpose of this lab is to introduce you to the Asset Repository. The Asset Repository is a component of IBM Cloud Pak for Integration that allows the user to store, manage, retrieve and search integration assets within the IBM Cloud Pak for Integration and its capabilities. 

Users of IBM Cloud Pak for Integration are able to use the Asset Repository to share Integration assets across the platform capabilities. Storing assets, such as JSON schemas, within this repository allows them to be accessed directly within the user interface of certain Integration capabilities. For example, an OpenAPI specification asset stored in the repository can be directly imported within the IBM API Connect user interface.

Assets can also be located in remote repositories, such as Git. This allows users to take advantage of the versioning capability offered by the remote repository. **Note:** Assets stored in a remote repository are read-only.

Users gain access to the Asset Repository through membership in one or more teams that have access to the repository.

# 2. Creating a Designer Flow in IBM Cloud Pak for Integration <a name="creating_a_designer_flow_in_cp4i"></a>

In this section, we will use App Connect Designer to create a simple flow that will be exposed as an API.

1\. In a browser, enter the URL for the Platform Navigator that is provided by your instructor.

2\. Select the **Enterprise LDAP**:

![alt text][pic0]

**Note:** You may get a warning message that your connection is not private.  If you get this message, you can add an exception.  

To add an exception in the Chrome browser, click **Advanced** and then click **Proceed** to the URL.

![alt text][pic1]

![alt text][pic2]

To add an exception in the Firefox browser, click **Advanced** and then click **Accept the Risk and Continue**.

![alt text][pic3]

![alt text][pic4]

3\. When prompted, use the username and password provided to you for this lab. The username in the screenshots of this lab is chopper13.

![alt text][pic5]

4\. Click on the App Connect Designer link to take you to the designer dashboard.  This should be suffixed with des with your username that you used to sign in with des<span style="color: red">** username number**</span>. In this example, the App Connect Designer link is called ace-des13.

If you have completed previous IBM Cloud Pak for Integration App Connect labs, the designer dashboard will be familiar to you. The App Connect Designer capability provides an IBM App Connect authoring environment for integration flows.

**Note:** If you get a warning message that your connection is not private, follow the instructions in Step 2.

![alt text][pic6]

5\. In this lab, we will be creating a simple API flow.  In the designer dashboard, select **Create flows for an API**.

**Note:** If you prefer, you can use a flow that you created in one of the App Connect labs.  If you use a flow that is already created, you can skip to Section 4.

![alt text][pic7] 

6\. The first thing that we will do is create the model for this. We will call the model **HelloWorld**. Also in the upper left, we will change the Name from **Untitled** to **HelloWorld-<span style="color: red">username</span>**. In the example below, the username is chopper13. You will want to replace chopper13 with your username. Click **Create model**.

![alt text][pic8]

7\. We will map the following property which is data type String. If you have a property that contains a numerical integer value, you could also set the data type to Number.

* Description

Now that we have defined the properties in our API model definition, we can implement a flow by clicking on the **Operations** tab.

![alt text][pic9]

8\. From the **Operations** drop-down menu, select **Add a custom operation**. Here we will customize the operation that we want our API to perform.

![alt text][pic10]

9\. Customize the details of your API operation. **Note:** You can optionally set a description for your individual API operation.
#
- Display Name: **Sample Flow**
- HTTP Verb: **GET** 
- Operation Name: **test**
- Response body: **HelloWorld**

**Note:** The operation name will be a part of your API Endpoint URL and is therefore consumer-facing.

After customizing your API operation, the details should match the image below.

![alt text][pic11]

10\. Now we can click the **Implement Flow** button next to our API operation definition. This will take us to the App Connect Designer flow.

![alt text][pic12]

11\. This we where we can utilize the in-built Smart Mapping feature provided by IBM App Connect Designer. Since we are just building a simple flow, we will not be leveraging the Smart Mapping feature.  If you were leveraging a connectors (e.g. ServiceNow or Salesforce), Smart Mapping grabs all the fields available from an external object.

Click on the **Response** action.

![alt text][pic13]

12\. In the **Response** action, we will define a default value of **Hello World!!!** for the **description** field.

![alt text][pic14]

13\. Click **Done**. This will take you back to the main page for this API. 

![alt text][pic15]

14\. Slide the toggle switch next to **Stopped**. It will change from Stopped to Starting.

![alt text][pic16]

15\. Once it shows Started, click on **Test** on the menu and continue to next section.

![alt text][pic17]

[pic0]: images/0.png
[pic1]: images/1.png
[pic2]: images/2.png
[pic3]: images/3.png
[pic4]: images/4.png
[pic5]: images/5.png
[pic6]: images/6.png
[pic7]: images/7.png
[pic8]: images/8.png
[pic9]: images/9.png
[pic10]: images/10.png
[pic11]: images/11.png
[pic12]: images/12.png
[pic13]: images/13.png
[pic14]: images/14.png
[pic15]: images/15.png
[pic16]: images/16.png
[pic17]: images/17.png

# 2a Testing the API flow <a name="testing_the_API_flow"></a>

Make sure your API is running by checking the upper pane of your Designer instance. Since App Connect Designer has a built-in REST API testing utility, we will leverage it in this lab.

1\. Now select the **GET** operation on the left menu and review the information on this API we just created. You should be able to see the Details pane describing the properties and parameters of your REST API operation. The full URL endpoint for your Designer flow is also displayed.

![alt text][pic18]

2\. Now click on the **Try It** button.

![alt text][pic19]

3\. Scroll down and click on the **Send** button. 

![alt text][pic20]

4\. This will trigger an API call to your Designer flow and in turn populate a Request and Response pane below the Send button. You will see the default value of **Hello World!!!** in the **Description** field.  

![alt text][pic21]

[pic18]: images/18.png
[pic19]: images/19.png
[pic20]: images/20.png
[pic21]: images/21.png

# 3. Deploying Your Designer Flow to App Connect Dashboard <a name="deploying_your_designer_flow_to_app_connect_dashboard"></a>

Now we can export our Designer flow from the App Connect Dashboard on IBM Cloud Pak for Integration. 

1\. Navigate to your App Connect Designer Dashboard so we can export our flow as a BAR file. We can get to the App Connect Designer Dashboard by clicking the **Dashboard** icon on the left-hand pane.

![alt text][pic22] 

2\. After navigating to the Dashboard, click on the triple-dot icon on your Designer flow.

![alt text][pic23]

3\. Click on **Export**.

![alt text][pic24]

4\. Select **Runtime flow asset (BAR)**. This will allow you to download the BAR file to your local directory.  Click **Export** and save the file.

![alt text][pic25]

5\. Now we will go back to the Integration home page. Click on hamburger menu next to **IBM Automation** on the top menu.

![alt text][pic26]

6\. Select **Integrations** under the **Run** menu.

![alt text][pic27]

7\.Now select the **Create a server** option from your App Connect Dashboard capability. 

![alt text][pic28]

8\. We will now select **Quick start designer integration** and then click **Next**.

![alt text][pic29]

9\. Now we will either drag and drop the BAR file we just exported or we can upload it. Click **Next**.

![alt text][pic30]

10\. The next section is for configuration. You can look at all the options that are available. Click **Next**.

![alt text][pic31]

11\. The final section is the server details. We will give it a name (**helloworld-<span style="color: red">username</span>**), set the flow type (**api-flows**), and change the Replicas to **1**. In the example below, the username is chopper13. You will want to replace chopper with your username. Click **Create**.

![alt text][pic32]

12\. This will take you back to the Servers Dashboard where you will see your new server. It will likely be showing **Pending** while it is starting up the pod.

![alt text][pic33]

13\. **Note:** It may take a several minutes to start up.  You can refresh the page. Once it is up and running it will show the following:

![alt text][pic35]

14\. We can also quickly test the API call running in the Integration server.  Click on the **helloworld-<span style="color: red">username</span>** Server.  In the example below, the username is chopper13.

![alt text][pic35]

15\. Click on the **HelloWorld-<span style="color: red">username</span>** API. In the example below, the username is chopper13.

![alt text][pic36]

16\. Select **GET HelloWorld/test**.

![alt text][pic37]

17\. Click on **Try it**.

![alt text][pic74]

18\. Click on **Send**.

![alt text][pic75]

19\. You will see the following results.

![alt text][pic38]

[pic22]: images/22.png
[pic23]: images/23.png
[pic24]: images/24.png
[pic25]: images/25.png
[pic26]: images/26.png
[pic27]: images/27.png
[pic28]: images/28.png
[pic29]: images/29.png
[pic30]: images/30.png
[pic31]: images/31.png
[pic32]: images/32.png
[pic33]: images/33.png
[pic34]: images/34.png
[pic35]: images/35.png
[pic36]: images/36.png
[pic37]: images/37.png
[pic38]: images/38.png
[pic74]: images/74.png
[pic75]: images/75.png

# 4. Populating the Asset Repository <a name="populating_asset_repository"></a>

As mentioned, the IBM Cloud Pak for Integration Asset Repository is an add-on to the IBM Cloud Pak for Integration (ICP4I) that allows the user to store, manage, retrieve and search integration assets within the IBM Cloud Pak for Integration and its capabilities.

Users of the Cloud Pak are able to utilize the Asset Repository to share integration assets across the platform capabilities. Storing assets, e.g. JSON schemas, within this repository allows them to be accessed directly within the user interface of certain Integration capabilities. For example, an OpenAPI specification asset stored in the repository can be directly imported within the IBM API Connect user interface.

In this lab, you will store a .yaml and a .bar file in the repository.

# 4a Storing a YAML File in the Asset Repository <a name="yaml_asset_repository"></a>

1\. Click on the hamburger menu next to **IBM Automation** on the top menu.

![alt text][pic39]

2\. Select **Integrations** under the **Design** menu.

![alt text][pic40]

3\. Navigate to your App Connect Designer Dashboard so we share share your asset with the Asset Repository. We can get to the App Connect Designer Dashboard by clicking the **Dashboard** icon on the left-hand pane.

![alt text][pic41] 

4\. After navigating to the Dashboard, click on the triple-dot icon on your Designer flow.

![alt text][pic23]

5\. Click on **Share as asset**.

![alt text][pic61]

6\. Here we can see details of the asset we are about to share.  Click **Share**. In the example below, the username is chopper13.

![alt text][pic62]

7\. Click on the hamburger menu next to **IBM Automation** on the top menu.

![alt text][pic76]

8\. Select the **Integration capabilities** under **Administration**.

**Note:** If you get a warning message that your connection is not private, follow the instructions in Section 2.

![alt text][pic77]

9\. Select the asset repository.  In the example below, the asset repository is called asset-repo-nlp.

![alt text][pic78]

10\. In additional to a few assets that are already in the Asset Repository, we should also see the asset that was just shared.

![alt text][pic63]

[pic39]: images/39.png
[pic40]: images/40.png
[pic41]: images/41.png
[pic42]: images/42.png
[pic43]: images/43.png
[pic44]: images/44.png
[pic45]: images/45.png
[pic46]: images/46.png
[pic61]: images/61.png
[pic62]: images/62.png
[pic63]: images/63.png
[pic76]: images/76.png
[pic77]: images/77.png
[pic78]: images/78.png

# 4b Storing a BAR File in the Asset Repository <a name="bar_asset_repository"></a>

1\. We should still be in the Asset Repository. If not, click on the hamburger menu next to **IBM Automation** on the top menu. Select the **Integration capabilities** under the **Administration** menu. Select the asset repository.  In the example below, the asset repository is called asset-repo-nlp.

![alt text][pic76]

![alt text][pic77]

![alt text][pic78]

2\. There are likely a few assets already in the repository. Click **Add assets**.

![alt text][pic47]

3\. Now we will either drag and drop the BAR file that we exported earlier or we can upload it.

![alt text][pic45]

4\. Click **Add assets**. In the example below, the username is chopper13. Your asset should reflect your username. 

![alt text][pic48]

[pic47]: images/47.png
[pic48]: images/48.png

# 5. Removing Your Designer Flow <a name="removing_designer_flow"></a>

We will now clean-up the environment so that we can explore how to import assets from the Asset Repository.

1\. Click on hamburger menu next to **IBM Automation** on the top menu.

![alt text][pic56]

2\. Select **Integrations** under the **Design** menu.

![alt text][pic40]

3\. Click the **Dashboard** icon on the left-hand pane.

![alt text][pic41]

4\. Click on the triple-dot icon on your Designer flow.

![alt text][pic23]

5\. Click **Stop**.

![alt text][pic58]

6\. Once the flow shows **Stopped**, click on the triple-dot icon on your Designer flow.

![alt text][pic59]

7\. Click **Delete**.

![alt text][pic60]

[pic56]: images/56.png
[pic57]: images/57.png
[pic58]: images/58.png
[pic59]: images/59.png
[pic60]: images/60.png

# 6. Importing a Flow From the Asset Repository <a name="import_a_designer_flow_in_cp4i"></a>

Now that we have cleaned up the environment, we will import the designer flow that we just shared to the Asset Repository.

1\. We should still be in the App Connect Designer. If not, click on the hamburger menu next to **IBM Automation** on the top menu. Select **Integrations** under the **Design** menu.

![alt text][pic39]

![alt text][pic40]

2\. Click the **Dashboard** icon on the left-hand pane if you are not already there.

![alt text][pic41]

3\. Click on **New**.

![alt text][pic65]

4\. Click **Describe a flow...**.

![alt text][pic79]

5\. Click on the asset that was shared, **HelloWorld-<span style="color: red">username</span>**.

![alt text][pic67]

6\. This will show the metadata of the asset.  Click **Create from asset**.

![alt text][pic68]

7\. The asset that is imported should look like the flow that we created earlier in the lab.

![alt text][pic80]

8\. Slide the toggle switch next to **Stopped**. It will change from Stopped to Starting.

![alt text][pic16]

9\. Once it shows Running, click on **Test** on the menu and continue to next section.

![alt text][pic17]

[pic61]: images/61.png
[pic62]: images/62.png
[pic63]: images/63.png
[pic64]: images/64.png
[pic65]: images/65.png
[pic66]: images/66.png
[pic67]: images/67.png
[pic68]: images/68.png
[pic79]: images/79.png
[pic80]: images/80.png

# 6a Testing the Imported Flow <a name="testing_the_imported_flow"></a>

Make sure your API is running by checking the upper pane of your Designer instance. Since App Connect Designer has a built-in REST API testing utility, we will leverage it in this lab.

1\. Now select the **GET** operation on the left menu and review the information on this API we just created. You should be able to see the Details pane describing the properties and parameters of your REST API operation. The full URL endpoint for your Designer flow is also displayed.

![alt text][pic18]

2\. Now click on the **Try it** button.

![alt text][pic19]

3\. Scroll down and click on the **Send** button. 

![alt text][pic20]

4\. This will trigger an API call to your Designer flow and in turn populate a Request and Response pane below the Send button. You will see the default value of **Hello World!!!** in the **Description** field.  

![alt text][pic21]

# 7. Lab Cleanup <a name="lab_cleanup"></a>

We will now clean-up the environment.

# 7a Removing Your Deployment <a name="removing_deployment"></a>

1\. Now we will go back to the Integration home page. Click on hamburger menu next to **IBM Automation** on the top menu.

![alt text][pic26]

2\. Select **Integrations** under the **Run** menu.

![alt text][pic27]

4\. Click on **Servers**.

![alt text][pic50]

5\. Click on the triple-dot icon on your **helloworld-<span style="color: red">username</span>** Server.

![alt text][pic51]

6\. Click on **Delete**. 

![alt text][pic52]

7\. Confirm the deletion by clicking **Delete**. This will delete the pod associated with your deployment.

![alt text][pic53]

8\. We will also want to delete the BAR file that was deployed. Click on the **BAR files** icon on the left-hand pane in the App Connect Dashboard.

![alt text][pic73]

9\. Click on the triple-dot icon on your **HelloWorld-<span style="color: red">username</span>** bar file.

![alt text][pic54]

10\. Click on **Delete BAR**.

![alt text][pic55]

[pic49]: images/49.png
[pic50]: images/50.png
[pic51]: images/51.png
[pic52]: images/52.png
[pic53]: images/53.png
[pic54]: images/54.png
[pic55]: images/55.png
[pic73]: images/73.png

# 7b Removing Your Flow in Designer <a name="removing_designer_flow2"></a>

1\. Click on hamburger menu next to **IBM Automation** on the top menu.

![alt text][pic56]

2\. Select **Integrations** under the **Design** menu.

![alt text][pic40]

3\. Click the **Dashboard** icon on the left-hand pane.

![alt text][pic41]

4\. Click on the triple-dot icon on your Designer flow.

![alt text][pic23]

5\. Click **Stop**.

![alt text][pic58]

6\. Once the flow shows **Stopped**, click on the triple-dot icon on your Designer flow.

![alt text][pic59]

7\. Click **Delete**.

![alt text][pic60]

8\. Confirm the deletion, by clicking **Delete**.

![alt text][pic82]

[pic56]: images/56.png
[pic57]: images/57.png
[pic58]: images/58.png
[pic59]: images/59.png
[pic60]: images/60.png
[pic82]: images/82.png

# 7c Removing Assets from the Asset Repository <a name="removing_assets"></a>

1\. Click on the hamburger menu next to **IBM Automation** on the top menu.

![alt text][pic69]

2\. Select the **Integration capabilities** under **Administration**.

![alt text][pic77]

9\. Select the asset repository.  In the example below, the asset repository is called asset-repo-nlp.

![alt text][pic78]

3\. Click on the **Delete** icon next to the **HelloWorld-<span style="color: red">username</span>.bar** asset that we shared.

![alt text][pic70]

4\. Click **Delete** to confirm the deletion.

![alt text][pic71]

5\. Click on the **Delete** icon next to the **HelloWorld-<span style="color: red">username</span>** asset that we shared.

![alt text][pic72]

4\. Click **Delete** to confirm the deletion.

![alt text][pic71]

[pic69]: images/69.png
[pic70]: images/70.png
[pic71]: images/71.png
[pic72]: images/72.png

---

# Congratulations!
You have completed the IBM Cloud Pak for Integration Asset Repository lab.

[Return to main lab page](../sfdc-experience.md)