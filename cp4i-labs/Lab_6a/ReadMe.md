# IBM Cloud Pak for Integration

## Operations Dashboard

[Return to main lab page](../sfdc-experience.md)
---

# Table of Contents 
- [1. Introduction](#introduction)
 
- [2. Configuring IBM MQ](#configuring_MQ)
  
- [3. Configuring an Integration Flow Using IBM App Connect Enterprise Toolkit](#configuring_integrationflow_toolkit)

- [4. Deploying your Integration Flow to App Connect Dashboard](#deploying_integrationflow)

- [5. Configuring IBM API Connect](#configuring_apiconnect)

- [6. Using Operations Dashboard](#using_operations_dashboard)

- [7. Lab Cleanup](#lab_cleanup)
	* [7a Removing Assets from IBM API Connect](#removing_assets)
	* [7b Removing Your Deployment](#removing_deployment)
	* [7c Removing the MQ Queue](#removing_queue)
    
---

# 1. Introduction <a name="introduction"></a>

The purpose of this lab is to introduce you to the Operations Dashboard. The IBM Cloud Pak for Integration Operations Dashboard Add-on provides cross-component transaction tracing to allow troubleshooting and investigating errors and latency issues across integration capabilities to ensure applications meet service level agreements.

**Main features:**
#
- Visually follow the journey of distributed transactions – from the entry as an API, to invocation of an integration flow, placement into an Event Streams topic or an MQ queue and beyond, including complex, asynchronous and long-running use cases
- Quickly understand latencies and errors across integration capabilities
- Identify and focus on issues using builtin dashboards and advanced filtering based on capability-specific terminology
- Execute and schedule reports and define proactive alerts to quickly respond to issues
- Non-intrusive implementation that does not require changes to existing integration code for sending tracing data

**Supported capabilities:**
Currently, the Operations Dashboard supports tracing across the following IBM Cloud Pak for Integration capabilities:
#
- App Connect (ACE)
	* Only HTTP, SOAP, MQ and Kafka nodes are supported
	* Tracing MQ messages is only supported for MQ messages that are copied into the default output location. If "Output data location" or "Result data location" (e.g. in the MQGet node) is set to a different location, those messages will not be traced by Operations Dashboard
- API Connect
	* APIs using API Gateway are supported
	* APIs using v5 Compatible Gateway are not supported
- MQ
	* Only MQ messages that include an MQRFH2 header or message properties are supported
	* MQ client connection is supported, binding mode is not supported
- External applications
	* Applications deployed inside the Cloud Pak for Integration cluster can push trace data into the Operations Dashboard runtime (requires code changes to the application)

**Note:** An additional license must be purchased to remove the usage limitations of this add-on capability. Without this license, the following limitations are valid:
#
- Trace data for Operations Dashboard generated from any of the IBM Cloud Pak for Integration capabilities cannot be exported or used outside of IBM Cloud Pak for Integration Operations Dashboard
- Spans are available for a limited duration based on the following conditions (if any of these conditions are reached, new spans will replace existing spans that have met the established limit):
	* Maximum duration of two hours
	* When the total number of spans within IBM Cloud Pak for Integration platform reaches one million

In this lab, we will use the IBM Cloud Pak for Integration to deploy an integration flow on containers and expose it as a secure rate-limited API.

Extending access via APIs to your back-end integrations empowers your partners and developer community to create new business value, technical value, and customer experiences for your products and offerings.

In this lab, we will deploy an app integration flow that takes data from one source and sends it to a message queue for reliable delivery. Then, we will expose this integration as a rate-limited API secured by a key and secret.  Lastly, we will look at the tracing tracing capability provided by the IBM Cloud Pak for Integration.

# 2. Configuring MQ <a name="configuring_MQ"></a>

In this section, we will configure MQ.

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

4\. Click on the qmgr link to take you to the MQ Console.  This should be suffixed with your username number that you used to sign in with. In this example, it is showing sign in as chopper13 and the qmgr link is qmgr13.

**Note:** If you get a warning message that your connection is not private, follow the instructions in Step 2.

The IBM MQ Console is a web-based user interface that can be used to perform common administration tasks.

![alt text][pic7]

5\. From the IBM MQ Console, we can manage the queue manager, create queues, learn MQ basics, and get more information about IBM MQ. Click **Create a queue**.

![alt text][pic8]

6\. On the "Create a queue" page, we can choose a queue type of Local, Alias, Remote, and Model. Click **Local**.

![alt text][pic9]

7\. Enter the queue name: **ORDERS** and keep the default values. **Note:** Queue manager and queue names are case sensitive. Click **Create**.

![alt text][pic10]

8\. Click **Manage** on the menu.

![alt text][pic11]

9\. Click on **Local queue managers**.

![alt text][pic143]

9\. On the **Queues** tab, we see queue names (including the **ORDERS** queue, queue types, and queue depths. Also make sure to note the name of your queue manager. In the example below, the queue manager name is CMQGR13.

![alt text][pic12]

10\. When an application connects to MQ using client bindings, network connections have to be opened up, which can have security implications. These have been addressed with the introduction of the CHLAUTH and CONNAUTH features. In this lab, we will be disabling CHLAUTH. **Note:** In an actual deployment, you will want to consider how you apply your MQ security configuration (e.g. bake the configuration into the image).

Click on **View configuration**.

![alt text][pic43]

11\. Click on **Edit**.

![alt text][pic45]

12\. Click on **Communication**, select **Disabled** from the **CHLAUTH records** drop-down menu, and click **Save**.

![alt text][pic44]

[pic0]: images/0.png
[pic1]: images/1.png
[pic2]: images/2.png
[pic3]: images/3.png
[pic4]: images/4.png
[pic5]: images/5.png
[pic6a]: images/6a.png
[pic6b]: images/6b.png
[pic7]: images/7.png
[pic8]: images/8.png
[pic9]: images/9.png
[pic10]: images/10.png
[pic11]: images/11.png
[pic12]: images/12.png
[pic43]: images/43.png
[pic44]: images/44.png
[pic45]: images/45.png
[pic143]: images/143.png

# 3 Configuring an Integration Flow Using IBM App Connect Enterprise Toolkit <a name="configuring_integrationflow_toolkit"></a>

In this section, we will be opening and examining an application integration flow in the IBM App Connect Enterprise Toolkit. With the Toolkit you can build powerful and complex integration applications, services, and APIs quickly and easily using a visual designer. These integration solutions can be directly deployed to the Cloud Pak for Integration running on-premises, in any cloud, or combinations of both.

1\. Use the connectivity detail that is provided by your instructor.

2\. Click on the **Terminal** icon.

![alt text][pic13a]

If working with a Toolkit that has been installed on Windows, right-click in the Desktop to open a terminal window. In the terminal window, type **ace toolkit** to open the App Connect Enterprise Toolkit.

![alt text][pic13b]

3\. In the **Workspace Launcher** window, enter **home/student/POTWorkspace**. Click **OK**.

![alt text][pic14]

4\. Open the Firefox browser.

![alt text][pic119]

5\.  If you are viewing this page from within the VDI desktop, click on the [**ace-apic-lab6.zip**](asset/ace-apic-lab6.zip) link to download the file.

6\. Click **Download** to download ace-apic-lab6.zip.

![alt text][pic124]

7\. Make sure **Save File** is selected and click **OK**.

![alt text][pic125]

8\. In the Toolkit, click **File** &#8594; **Import**.

![alt text][pic15]

9\. In the **Import** window, expand **IBM Integration** and click **Project Interchange**.  Click **Next**.

![alt text][pic16]

10\. Click **Browse** that is next to **From zip file**.

![alt text][pic17]

11\. Navigate to the directory (**student** &#8594; **Downloads**) that contains the **ace-apic-lab6.zip** file and click **Open**.

![alt text][pic18]

12\. Confirm that the **ace-apic-lab6.zip** file is listed in the **From zip file** field, confirm that the **Project location root** contains the path to your workspace (**home/student/POTWorkspace**), confirm there is a checkmark next to **orders**, and click **Finish**.

![alt text][pic19]

13\. If the Toolkit is on the Welcome screen, close the **Welcome** tab.

![alt text][pic126]

We should see **orders** listed on the **Application Development** tab.

![alt text][pic20]

 If **orders** is not listed, right-click in the **Application Development** pane and click **Refresh**.

![alt text][pic21]

14\. To view the integration flow that we will deploy, click **orders** &#8594; **Resources** &#8594; **Subflows** &#8594; **getOrder.subflow**.

![alt text][pic22]

15\. If the application that connects to the Queue Manager is also deployed on the same cluster, we would use the service name to connect to the Queue Manager.

In many scenarios, the application that needs to connect to the queue manager may be deployed outside the OpenShift Cluster where the Queue Manager is deployed. In this case, we need an OpenShift Route to connect an application to an IBM MQ Queue Manager from outside an OpenShift cluster. Therefore, we must enable TLS on the IBM MQ queue manager and client application, because Server Name Indication (SNI) is only available in the TLS protocol. The OpenShift Container Platform (OCP) Router uses SNI for routing requests to the IBM MQ Queue Manager. The required configuration of the OpenShift Route depends on the SNI behavior of your client application.  Additional detail can be found at: <a href="https://community.ibm.com/community/user/integration/viewdocument/connecting-to-a-queue-manager-on-cl" target="_blank">https://community.ibm.com/community/user/integration/viewdocument/connecting-to-a-queue-manager-on-cl</a>.

We will now configure the MQ piece. In the message flow, click on the **MQ Output** node. Click on the **Properties** tab and click the **Basic** tab. Enter the queue name, **ORDERS**, that was created in Section 2.

![alt text][pic23]

16\. In a browser, enter the URL for the OpenShift Console that is provided by your instructor.

**Note:** If you get a warning message that your connection is not private, follow the instructions in Section 2 Step 2.

17\. When prompted, use the username and password provided to you for this lab. The username in the screenshots of this lab is chopper13.

![alt text][pic128]

18\. Close the **Welcome to Dev Perspective!** window if presented with one.

![alt text][pic129]

19\. Click on the caret next to **Developer** and select **Administrator**.  **Note**:  If you are already in the Administrator perspective, you can skip this step.

![alt text][pic130]

20\. Click on the caret next to **Networking** and select **Services**.

![alt text][pic131]

21\. You will be presented with a screen that says restricted access.

![alt text][pic132]

Click on the caret next to **Projects: chopper1** and select the project that has the same name as the username that you logged in with.  The project in the screenshots for this lap is chopper13.

![alt text][pic133]

22\. Scroll down until you see the services prefixed with mq.  The service that you will use in the next step is **mq-<span style="color: red">username number</span>-ibm-mq**.

![alt text][pic134]

23\. In the Toolkit, click on the **MQ Connection** tab.  In this use case, App Connect Enterprise will be using a MQ client connection.

![alt text][pic135]

24\. Select **MQ client connection properties** from the drop-down menu for **Connection**.  Enter the name of your queue manager for **Destination queue manager name**, **Queue manager host name** (<span style="color: red">service-name</span>.<span style="color: red">namespace</span>.svc), **Listener port number** (1414), and **Channel name** (SYSTEM.DEF.SVRCONN). In the example below the Destination queue manager name is CMQGR13, the Queue manager host name is qmgr13-ibm-mq.chopper13.svc.

![alt text][pic24]

25\. Save the message flow (**File** &#8594; **Save**).  

![alt text][pic25]

26\. In the **Application Development** pane on the left, right-click on the **orders** flow and click **New** &#8594; **BAR file**.

![alt text][pic26]

27\. Enter the name of BAR file: **<span style="color: red">username</span>_orders** and click **Finish**. App Connect Enterprise is creating an empty BAR file. In the example below, the username is chopper13. You will want to replace chopper13 with your username.

![alt text][pic27]

28\. We will configure which artifacts are compiled in the BAR file. In the **<span style="color: red">username</span>_orders.bar** file, click **orders** and click **Compile and in-line resources**.  Click **Build and Save**. 

![alt text][pic28]

29\. A pop-up window displays “Operation completed successfully.” Click **OK**.

![alt text][pic29]

[pic13a]: images/13a.png
[pic13b]: images/13b.png
[pic14]: images/14.png
[pic15]: images/15.png
[pic16]: images/16.png
[pic17]: images/17.png
[pic18]: images/18.png
[pic19]: images/19.png
[pic20]: images/20.png
[pic21]: images/21.png
[pic22]: images/22.png
[pic23]: images/23.png
[pic24]: images/24.png
[pic25]: images/25.png
[pic26]: images/26.png
[pic27]: images/27.png
[pic28]: images/28.png
[pic29]: images/29.png
[pic119]: images/119.png
[pic120]: images/120.png
[pic121]: images/121.png
[pic122]: images/122.png
[pic123]: images/123.png
[pic124]: images/124.png
[pic125]: images/125.png
[pic126]: images/126.png
[pic127]: images/127.png
[pic128]: images/128.png
[pic129]: images/129.png
[pic130]: images/130.png
[pic131]: images/131.png
[pic132]: images/132.png
[pic133]: images/133.png
[pic134]: images/134.png
[pic135]: images/135.png

# 4. Deploying your Integration Flow to App Connect Dashboard <a name="deploying_integrationflow"></a>

In this section, we will deploy a BAR file in App Connect Enterprise Dashboard.

1\. If the Platform Navigator is not longer open, in a browser, enter the URL for the Platform Navigator that is provided by your instructor and log in.

If the Platform Navigator is open, expand the menu in the upper left.

![alt text][pic30]

Click on **Integrations** under the **Run** menu.

![alt text][pic31]

**Note:** If you get a warning message that your connection is not private, follow the instructions in Section 2.

2\. Select the **Create a server** option from your App Connect Dashboard capability. 

![alt text][pic32]

3\. In the **Create an Integration Server** page. You have two options to deploy a BAR file.  We can deploy a BAR file from App Connect Toolkit or from App Connect Designer. In this lab, we will deploy a BAR file from App Connect Toolkit. Click **Quick start toolkit integration** and click **Next**.

![alt text][pic33]

4\. Now we will either drag and drop the BAR file we created in Section 3 or we can upload it. Click **Next**.

![alt text][pic34]

5\. The next section is for configuration. You can look at all the options that are available. Click **Next**.

![alt text][pic35]

6\. The final section is the server details. We will give it a name (**<span style="color: red">username</span>-orders**), enable the toggle for **Enable Operations Dashboard tracing**, and enter **cp4i-tracing** for the **Operations Dashboard namespace**.  In the example below, the username is chopper13. You will want to replace chopper13 with your username. Click **Create**.

![alt text][pic36]

7\. This will take you back to the Servers Dashboard where you will see your new server. It will likely be showing **Pending** while it is starting up the pod.

![alt text][pic37]

8\. **Note:** It may take a several minutes to start up.  You can refresh the page. Once it is up and running it will show the following:

![alt text][pic39]

9\. We can also quickly test the API call running in the Integration server.  Click on the **<span style="color: red">username</span>-orders** Server.  In the example below, the username is chopper13.

![alt text][pic40]

10\. Click on the **orders** API.

![alt text][pic41]

11\. Click on **GET/{order}**.

![alt text][pic42]

12\. Click on **Try it**.

![alt text][pic144]

13\. Enter **0000** for **orders** and click **Send**.

![alt text][pic46]
 
If the API call is successful, you see JSON reply with {“accountid”:“ABC-1234567890”,“orderid”:“0000”}.

![alt text][pic145]

14\. Let's check to see if the message is in the MQ queue.  Select your Queue Manager instance. This should be suffixed with your username that you used to sign in with. In this example, it is showing sign in as chopper13.

![alt text][pic47]

![alt text][pic48]

15\. Click on **Manage CMGR<span style="color: red">username number</span>**.

![alt text][pic49]

16\. We see that there is a message in the **ORDERS** queue.

![alt text][pic50]

17\. Click on the triple-dot icon for the **ORDERS** queue and click **View messages**.

![alt text][pic51]

18\. We see the message that was put on the **ORDERS** queue by the Integration Server.

![alt text][pic52]

[pic30]: images/30.png
[pic31]: images/31.png
[pic32]: images/32.png
[pic33]: images/33.png
[pic34]: images/34.png
[pic35]: images/35.png
[pic36]: images/36.png
[pic37]: images/37.png
[pic38]: images/38.png
[pic39]: images/39.png
[pic40]: images/40.png
[pic41]: images/41.png
[pic42]: images/42.png
[pic46]: images/46.png
[pic47]: images/47.png
[pic48]: images/48.png
[pic49]: images/49.png
[pic50]: images/50.png
[pic51]: images/51.png
[pic52]: images/52.png
[pic136]: images/136.png
[pic144]: images/144.png
[pic145]: images/145.png

# 5. Configuring IBM API Connect <a name="configuring_apiconnect"></a>

We've created an application integration flow and successfully called it by a REST API call! Now, to make it accessible to the rest of the world, it is important to add security around it - at least in the form of a client ID. This way, in addition to access control, you can get insights into which teams or customers are the least and most active. Adding security to an API is simply done by an OpenAPI configuration parameter. We can add rate limits to the API to increase the calls per second, minute, or hour to scale up as much as you need.

1\. In the Platform Navigator, select your App Connect Dashboard instance.

![alt text][pic53]

![alt text][pic54]

2\. Click on **Servers**.

![alt text][pic55]

3\. Click on the **<span style="color: red">username</span>-orders** Server.  In the example below, the username is chopper13.

![alt text][pic56]

4\. Click on the **orders** API.

![alt text][pic57]

5\. Click on the **Download OpenAPI Document** icon.  This will download a file called **orders-1.0.0.yaml**.  

![alt text][pic58]

6\. In order to proceed with the next steps, we need to add a couple of lines to the yaml file that was downloaded.

Using a text editor, open the **orders-1.0.0.yaml** file.

![alt text][pic140]

![alt text][pic141]

![alt text][pic142]

7\. Add the following lines to then end of the yaml file:

**x-ibm-configuration:**<br/>
&nbsp;&nbsp;**enforced: true**<br/>
&nbsp;&nbsp;**gateway: datapower-api-gateway**
  
**Note:** There are 2 spaces before **enforced** and **gateway**.

![alt text][pic137]

8\. Save the **orders-1.0.0.yaml** file.

9\. In the Platform Navigator, navigate to the API Connect instance.

![alt text][pic59]

![alt text][pic60]

10\. Click **TechCon LDAP User Registry**.

![alt text][pic61]

11\. When prompted, log in with the username and password provided to you for this lab. Click **Log in**. The username in the screenshots of this lab is chopper13.

**Note:** If you get a warning message that your connection is not private, follow the instructions in Section 2.

![alt text][pic62]

12\. Click **Develop APIs and products**.

![alt text][pic63]

13\. Click **Add** &#8594; **API**.

![alt text][pic64]

14\. Click **From existing OpenAPI service** and click **Next**.

![alt text][pic65]

15\. Now we will either drag and drop the yaml file that was downloaded or we can upload it. The yaml should show that it has been successfully validated. Click **Next**.

![alt text][pic66]

16\. This an information about the API creation, review click **Next**. 

![alt text][pic67]

17\. Make sure that **Secure using Client ID** and **CORS** are checked and click **Next**.

![alt text][pic68]

18\. Click **Edit API** in the API Summary.

![alt text][pic69]

19\. In the **API Setup** tab, we can configure the API.  Here we can explore the **Design**, **Source**, and **Assemble** tabs. To test the API, we must activate the API. Switch the toggle from **Offline** to **Online**. This step automatically publishes the API.

![alt text][pic70]

![alt text][pic71] 

20\. Click on the **Test** tab.

![alt text][pic72]

21\. Enter **0000** for the value of **order**.  Click **Send**.

![alt text][pic73]

**Note:** You may get a **No response received** popup.  Click **Here**.

![alt text][pic74]

To add an exception in the Chrome browser, click in the whitespace of the page.

![alt text][pic75]

Blindly type **thisisunsafe**.  This should direct you to a new page that states **401 - Unauthorized**.

![alt text][pic76]

Navigate back to the **API Connect** browser window.

To add an exception in the Firefox browser, click **Advanced** and click **Accept the Risk and Continue**.

![alt text][pic77]

![alt text][pic78]

This will direct you to a new page that states **401 - Unauthorized**.

![alt text][pic79]

Navigate back to the **API Connect** browser window.

22\. Click **Send**.

![alt text][pic80]

23\. We should see the following response.

![alt text][pic81]

[pic53]: images/53.png
[pic54]: images/54.png
[pic55]: images/55.png
[pic56]: images/56.png
[pic57]: images/57.png
[pic58]: images/58.png
[pic59]: images/59.png
[pic60]: images/60.png
[pic61]: images/61.png
[pic62]: images/62.png
[pic63]: images/63.png
[pic64]: images/64.png
[pic65]: images/65.png
[pic66]: images/66.png
[pic67]: images/67.png
[pic68]: images/68.png
[pic69]: images/69.png
[pic70]: images/70.png
[pic71]: images/71.png
[pic72]: images/72.png
[pic73]: images/73.png
[pic74]: images/74.png
[pic75]: images/75.png
[pic76]: images/76.png
[pic77]: images/77.png
[pic78]: images/78.png
[pic79]: images/79.png
[pic80]: images/80.png
[pic81]: images/81.png
[pic137]: images/137.png
[pic140]: images/140.png
[pic141]: images/141.png
[pic142]: images/142.png

# 6. Using Operations Dashboard <a name="using_operations_dashboard"></a>

Cloud Pak for Integration - Operations Dashboard Add-on is based on the Jaeger open source project and the OpenTracing standard to monitor and troubleshoot microservices-based distributed systems. Operations Dashboard can distinguish call paths and latencies. DevOps personnel, developers, and performance specialists now have one tool to visualize throughput and latency across integration components that run on Cloud Pak for Integration. Cloud Pak for Integration - Operations Dashboard Add-on is designed to help organizations that need to meet and ensure maximum service availability and react quickly to any variations in their systems.

1\. If the Platform Navigator, expand the menu in the upper left.

![alt text][pic82]

Click on the caret next to **Operate** and select **Integration tracing**.

**Note:** If you get a warning message that your connection is not private, follow the instructions in Section 2.

![alt text][pic83]

2\. The first time you access Tracing, you might see this what’s new page. Close the window after review.

![alt text][pic85]

3\. In the **Tracing** page, check the **Overview** tab. We see all of the products that you can use this tool with: APIC, APP Connect and IBM MQ.  **Note**: More tracing products will add in the future releases.

![alt text][pic86]

4\. You can monitor each product separately. Click **App C overview**.

![alt text][pic87]

5\. Operations Dashboard generates a list of tracing. Click **Traces** on the left-hand pane.  

![alt text][pic88]

6\. Click on one of the Trace entries. **Note:** Your trace entries may look different than what is captured in the screenshot below. Look for a trace that is tied to your username and shows API C, App C, and MQ.

![alt text][pic89]

7\. We can drill into the individual components.  Click on **orders 1.0.0**.

![alt text][pic90]

The trace will take you through the activity that occurred in IBM API Connect.

![alt text][pic91]

8\. Click on **gen.orders**.

![alt text][pic92]

9\. Click on **ORDERS**.

![alt text][pic93]

[pic82]: images/82.png
[pic83]: images/83.png
[pic84]: images/84.png
[pic85]: images/85.png
[pic86]: images/86.png
[pic87]: images/87.png
[pic88]: images/88.png
[pic89]: images/89.png
[pic90]: images/90.png
[pic91]: images/91.png
[pic92]: images/92.png
[pic93]: images/93.png

# 7. Lab Cleanup <a name="lab_cleanup"></a>

We will now clean-up the environment.

# 7a Removing Assets from IBM API Connect <a name="removing_assets"></a>

1\. In the Platform Navigator, navigate to the API Connect instance.

![alt text][pic94]

![alt text][pic95]

If prompted, log in with the username and password provided to you for this lab. 

2\. Click on **Developer APIs and products**.

![alt text][pic96]

3\. Click on the triple-dot icon for **orders** and click **Delete**.

![alt text][pic97]

4\. Confirm the deletion by clicking **Delete**.

![alt text][pic98]

[pic94]: images/94.png
[pic95]: images/95.png
[pic96]: images/96.png
[pic97]: images/97.png
[pic98]: images/98.png
[pic138]: images/138.png

# 7b Removing Your Deployment <a name="removing_deployment"></a>


1\. In the Platform Navigator, select your App Connect Dashboard instance.

![alt text][pic90]

![alt text][pic100]

2\. Click on **Servers**.

![alt text][pic102]

3\. Click on the triple-dot icon on your **<span style="color: red">username</span>-orders** Server.

![alt text][pic103]

4\. Click on **Delete**. 

![alt text][pic104]

5\. Confirm the deletion by clicking **Delete**. This will delete the pod associated with your deployment.

![alt text][pic105]

6\. We will also want to delete the BAR file that was deployed. Click on the **BAR files** icon on the left-hand pane in the App Connect Dashboard.

![alt text][pic106]

7\. Click on the triple-dot icon on your **<span style="color: red">username</span>_orders** bar file.

![alt text][pic107]

8\. Click on **Delete BAR**.

![alt text][pic108]

[pic99]: images/99.png
[pic100]: images/100.png
[pic101]: images/101.png
[pic102]: images/102.png
[pic103]: images/103.png
[pic104]: images/104.png
[pic105]: images/105.png
[pic106]: images/106.png
[pic107]: images/107.png
[pic108]: images/108.png

# 7c Removing the MQ Queue <a name="removing_queue"></a>

1\. In the Platform Navigator, navigate to your Queue Manager.

![alt text][pic109]

![alt text][pic110]

2\. Click on **Manage CMQGR<span style="color: red">username number</span>**.

![alt text][pic111]

3\. Click on the triple-dot icon on the **ORDERS** queue.

![alt text][pic112]

9\. Click on **Clear queue**.

![alt text][pic113]

10\. To confirm the deletion of messages, click **Clear queue**.

![alt text][pic114]

11\. Click on the **ORDERS** queue.

![alt text][pic115]

12\. Click on the **Actions** icon.

![alt text][pic116]

13\. Click **Delete queue**.

![alt text][pic117]

14\. To confirm the deletion of the queue, click **Delete**.

![alt text][pic118]

[pic109]: images/109.png
[pic110]: images/110.png
[pic111]: images/111.png
[pic112]: images/112.png
[pic113]: images/113.png
[pic114]: images/114.png
[pic115]: images/115.png
[pic116]: images/116.png
[pic117]: images/117.png
[pic118]: images/118.png
[pic139]: images/139.png

---

# Congratulations!
You have completed the IBM Cloud Pak for Integration Operations Dashboard lab.

[Return to main lab page](../sfdc-experience.md)
