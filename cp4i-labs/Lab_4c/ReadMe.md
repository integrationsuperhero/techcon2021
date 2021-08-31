# IBM App Connect Enterprise

## App Connect Designer ServiceNow

[Return to main lab page](../sn-experience.md)

---

# Table of Contents 
- [1. Introduction](#introduction)
  * [Pre-Lab: Gathering your ServiceNow Credentials](#pre_lab)
- [2. Setup connection to Smart connectors for this lab](#Setup_connections)
- [3. Create a Designer Event Flow in CP4I for ServiceNow ](#create_a_designer_flow)
- [4. Testing the Event flow ](#test_a_designer_flow)
- [5. Deploying Your Designer Flow to App Connect Dashboard  ](#deploy_a_designer_flow)
    
---

# 1. Introduction <a name="introduction"></a>

In an event-driven flow, you identify an event that can occur in your source application and actions that can be performed in one or more target applications. The flow is triggered when the event occurs.
* The purpose of this LAB is to show how to create an event-driven flow to identify when new ServiceNow User IDs Records are created. 

When prompted to log in to CP4I use the username and password provided to you for this lab.   

 ## Pre-Lab: Gathering your ServiceNow Credentials <a name="pre_lab"><a/>

For this lab, you will need to collect your ServiceNow account details: URL, Username, Password, Client Secret, and Client ID. You can learn how to obtain these values from this link:

 https://developer.ibm.com/integration/docs/app-connect/how-to-guides-for-apps/use-ibm-app-connect-ServiceNow/


# 2. Setup connection to Smart connectors for this lab.<a name="Setup_connections"></a>

In this section, we use App Connect Designer to create a flow that is triggered when an event occurs on ServiceNow records.

1\. Use the URL that was provided to you for the cluster you will be using.   Select the Enterprise LDAP.

![alt text][pic0]

2\. When prompted, use the username and password provided to you for this lab. In this example, we are using wedge15.

![alt text][pic1]

3\. You will now be on your home page and it the upper left it will show your login name.   Under **Integrations**, click on the App Connect Designer link to take you to the designer dashboard.

![alt text][pic2]

4\. In the upper left, always make sure you are in the correct namespace.  Select the tab on the left to open the Dashboard screen.

![alt text][pic3]

* **NOTE:** If you already have a MQ connect from other labs skip to item 10

5\. For the this lab, we will use MQ as our target application.  We will need to login to the OCP console with your user id to set up the connection and security for the MQ QMgr.  
* **Note:** In this example, we are using the wedge cluster and wedge15 id.  Make sure you use the url and sign on info provided to you.
* This is the OCP URL example for the wedge cluster --> https://console-openshift-console.apps.wedge.coc-ibm.com/ 

![alt text][pic6a]

6\. Now we will select **Pods** under **Workloads**.   Make sure you select the correct Project in the top menu. In this example, we are logged in as wedge15 so we select the wedge15 Project.  Scroll down to find your qmgr pod running and click on it. 

 ![alt text][pic6b]

7\. In the **Pod Details** screen, you will see the Pod name.  Make sure to save the highlighted part since that is the service name and will be used to create the hostname.   Next click on the **Terminal** tab.

![alt text][pic6c]

8\. We now have a terminal window open into the pod that is running our QMgr.  Enter the command **runmqsc** .  This will sign you into the default QMgr. 

![alt text][pic6d]

9\. Now we will run the following commands to disable security on the QMgr.

* ALTER QMGR CHLAUTH (DISABLED)
* ALTER AUTHINFO(SYSTEM.DEFAULT.AUTHINFO.IDPWOS)  AUTHTYPE(IDPWOS) CHCKCLNT(NONE)
* REFRESH SECURITY TYPE(CONNAUTH)

![alt text][pic6e]

10\. Now go back to the tab where you signed into CP4I and click on the top **IBM Automation** to get to the home page.   Then right click your qmgr and open in a new tab.  

![alt text][pic6f]

11\. Now save the name of your QMgr and click on the tile to open that Qmgr dashboard.

![alt text][pic6g]

12\. Now we will create a new Queue to use in our new flow

![alt text][pic6h]

13\. We will be creating a new **Local** queue.  

![alt text][pic6i]

14\. Enter the name for the Queue.  In the example, we used **SERVICENOW.USER.EVENT** then click **Create**.  

![alt text][pic6j]

15\. Now go back to the tab with the App Connect Designer and select the **Catalog** on the left menu. Scroll down to **IBM MQ** and fill in the connection info.  
* Enter the QMgr name
* For the QMgr host we will use the \<service\>.\<namespace\>.svc
* Port is 1414
* Channel SYSTEM.DEF.SVRCONN

![alt text][pic6k]

# 3. Create a Designer Event Flow in CP4I for ServiceNow  <a name="create_a_designer_flow"></a>

1\. Click on the App Connect Designer **Dashboard** icon.

![alt text][pic7]

2\. Select from the New drop down to create a new **Event-driven flow**. 

![alt text][pic8]

3\. First thing, enter a name that identifies the purpose of your flow.  For example, ServiceNow New User Events. 
* Click on the first blue **+** and scroll down to **ServiceNow** connector.   Select **Connect to ServiceNow** to see more.  

![alt text][pic9]

* **Note:** If you already have a Account connected for ServiceNow skip to step 8

4\. The connect to ServiceNow form will show.   Fill in with the info from the pre-lab section at the beginning of this lab.  Click **Connect**.   

![alt text][pic9a]

5\. If you see the following error ignore and close it.  

![alt text][pic9b]

6\. Now we will select the action we will be listening for.  In this case, it will be whenever a New system user is created.   Click on that.

![alt text][pic9c]

7\. Click on the second blue **+** and scroll down to **IBM MQ** connector.   Select **Put message on a queue**.  

![alt text][pic9d]

8\. Now you will see the fields that can be mapped for the target application.  
* Enter the ServiceNow Queue we created. 
* Select **Text** for the message type.
* And for message payload you can start typing.  For example, if you  type sys, you will see all the possible mapping options.   Select the **System User id**
* We can also add additional info to the payload like the Name, email, etc.

![alt text][pic9e]

9\. Now you can see what will be put to the Queue payload for this event.  Now we are ready to start the flow.  In the upper right corner click on the 3-dots and start the flow.  

![alt text][pic9f]

[pic0]: images/0.png
[pic1]: images/1.png
[pic2]: images/2.png
[pic3]: images/3.png
[pic4]: images/4.png
[pic5]: images/5.png
[pic6]: images/6.png
[pic6a]: images/6a.png
[pic6b]: images/6b.png
[pic6c]: images/6c.png
[pic6d]: images/6d.png
[pic6e]: images/6e.png
[pic6f]: images/6f.png
[pic6g]: images/6g.png
[pic6h]: images/6h.png
[pic6i]: images/6i.png
[pic6j]: images/6j.png
[pic6k]: images/6k.png
[pic7]: images/7.png
[pic8]: images/8.png
[pic9]: images/9.png
[pic9a]: images/9a.png
[pic9b]: images/9b.png
[pic9c]: images/9c.png
[pic9d]: images/9d.png
[pic9e]: images/9e.png
[pic9f]: images/9f.png
[pic10]: images/10.png
[pic11]: images/11.png
[pic12]: images/12.png
[pic13]: images/13.png
[pic14]: images/14.png
[pic15]: images/15.png
[pic16]: images/16.png
[pic17]: images/17.png
[pic18]: images/18.png
[pic19]: images/19.png
[pic20]: images/20.png
[pic21]: images/21.png


# 4 Testing the Event flow <a name="test_a_designer_flow"></a>

 We will now test the new Event flow.  We will log into our ServiceNow developer account and create a new User.   

1\. Open a new tab and go to https://developer.servicenow.com/dev.do  Follow the steps to login with your ServiceNow login.  

![alt text][pic22]
![alt text][pic22a]
![alt text][pic22b]

2\. Once logged into your ServiceNow developer instance, click **Start Building**.    

![alt text][pic22c]


3\. You are now in the management screen.   We will now add a new user which will trigger our AppConnect Event flow.   
*  In the search, enter **user admin**.  You should see **Users** under **User Administration**.  Click on **Users** and that screen will be displayed.  

![alt text][pic22e]


4\. On the top menu, click the **New** button and fill-in the new record info.  When done in the upper right corner, click the **Submit** button. 
* **Note:** The main fields will be the User ID, First and Last Name, and email since those are the fields mapped in the flow.  

![alt text][pic22f]

![alt text][pic22g]

5\. Now we will go back to the MQ console to check for messages on our Queue from this event.   

![alt text][pic23]

![alt text][pic23a]

5\. We will now see that you have a new message on the SERVICENOW queue. Click on the Queue name to look at the message.

![alt text][pic23b]

5\. You will now see the message.  Under the **Application data**, you will see the payload info that we mapped in the flow.   Notice that the first thing is the SystemUserID.  This is a unique id added to the record by ServiceNow when creating a new entry. 

![alt text][pic23c]

[pic22]: images/22.png
[pic22a]: images/22a.png
[pic22b]: images/22b.png
[pic22c]: images/22c.png
[pic22d]: images/22d.png
[pic22e]: images/22e.png
[pic22f]: images/22f.png
[pic22g]: images/22g.png
[pic23]: images/23.png
[pic23a]: images/23a.png
[pic23b]: images/23b.png
[pic23c]: images/23c.png


# 5. Deploying Your Designer Flow to App Connect Dashboard <a name="deploy_a_designer_flow"></a>

As in other labs, we can export our Designer flow as a bar file and deploy to App Connect Dashboard on Cloud Pak for Integration. We will not do that in this lab.   


[Return to main lab page](../sn-experience.md)
