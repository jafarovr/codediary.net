---
layout: post
status: publish
published: true
title: What are Content, Subscription and Differential Charging?
date: '2017-03-23 22:08:45 +0000'
date_gmt: '2017-03-23 18:08:45 +0000'
permalink: /posts/what-are-content-subscription-and-differential-charging/
categories:
- Random
- Telecom
tags: []
---
## Content Charging

Content or value based charging is to charge an end-user based on the variable value of a used service rather than on time used or flat rates. This can be used, for example, when downloading music video clips. Different contents can be charged at different rates, i.e. music contents 30 TK/minute, news contents 20 TK/minute etc.

### Subscription Charging

Subscription charging is applied for regular subscription based services. Users can subscribe for a service for a specific modality. Here Subscription Based Modality means users can subscribe for a service monthly/bi weekly/weekly/daily and charge accordingly. Different modalities have different charging rates. When the user is subscribed to a service, the system treats the user as a registered user and sends service specific content to that user and the user can get the service.


### Differential Charging

When a user calls a Service and gets different service nodes at different rates, it is Differential Charging.
For example, ifthe Subscriber calls 1234 and presses 1, the rate for this node is higher. If subscriber presses 2, the rate is lower. This is Differential Charging. This rate can be available for the whole day; it can also be for any specific time of the day.
Based on Registered user group, a different rate is applicable as compared toan Un-registered group. For example, user calls 2002 (sports portal) and those who are registered arecharged 1.5 Tk and those who are unregistered arecharged 2 TK.
Period is a specific duration for which a different rate is applicable. Say during Ramadan, calling to Hazz & Surah Service, subscribers will be charged less than other periods.
## Types of charging

Users can be Prepaid or Postpaid. Based on user type, charging is different.

### Prepaid

Prepaid is generally pay first then use. The charging system needs to know the user's account balance and it will need to update the account in real-time. This is often referred to as "Online-Charging".

### Postpaid

Postpaid is use first then pay. Postpaid credit risk is generally managed through credit limit. The charging system only needs to collect the usage data for processing it at the end of the month. As no real-time account updating is needed, this is often called "Offline-Charging".
The charging system aggregates the usage data of Postpaid users (CDR) into files. These files can be transmitted in real-time or in batch to the billing domain for further processing. Postpaid CDRs are often processed quite frequently (around 5-10 minute interval) to avoid credit risk and better management of control function.
## General charging flow for mobile services

When the User requests any Basic Service to MSC, MSC first takes Authorization from Registration Server (HLR), based on the Service, different charging flow is occurs.

### Voice/Video

If requested Service is a Voice call and that Voice call is a Prepaid Voice Call, Prepaid Charging System (IN) is called for this Prepaid charging. If the Voice call is a Postpaid Voice Call, CDR is generated in this case. This CDR will later use for Postpaid charging.

### Data

For Data Service, it comes to WAP/Internet Gateway then request goes to AAA. AAA goes to IN for Prepaid Subscribers or generate CDR for Postpaid Subscriber.

### SMS

When requested Service is SMS, it will come to SMSC. When charging request comes to SMSC, it will charge IN for Prepaid Users and generate CDR for Postpaid Users.
## Charging of Value Added Services (VAS)

All value added services go through any one of the basic services. Thismeans that for all the Value Added Services, a basic charging process is already active through Prepaid or Postpaid Systems. For Value Added Services, the core nodes like MSC, SMSC etc interacts with the Value Added Service Applications. For additional charging, VAS Application generates CDR for Postpaid Users or request IN for Prepaid Users. First charging is done by MSC/SMSC/WAP or Internet Gateway and if any additional charging is needed, this is handled by the VAS Application.

### Why Charging Gateway is required

Content Based Charging, Subscription Based Charging or Differential Charging - when applied for Service/Application is not possible through this general charging flow. Because MSC/Core Charging Systems do not have enough information based on which these types of charging can be done.
If application requires further charging - it has to interact with Charging System (Prepaid & Postpaid) directly. But often Core Charging Systems are not exposed to the VAS Applications directly. There is a middleware application which sits in between. This application is known as Charging Gateway. This handles all Charging Requests from VAS Applications and processes them towards the core charging system.

## Charging through CGW

Charging Gateway provides the interface to transfer charging information collected from VAS Applications to the Billing/IN System. It also acts as storage buffer for real-time CDR collection and performs consolidation of CDRs and pre-processing of CDR Fields before sending to Billing system.
Third party Application/Service requests for charging to CGW. CGW verifies the requester through AAA. Charging Database stores different rate for Services, based on that CGW submits request to Billing Interface. CGW then sends back the charging result to requesting third party application and writes CDR into DB.
CGW differentiates Prepaid and Postpaid users, based on that IN is called or CDR is generated.
## Types of charging through CGW


### Event Based

For Content, Registration, SMS or any other event, the charging is done through CGW. This is a one-time charging. For Registration, using MSISDN and specific Service ID, charging request is formed and CGW is called. Based on received response, the user is registered (for successful charging).

### Subscription and Renewal

This is a service based on subscription model and its regular renewal is a very common scenario. Subscriptions can be free for a certain configurable period. The subscriber"s service renewal, termination of that service, and the change to another service is possible based on service charge and service logic.
There aredifferent Subscription Statuses, such as the User beingRegistered, Deregistered, Renewed, In Grace Period, Renew Failed, and Downgraded (Switch to different fee cycle after completing existing cycle) status.

### Dynamic Downgrading through Micro Charging

For subscription based modality, Charging Gateway generally supports monthly/bi weekly/weekly/daily fee management. The user can be registered based on automatic fallback subscription method, for example if the subscriber account has inadequate balance for monthly fee, it will automatically shift to weekly fee management. This feature is known as Dynamic Downgrading through Micro Charging. This helps subscriber retention in a major way.

### Grace Period

To keep subscriber in service, there is an option for Grace Period. If any service modality has a Grace Period option for any specific modality (e.g. monthly) and the user does not have enough balance while renewing the subscription, the user can use the service for the specified Grace Period. During the Grace Period, if the user has balance again, he/she will be charged for the amount starting from the renewal due date. In this way, the user is not de-registered from the service. In case of insufficient balance after the Grace Period, the user cannot use the service and is de-registered from the service.

### Win Back Renewal Failed Subscribers

Subscription Service has Retry Period option where De-registered Subscribers (for insufficient balance) can be registered back into the service. This is not available for the users who de-register themselves. These Renewal Failed users are attempted for charging if the Retry Period option is ON. If charging is successful within this Retry Period, this user is updated as Registered. If charging fails, the user is De-registered from Renewal Failed status.
Based on type of Subscription, Charging request is sent to CGW and based on received response, Subscriber status is updated. CGW finds charging node based on subscriber type. If subscriber is postpaid, CGW communicates with Postpaid Billing (Mostly Generate CDR). If subscriber is prepaid, CGW communicates with IN.

### Session Based

When any Voice/Video session is started, CGW sends a query to the relevant IN platform to verify if there is sufficient balance available to initiate the session. If there is insufficient balance, CGW rejects the session. Based on the response, CGW authorizes the VAS session initiation or event. Afterwards the initial allowed session, subsequent VAS session charging requests, are sent again and based on the received response, the VAS Application allows the service to continue. Upon closure of the session, CGW communicates with the IN platform to commit the final charges.

## Considerations for Charging

### TPS

Transaction per Second (TPS) is an important factor for Charging. Since online charging is handled, the TPS for Charging Gateway needs to be considered and calculated. CGW TPS is affected by the Numberof Session Based Charging during peak hours and Number of Registration/Renewal Requests during peak hour.

### CDR format

CDR should be generated for all calls regardless of charging node, successful and unsuccessful call with result code. Distinguishable differential airtime charging for postpaid users for same short code are to be indicated in CDR as well.
**Following information MUST cover in CDR:**

* Calling/source number
* Called/destination number
* Incoming or Outgoing Call
* Specific service (e.g. Voice Portal)
* Airtime (Session) or Event Based (Registration/Content based)
* Airtime charging rate ID
* Start time of the session
* End time of the session
* Charged amount by Charging Gateway (not MSC Charging)

If a single call has multiple rates, a final master CDR has to be provided to reconcile.
Field separator and Row separator of CDR file can be defined.

### IN Interface

CGW interacts with IN interface for Event based charging, Session Based charging, Differential charging for Prepaid Users. It is generally custom built for every different IN Vendor.

### CGW Interface

There are CGW TCP Interface and Web Services Interface. Application connects to CGW TCP interface or web services.

### Renewal

Users subscribed to different services require regular renewal and the Renewal Application does this. It attempts to charge the subscriber after a certain interval based on the subscription plan (weekly, monthly etc). In case of insufficient balance, users can be downgraded or there can be Grace Period for some services.
There is a Notification option. For example,when a user is de-registered from the system for not having enough balance, there is a SMS notification. Conceptually, Notification sending is possible at any State Change.

Source: [http://ssd-tech.com/knowledge-base/telecom-platforms/content-subscription-and-differential-charging/](http://ssd-tech.com/knowledge-base/telecom-platforms/content-subscription-and-differential-charging/)