# IBM App Connect Enterprise

## App Connect Designer Salesforce

[Return to main lab page](../sfdc-experience.md)

---

# Table of Contents 
- [1. Introduction](#introduction)
  * [Pre-Lab: Gathering your Salesforce Credentials](#pre_lab)
- [2. Setup connection to Smart connectors for this lab](#Setup_connections)
- [3. Create a Designer Event Flow in CP4I for Salesforce ](#create_a_designer_flow)
- [4. Testing the Event flow ](#test_a_designer_flow)
- [5. Deploying Your Designer Flow to App Connect Dashboard  ](#deploy_a_designer_flow)
    
---

# 1. Introduction <a name="introduction"></a>

In an event-driven flow you identify an event that can occur in your source application and actions that can be performed in one or more target applications. The flow is triggered when the event occurs.
* The purpose of this LAB is to show how to create an event-driven flow to identify when new Salesforce Account Records are created. 

When prompted to log in to CP4I use the username and password provided to you for this lab.   

 ## Pre-Lab: Gathering your Salesforce Credentials <a name="pre_lab"><a/>

For this lab, you will need to collect your Salesforce account details: Username, Password, Client Secret, and Client ID. You can learn how to obtain these values from this link:

 https://developer.ibm.com/integration/docs/app-connect/how-to-guides-for-apps/use-ibm-app-connect-salesforce/


# 2. Setup connection to Smart connectors for this lab.<a name="Setup_connections"></a>

In this section we use App Connect Designer to create a flow that is triggered when an event occurs on Salesforce records.

1\. Use the URL that was provided to you for the cluster you will be using.   Select the Enterprise LDAP

![alt text][pic0]

2\. When prompted use the username and password provided to you for this lab. In this example we are using wedge15.

![alt text][pic1]

3\. You will now be on your home page and it the upper left it will show your login name.   Under Integrations click on the App Connect Designer link to take you to the designer dashboard.

![alt text][pic2]

4\. In the upper left always make sure you are in the correct namespace.  Select the tab on the left to open the Catalog screen.

![alt text][pic3]

5\. Now scroll down to the Salesforce connector and click on the Connect button.  We are setting up our Salesforce connector here first but this also can be done with in the flow as you are building it. 
* **Note:** if you already have a Account connected for Salesforce skip to step 8

![alt text][pic4]

6\. Fill in the Salesforce connection info that you have from the pre-lab section at the beginning of this lab. 

![alt text][pic5]

7\. You will see the events and actions availble with this connector. Also you can change the Account Name to something more meaningful to you by clicking on the 3 dots next to the Account name. 

![alt text][pic6]

8\. For the this lab we will use MQ as our target applications.  We will need to login to the OCP console for your user id to set up the connection and security for the MQ QMgr.  
* **Note:** In this example we are using the wedge cluster and wedge15 id.  Make sure you use the url and sign on info provided to you.
* This is the OCP URL for the wedge cluster --> https://console-openshift-console.apps.wedge.coc-ibm.com/ 

![alt text][pic6a]

9\. Now we will select Pods under Workloads.   Make sure you select the correct Project in the top menu. In this example we are logged in as wedge15 so we select the wedge15 Project.  Scroll down to find your qmgr pod running and click on it. 

 ![alt text][pic6b]

10\. In the Pod Details screen you will see the Pod name.  Make sure to save the highlighted part since that is the service name and will be used to create the hostname.   Next click on the Terminal tab.

![alt text][pic6c]

11\. We now have a terminal window open into the pod that is running our QMgr.  Enter the command **runmqsc** .  This will sign you into the default QMgr. 

![alt text][pic6d]

12\. Now we will run the following commands to disable security on the QMgr.

* ALTER QMGR CHLAUTH (DISABLED)
* ALTER AUTHINFO(SYSTEM.DEFAULT.AUTHINFO.IDPWOS)  AUTHTYPE(IDPWOS) CHCKCLNT(NONE)
* REFRESH SECURITY TYPE(CONNAUTH)

![alt text][pic6e]

13\. Now go back to the tab where you signed into CP4I and click on the top IBM Automation to get to the home page.   Then right click your qmgr and open in a new tab.  

![alt text][pic6f]

14\. Now save the name of your QMgr and click on the tile to open that Qmgr dashboard.

![alt text][pic6g]

15\. Now we will create a new Queue to use in our new flow

![alt text][pic6h]

16\. We will be creating a new Local queue.  

![alt text][pic6i]

17\. Enter the name for the Queue.  In the example we used "SALESFORCE.ACCOUNT.EVENT" then click Create.  

![alt text][pic6j]

16\. Now go back to the tab with the App Connect Designer and select the catalog on the left menu. Scroll down to IBM MQ and fill in the connection info.  
* Enter the QMgr name
* For the QMgr host we will use the <service><namespace>.svc
* Port is 1414
* Channel SYSTEM.DEF.SVRCONN

![alt text][pic6k]

# 3. Create a Designer Event Flow in CP4I for Salesforce  <a name="create_a_designer_flow"></a>

1\. Click on the App Connect Designer dashboard icon:

![alt text][pic7]

2\. Select from the New drop down to create a new Event-driven flow:  

![alt text][pic8]

3\. First thing enter a name that identifies the purpose of your flow, for example:
Salesforce New Account Events. 

![alt text][pic9]

4\. Click on the first Blue + and scroll down to Salesforce connector.   Select New account under Accounts.  

![alt text][pic10]

5\. Now we show the Salesforce connector.  Click on the next Blue + and scroll down to IBM MQ.  Select the Put message action.

![alt text][pic11]


6\. Now enter the Queue Name you created for MQ.  We used SALESFORCE.ACCOUNT.EVENT
* For Message type select Text
* For the Message payload click on the insert mapping and you will have a list of Suggested mappings.  Scroll down and select the Account Name.

![alt text][pic12]

7\. Now we are ready to start the flow.  In the upper right corner click on the 3-dots and start the flow.  

![alt text][pic13]

8\. Now return to the tab where you have the MQ console opened.  Click on manged and you should see your new Queue.  It should be showing 0/5000 which means it has no messages and can handle 5000 messages. 
* This is where we will check when we test this flow. 

![alt text][pic14aa]


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

 We will now test the new Event flow.  We will log into our Salesfoce account and create a new Account.   

1\. Open a new tab and go to https://www.salesforce.com/  login with your Salesforce login.  

![alt text][pic22]
![alt text][pic22a]

2\. In the left menu seach for Accounts and select that.   

![alt text][pic23]


3\. There are two methods for creating a new Account.  Select one to get the New Account screen

![alt text][pic24]


4\. Fill-in the new Account info.  You will need a Account Name and website and then click  **Save**

![alt text][pic25]

5\. Now we will go back to the MQ console to check for messages on our Queue from this event.   

![alt text][pic26]

5\. You will now see that it shows 1/5000 which means we have a new message.  Click on the Queue to see the message.  Remeber in the mapping of the flow we put the Account Name in the Message body so you should see that here. 

![alt text][pic27]

[pic22]: images/22.png
[pic23]: images/23.png
[pic24]: images/24.png
[pic25]: images/25.png
[pic25a]: images/25a.png
[pic26]: images/26.png
[pic27]: images/27.png


# 5. Deploying Your Designer Flow to App Connect Dashboard <a name="deploy_a_designer_flow"></a>

As in other labs we can export our Designer flow as a bar file and deploy to App Connect Dashboard on Cloud Pak for Integration. We will not do that in this lab.   


[Return to main lab page](../sfdc-experience.md)
