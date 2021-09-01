---
title: APIC Dev Jam Lab 2 - The Developer Portal Experienace
---

In this lab, we will take the API created in Lab 1 and publish it to a
Developer Portal, ready for consumption by app developers. We will begin
by creating a new catalog and configuring the developer portal for our
Inventory product. We will then define a new plan in the product and
publish to our new developer portal.

In this tutorial, you will explore the following key capabilities:

-   Configure the Developer Portal and publish the APIs.

-   Create a Portal Account.

-   Create App and Subscribe to Plan.

-   Test API in the Developer Portal

## APIC Dev Jam Series

The APIC Dev Jam Series is a hands-on workshop with lab exercises that
walk you through designing, publishing, and securing APIs. This workshop
is for API developers, architects, and line of business people who want
to create a successful API strategy. There are 8 labs and each is 30
minutes long. Make sure you choose enough time in your reservation to
get through all the labs! 

**NOTE: This demo environment contains a
full API Connect installation in Cloud Pak for Integration. The login
information to the APIC cluster will be sent in a separate email when
you reserve the instance. Use Google Chrome, Firefox or Microsoft Edge
to access the cluster using the credentials supplied. Make sure you
login using API Manager User Registry not Common Services
registry.**

Prerequisites: Labs 1

## Generate the developer portal

A developer portal for the sandbox catalog was already configured in
this environment. A consumer account "ibmuser" was also created to use
the Developer Portal server. In this section, login to Developer Portal
using the credentials provided in email.

### Login to API Connect Developer Portal

1.  API Connect Developer Portal provides consumers access to API
    Catalog information to explore and test APIs, register Applications
    and subscribe to Plans. Portal Administrator can customize the looks
    and feel to their organizational specifications. The default
    Developer Portal looks like below:

    ![](images/Step1_1.png)

2.  Some products are visible to all users without an account depending
    on the Product visibility setting. Additional options are available
    when you login to the Portal Server.

    Click **Sign in** to login to the portal

3.  Login to portal user using the username and password supplied in the
    email.

    ![](images/Step1_3.png)

4.  After successful login, you will see a Get Started page. Proceed to
    create a new Test application.

 

 ### Register a test app

API Connect enforces entitlement rules to ensure that consumers are
allowed to access the APIs that are being requested. The instructions
below will guide you through registering your consumer application and
subscribing it to an API Product.

#### Create A New Consumer Application


1.  Click on Apps
    ![](images/Step2_1.png)

2.  Click  *Create a new App*.

    ![](images/Step2_2.png)

3.  Give your application the title `IBM Consumer Application` and then
    click `save`.

    ![](images/Step2_3.png)


4. When your consumer application is registered in the IBM API Connect
system, it is assigned a unique set of client credentials. These
credentials must be provided on each API request in order for the system
to validate your subscription entitlements. The Client ID can be retireved anytime but the Client Secret can only be retrieved at this time.

![](images/Step2_4.png)

5.  Click the OK

### Subscribe to the API Product

At this point, your registered consumer application has no entitlements.
In order to grant it access to an API resource, you must subscribe to a
Product and Plan.

1.  Click `API Products` at
    the top of the screen.

2.  Click the `inventory auto product
    1.0.0` product.

    ![](images/Step3_1.png)

3.  Scroll down to the Plan and select it.

![](images/Step3_2.png)

4.  **Select your application**, 
      ![](images/Step3_4.png)

5. Click **Done**.
      ![](images/Step3_6.png)

 ## Test the API

The API Connect Developer Portal allows consumers to test the APIs
directly from the website. This feature may be enabled or disabled
per-API.


1.  Click `API Products` at
    the top of the screen.


2.  Click the `inventory auto product 1.0.0` product.

        ![](images/Step3_1.png)

3.  Open the **inventory 1.0.0** API to browse the API definition.

    ![](images/Step4_3.png)

4.  Click the `GET /Items` operation
    on the left palette.

    In the right column, you will find information about the request
    parameters and links to the response schemas.

5.  Click the `Try it` tab.

    ![](images/Step4_5.png)

6.  If you only have one application registered, it will be
    automatically selected in the `Client
    ID` drop-down menu. If you have more
    than one, select the application which is subscribed to this API
    Product.
    ![](images/Step4_7.png)

7.  Paste your `Client Secret` into the
    provided field.

8.  Click the `Send` button
    to invoke the API.

8.  Scroll down to see the call results.

**Note: If running for the first time, you may see Code: 0 No response
received. Causes include a lack of CORS support on the target server,
the server being unavailable, or an untrusted certificate being
encountered. Clicking the link below will open the server in a new tab.
If the browser displays a certificate issue, you may choose to accept it
and return here to test again.**

## Summary

You completed the APIC Dev Jam Lab 2 - The Developer Portal Experience.
Throughout the tutorial, you explored the key takeaways: 

-   Configure Developer Portal and publish the APIs.

-   Create a Portal Account. 

-   Create App and Subscribe to Plan

-   Test API in Developer Portal. 


Continue the APIC Dev Jam! Go to [APIC Dev Jam Lab 3 - The Developer Portal Experience](/APICDevJam/Lab3) 
