---
title: Wazza documentation

language_tabs:
  - shell
  - objective_c
  - java

toc_footers:
  - <a href='https://www.wazza.io/register'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Introduction

> Code will appear here!

The Wazza API is organized around REST. Our API is designed to have predictable, resource-oriented URLs and to use HTTP response codes to indicate API errors. We use built-in HTTP features, like HTTP authentication and HTTP verbs, which can be understood by off-the-shelf HTTP clients. JSON will be returned in all responses from the API, including errors (though if you're using API bindings, we will convert the response to the appropriate language-specific object).

We have language SDKs in Shell, objective_c and Java. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

## First time use

Head on to [login] page and enter your credentials.
You will be redirected to the Dashboard page and prompted to add a new Mobile Application.
After this, Wazza will start to crunch data and it will soon be available here.
If you click in any of the available applications, you will be able to see more KPIs.
In the same way, if you click any of these KPIs, you will get access to detailed information including charts.

# Registration

Head on to the [register] page to register your account. If you have a promocode, use it and you will get early access.
You will receive an e-mail whenever your account is activated and you may begin to use it as you will.

# Authentication

You authenticate to the Wazza API by providing your secret API key in the request. You can manage your API keys from your account. Your API keys carry many privileges, so be sure to keep them secret!
Authentication to the API occurs via HTTP Basic Auth. Provide your API key as the basic auth username. You do not need to provide a password.
All API requests must be made over HTTPS. Calls made over plain HTTP will fail. You must authenticate for all requests.


> To authorize, use this code:

```objective_c
[WazzaAnalytics initWithCredentials:@@"Company Name" :@@"App Name" :@@"API token"];
[WazzaAnalytics setDelegate:self];
```

```java
// Keep in mind you will still need to open and close sessions. Further information can be found in SDKs Integration documentation.
Wazza wazza = Wazza.init(this, "API_KEY", "Application Name", "Company Name");
```

```shell
# With shell, you can just pass the correct header with each request
curl "https://www.wazza.io/api/auth/"
  -H "SDK-TOKEN: API_KEY"
```

> Make sure to replace `API_KEY` with your API key.


### HTTP Request

`POST https://www.wazza.io/api/auth/`

The sent header looks like this:
`SDK-TOKEN: API_KEY`

Additionally, an optional header can be sent if the user exists or not. It is up to the sender to store that information, which can be done analysing the result of API call. The goal of this is to reduce the number of database requests for user existence. A boolean result is sent if a mobile user was created or not.

This header looks like this:
`X-UserExists: *optional*`

### Success Response:

* **Code:** 200 OK
* **Content:**
  ```json
    {
      "result": "[boolean]"
    }
  ```

### Error Response:

* **Code:** 404 NOT FOUND
* **Content:** `{}`


<aside class="notice">
You must replace <code>API_KEY</code> with your personal API key.
</aside>


# Sessions

## Save session
Saves information regarding one or more sessions.

> To record a session use this code:

```objective_c
  [[WazzaAnalytics sharedInstance] endSession];
```

```java
  wazza.sessionClose();
```

> The sent JSON is structured like this:

```json
    {
      "userId": "[string]",
      "applicationName": "[string]",
      "companyName": "[string]",
      "startTime": "[string]",
      "endTime": "[string]",
      "deviceInfo": {
        "osName": "[string]",
        "osVersion": "[string]",
        "deviceModel": "[string]"
      },
      "location": "#optional"{
        "latitude": "[double]",
        "longitude": "[double]"
      },
      "purchases":[
        {
          "[string]"
        },
        "..."
      ]
    }
```

### HTTP Request

`POST https://www.wazza.io/api/session/new/`

### Success Response:

* **Code:** 200 OK
* **Content:**
  ```json
    {}
  ```

### Error Response:

* **Code:** 404 NOT FOUND
* **Content:**
  ```json
    {}
  ```


<aside class="warning">
Remember — the API Key must always be included!
</aside>

# Payment

## Save Payment

> In order to save a payment, you need to:

```java
  wazza.purchase("SKU");
```

```objective_c
  [[WazzaAnalytics sharedInstance] makePurchase:@"PRODUCT_ID"];
```

Saves information regarding one payment.
Depending on the payment system, the sent JSON structure will change. In particular, if the payment was made via PayPal, some attributes are added to common structure. In-App Purchase payments have no extra attributes.

> Common payment JSON structure:

  ```json
    {
      "paymentSystem": "[int]",
      "id": "[string]",
      "itemId": "[string]",
      "userId": "[string]",
      "price": "[double]",
      "time": "[Date]",
      "deviceInfo": {
        "osName": "[string]",
        "osVersion": "[string]",
        "deviceModel": "[string]"
      },
      "sessionId": "[string]",
      "success": "[boolean]"
    }
  ```
  
> PayPal payment JSON structure:
  
  ```json
    {
      "currencyCode": "[string]",
      "shortDescription": "[string]",
      "intent": "[string]",
      "processable": "[boolean]",
      "responseID": "[string]",
      "state": "[string]",
      "responseType": "[string]",
      "quantity": "[int]"
    }
  ```

### HTTP Request

`POST https://www.wazza.io/api/purchase/`

### Success Response:

* **Code:** 200 OK
* **Content:**
  ```json
    {}
  ```

### Error Response:

* **Code:** 404 NOT FOUND
* **Content:**
  ```json
    {
    "error": "[string]"
    }
  ```

## PayPal payment verification

Sends a PayPal payment for verification.

> The sent JSON structure is:

```json
    {
      "responseID": "[string]",
      "price": "[double]",
      "currencyCode": "[string]"
    }
```

### HTTP Request

`POST https://www.wazza.io/api/payment/verify/`

### Success Response:

* **Code:** 200 OK
* **Content:**
  ```json
    {}
  ```

### Error Response:

* **Code:** 404 NOT FOUND
* **Content:**
  ```json
    {
    "error": "[string]"
    }
  ```

# Analytics

The SDKs are designed to feed information into our engine and not to collect it; therefore, any analytics request shall be made using the REST API directly.
For each metric, total and detailed queries can be made; the difference boils down to the url the request is sent.

Contact us for a fully detailed information on the available metrics.

> You may find an example for Lifetime Value below:

```shell
curl "https://www.wazza.io/api/analytics/ltv/total/Wazza/Demo/19-07-2015/26-07-2015"
  -H "SDK-TOKEN: API_KEY"
```

> which returns something like:

```json
  {
     "value":0.0,
     "delta":0.0,
     "previous":0.0,
     "platforms":[
        {
           "platform":"Android",
           "value":0.0,
           "delta":0.0,
           "previous":0.0,
           "paymentSystems":[
              {
                 "system":1,
                 "value":0.0,
                 "previous":0.0,
                 "delta":0.0
              },
              {
                 "system":2,
                 "value":0.0,
                 "previous":0.0,
                 "delta":0.0
              }
           ]
        },
        {
           "platform":"iOS",
           "value":0.0,
           "delta":0.0,
           "previous":0.0,
           "paymentSystems":[
              {
                 "system":1,
                 "value":0.0,
                 "previous":0.0,
                 "delta":0.0
              },
              {
                 "system":2,
                 "value":0.0,
                 "previous":0.0,
                 "delta":0.0
              }
           ]
        }
     ]
  }
```


### HTTP Request

`GET https://www.wazza.io/api/analytics/{METRIC}/{TYPE}/{COMPANY}/{APP}/{BEGIN}/{END}`

### URL Parameters

Parameter | Description
--------- | -----------
METRIC | The metric you want to retrieve
TYPE | Total (value) or Detailed (a list of the values per day)
COMPANY | Your company name
APP | Your app name
BEGIN | The beginning of the timeframe
END | The ending of the timeframe

### Success Response:

* **Code:** 200 OK
* **Content:**
  ```json
    {
       "value": "[number]",
       "delta": "[number]",
       "previous": "[number]"
    }
  ```

### Error Response:

* **Code:** 404 NOT FOUND
* **Content:**
  ```json
    {
    "error": "[string]"
    }
  ```

# SDKs Integration

The integration process is designed to be as easy as possible with basic setup complete in under 5 minutes.
Support for other platforms and frameworks is coming. Drop us an e-mail and we may be able to get you early access to new SDKs.


The SDK archive should contain these files:

  + Android SDK - Wazza_Android_SDK_x.y.z.jar : The library containing Wazza's analytic collection and reporting code for Android platform.

  + iOS SDK - Wazza_iOS_SDK_x.y.z which contains library .a file and folder with headers for iOS platform

  + ProjectApiKey.txt : This file contains the name of your project and your project's API key. Alternatively, you can also get the key in the Dashboard.

  + README.pdf : PDF file containing instructions (This exact same information).

x.y.x denote the latest version of Wazza SDK. You can download this archive from your dashboard.

## Android

    1. Download the Wazza SDK.

    2. Add the Wazza lib to your project
      
      - If you're using Eclipse, modify your Java Build Path, and choose Add External JAR.
      
      - If you're using the SDK tools directly, drop it into your libs folder and the ant task will pick it up.

    3. Configure your AndroidManifest.xml to:

> The manifest should look like this:

```xml
  <manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="io.wazza.sample"
    android:versionCode="1"
    android:versionName="1.0">

    <uses-sdk
      android:minSdkVersion="15"
      android:targetSdkVersion="19"/>
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
  </manifest>
```
      - Have access to the Internet and allow Wazza SDK to check state of the network connectivity

      - You may specify a versionName attribute in the manifest to have data reported under that version name
      
      - Declare min version of Android OS the application supports. Please note that Wazza supports Android OS versions 15 and above.

    4. Incorporate the following lines of Wazza code:
      - For each activity of your application, add:

        ```java
          import io.wazza.sdk.android.Wazza;
        ```

        and on onCreate():

        ```java
          Wazza wazza = Wazza.init(this, "API_KEY", "Application Name", "Company Name");
          wazza.sessionOpen();
        ```

        and on onStop():

        ```java
          wazza.sessionClose();
        ```

      - Whenever you call the Google In-app Billing service, swap that call with Wazza's one:

        ```java
          wazza.purchase("SKU");
        ```

    That's it. We recommend always calling Wazza from the main (UI) thread. The results are not guaranteed or supported when called from other threads.

## iOS

    1. Download the Wazza SDK

      ![Alt Text](https://s3-eu-west-1.amazonaws.com/wazza-landing-page/ios+doc+-+1.png "Figure 1 - Wazza content")

    2. Drag **Wazza** folder to your Xcode project:

      ![Alt Text](https://s3-eu-west-1.amazonaws.com/wazza-landing-page/ios+doc+-+2.png "Figure 2 - Draging")

    3. Select **Copy items into destination Group's folder** and choose **Create groups for any added folders**:

      ![Alt Text](https://s3-eu-west-1.amazonaws.com/wazza-landing-page/ios+doc+-+3.png "Figure 2 - Draging")

    4. Code setup.

You need to initialize Wazza's singleton with your API token (you can get it on app setting of Wazza's dashboard).

    ```objective_c
      [WazzaAnalytics initWithCredentials:@"Company Name" :@"App Name" :@"API_KEY"];
      [WazzaAnalytics setDelegate:self];
    ```

From this moment on, each time you want to use Wazza's singleton, you can use it with:

    ```objective_c
      [WazzaAnalytics sharedInstance];
    ```

Next, you need to put the following code every time a session is closed.

    ```objective_c
      [[WazzaAnalytics sharedInstance] endSession];
    ```

On every **in-app purchase action** you will need to add the following code

    ```objective_c
      [[WazzaAnalytics sharedInstance] makePurchase:@"PRODUCT_ID"];
    ```

[register]:https://www.wazza.io/register
[login]:https://www.wazza.io/login
