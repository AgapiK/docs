---
title: "Translation"
url: /appstore/app-services/translation/
category: "App Services"
description: "Describes the configuration and usage of the Translation app service, which enables performing text translation in your web apps."
tags: ["translation", "service", "app store", "marketplace", "component", "platform support"]
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details. 
---

## 1 Introduction

The [Translation](https://marketplace.mendix.com/link/component/118411) app service enables you to build web applications to work with state-of-the-art multi-language text translation. All you need to do is drag and drop items and configure them.

Here is an overview of what the Translation contains:

| Item                                        | Name                        |
| ------------------------------------------- | --------------------------- |
| [Predefined entities](#predefined-entities) | Language, Translator        |
| [Constants](#constants)                     | AWS_Default_Region |
| [Microflows](#microflows)                   | CreateTranslator, TranslateText_MF            |
| [Nanoflows](#nanoflows)                     | TranslateText               |

In most cases, you will only need what is contained in the **Translation** > **USE_ME** folder. The content in the **Translation** > **Internal** folder is for internal use only and you will not need it.

### 1.1 Typical Use Cases

You can use this app service on Mendix cloud that enables you to easily perform text translation with multi-language support in your Mendix applications.

### 1.2 Features

This app service enables doing the following:

* Customize text translation
* Switch different language options

### 1.3 Prerequisites

This app service can only be used with Studio Pro 9 versions starting with [9.3.0](/releasenotes/studio-pro/9.4/).

## 2 Installation

### 2.1 Obtaining a AWS credentials {#obtain-aws-credentials}

Translation is a premium Mendix product that requires AWS authentication. To use this app service in your app, first you must obtain AWS credentials. For more information, see [AWS credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html).

### 2.2 Installing the Component in Your App

1. To download and install the Translation app service in your app, follow the instructions in the [Importing Content from the App Explorer](/appstore/general/app-store-content/#import) section in *Use Marketplace Content in Studio Pro*. After the app service is installed, you can see it in the **App Explorer** and also in the **Cognitive AI widgets** category in the **Toolbox**.

2. To download and install the AWS Authentication Connector in your app, follow the instructions in the [Importing Content from the App Explorer](/appstore/general/app-store-content/#import) section in *Use Marketplace Content in Studio Pro*. After the app service is installed, you can see it in the **App Explorer**.

3. Map the **Administrator** and **User** module roles of the installed modules to the applicable user roles in your app.

## 3 Configuration

### 3.1 Predefined Entities {#predefined-entities}

#### 3.1.1 Translator

The **Translator** entity is a conceptual entity that incorporates all the information of the translator object. It contains both input and output text strings. You can choose to inherit from this entity, set an association to the entity, or copy this entity to your module. 

{{< figure src="/attachments/appstore/app-services/translation/translator.png" alt="translator" >}}

| Attribute | Description |
| --- | --- |
| **InputText** | The input text string (minimum length: 1, maximum length: 5000). |
| **OutputText** | The output text string. |

#### 3.1.2 Language

The **Language** entity is an entity referenced from **Translator** that incorporates all the information of supported language object.

{{< figure src="/attachments/appstore/app-services/translation/language.png" alt="language" >}}

| Attribute | Description |
| --- | --- |
| **Name** | The language name, equivalent to the locale name. |
| **Code** | The [language code](#supported-languages) that assigns letters or numbers as identifiers or classifiers for languages (minimum length: 2, maximum length: 5). |

### 3.2 Constants {#constants}

#### 3.2.1 AWS_Default_Region

The **AWS_Default_Region** constant provides a default AWS region configuration for an app that uses this app service. AWS Regions are separate geographic areas that AWS uses to house its infrastructure. These are distributed around the world so that customers can choose a region closest to them in order to host their cloud infrastructure there. The closer your region is to you, the better, so that you can reduce network latency as much as possible for your end-users.

### 3.3 Microflows {#microflows}

#### 3.3.1 CreateTranslator {#create-translator}

The **CreateTranslator** microflow takes **inputText**, **inputLanguageCode**, and **outputLanguageCode** as input parameters and creates translator actions in the back-end service. For instance, **inputLanguageCode** and **outputLanguageCode** can be set to `en-US`.

{{% alert color="info" %}}
For more information about the language codes, see the [Supported Languages](#supported-languages) section.
{{% /alert %}}

{{< figure src="/attachments/appstore/app-services/translation/createtranslator.png" alt="createtranslator" >}}

#### 3.3.2 TranslateText_MF

The **TranslateText_MF** microflow takes the **translator** and **credentials** object as an input parameter, performs text translation actions in the back-end service, and eventually updates the output text string of the **translator** object.

{{< figure src="/attachments/appstore/app-services/translation/translatetext.png" alt="translatetext" >}}

### 3.4 Nanoflows {#nanoflows}

#### 3.4.1 TranslateText

The **TranslateText** nanoflow takes the **translator** and **credentials** object as an input parameter, performs text translation actions in the back-end service, and eventually updates the output text string of the **translator** object.

{{< figure src="/attachments/appstore/app-services/translation/translatetext.png" alt="translatetext" >}}

### 3.5 Supported Languages {#supported-languages}

| Language | Language Code |
| ---- | -----------|
| Afrikaans |af |
| Albanian |sq |
| Amharic |am |
| Arabic |ar |
| Armenian |hy |
| Azerbaijani |az |
| Bengali |bn |
| Bosnian |bs |
| Bulgarian |bg |
| Catalan |ca |
| Chinese (Simplified) |zh |
| Chinese (Traditional) |zh-TW |
| Croatian |hr |
| Czech |cs |
| Danish |da |
| Dari |fa-AF |
| Dutch |nl |
| English |en |
| Estonian |et |
| Farsi (Persian) |fa |
| Filipino, Tagalog |tl |
| Finnish |fi |
| French |fr |
| French (Canada) |fr-CA |
| Georgian |ka |
| German |de |
| Greek |el |
| Gujarati |gu |
| Haitian Creole |ht |
| Hausa |ha |
| Hebrew |he |
| Hindi |hi |
| Hungarian |hu |
| Icelandic |is |
| Indonesian |id |
| Irish |ga |
| Italian |it |
| Japanese |ja |
| Kannada |kn |
| Kazakh |kk |
| Korean |ko |
| Latvian |lv |
| Lithuanian |lt |
| Macedonian |mk |
| Malay |ms |
| Malayalam |ml |
| Maltese |mt |
| Marathi |mr |
| Mongolian |mn |
| Norwegian |no |
| Pashto |ps |
| Polish |pl |
| Portuguese |pt |
| Portuguese (Portugal) |pt-PT |
| Punjabi |pa |
| Romanian |ro |
| Russian |ru |
| Serbian |sr |
| Sinhala |si |
| Slovak |sk |
| Slovenian |sl |
| Somali |so |
| Spanish |es |
| Spanish (Mexico) |es-MX |
| Swahili |sw |
| Swedish |sv |
| Tamil |ta |
| Telugu |te |
| Thai |th |
| Turkish |tr |
| Ukrainian |uk |
| Urdu |ur |
| Uzbek |uz |
| Vietnamese |vi |
| Welsh |cy |

### 3.6 Configuring the AWS credentials {#configure-aws-credentials}

#### 3.6.1 For an App Deployed Locally or as a Mendix Free App

If you deploy your app locally or as a Mendix Free App, configure the AWS credentials in Studio Pro. Perform the following steps:

1. Create a microflow as follows:
    1. Name the microflow *GetCredential*. 
    2. Right-click the working area and select **Add** > **Activity** from the pop-up menu.
    3. Double-click the activity to open the **Action Activity** dialog box.
    4. Select **Get Static Credentials** action from **AWS Authentication** category as target object.
    5. Under **Input** section, fill **Access key ID**, and **Secret access key** as AWS credentials respectivly.
    6. Under **Output** section, update **Object name** as *Credentials*.
    7. Click **OK** to save the changes.
    8. Right-click the credentials action to *Set $Credentials as return value*.

2. Default AWS region configuration:
    1. In the App Explorer, go to **Settings** to open the [App Settings](/refguide/app-settings/) dialog box.
    2. On the **Configurations** tab, click **Edit** to open the **Edit Configuration** dialog box.
    3. On the **Constants** tab, create a new constant with the predefined constant **Translation.AWS_Default_Region**.
    4. Fill in the **Value** with the AWS region that you obtained.
    5. Click **OK** to save the settings.

    {{< figure src="/attachments/appstore/app-services/translation/awsregion-inmendix.png" alt="awsregion-inmendix" >}}

3. This is the microflow could help you to pass credentials object in calling all the service actions. When you finish building the app, click **Run Locally** to run your app locally or click **Run** to deploy it as a Mendix Free App. Then you can see the app service in your app.

#### 3.6.2 For an App Deployed in the Mendix Cloud

If you deploy your app in the Mendix Cloud, configure the AWS region in the [Developer Portal](/developerportal/deploy/environments-details/).

Before you deploy your app, configure the app **Constants** in the deployment package.

{{< figure src="/attachments/appstore/app-services/translation/awsregion-cloudportal.png" alt="awsregion-cloudportal" >}}

If you have already deployed your app, change the existing **AWS_Default_Region** constant value on the **Model Options** tab and restart the app.

{{< figure src="/attachments/appstore/app-services/translation/awsregion-envdetails.png" alt="awsregion-envdetails" >}}

#### 3.6.3 For an App Deployed in Your Own Environment

If you deploy your app in your own environment, you need to configure the AWS region in your own environment. For more information, see [Deployment](/developerportal/deploy/).

## 4 Usage

### 4.1 Performing Text Translation in Your Browser

When you start from a blank app template in Mendix Studio Pro, follow the steps below to set up translation:

1. Create a nanoflow as follows:
    1. Name the nanoflow *CreateTranslator*.
    2. Add the **GetCredential** microflow from the sample module to the nanoflow. 
    3. Add the **CreateTranslator** microflow from the **Translation** > **USE_ME** folder to the nanoflow.
    4. Double-click the **CreateTranslator** microflow in the nanoflow, change the settings as shown in the screenshot below, and click **OK**. 

        {{< figure src="/attachments/appstore/app-services/translation/call-createtranslator-microflow.png" alt="call-createtranslator-microflow" >}}

        In this example,  **inputLanguageCode** is set to *'en'* and **outputLanguageCode** is set to *'zh'*, so the default translation will be from English to Chinese. You can also set them to other [language codes](#supported-languages). For more information about the parameters, see  [CreateTranslator microflow](#create-translator).

    5. Right-click the create object activity and select **Set $translator as return value** in the pop-up menu. 

        {{< figure src="/attachments/appstore/app-services/translation/createtranslator-nanoflow.png" alt="createtranslator-nanoflow" >}}

2. Create a microflow as follows:
    1. Name the microflow *GetTranslatorSupportedLanguages*. 
    2. Right-click the working area and select **Add** > **Parameter** from the pop-up menu.
    3. Double-click the parameter to open the **Parameter** dialog box.
    4. Set **Data type** to **Object** and select **Translator** entity from **Translation** module as target object.
    5. For **Name**, enter *translator*.
    6. Click **OK** to save the changes.
    7. Double-click the end event to open the **End Event** dialog box.
    8. Set **Type** to **List**.
    9. For **Entity**, select **Language** entity from **Translation** module 
    10. In the text box, enter *$translator/Translation.Translator_SupportedLanguages/Translation.Language*.
    11. Click **OK** to save the settings.  

        {{< figure src="/attachments/appstore/app-services/translation/gettranslatorsupportedlanguages-microflow.png" alt="gettranslatorsupportedlanguages-microflow" >}}

3. Add a **Data view** widget to your page.
4. Set the **CreateTranslator** nanoflow as the data source of the **Data view** widget as follows:
    1. Double-click the **Data view** widget to open the **Edit Data View** dialog box.
    2. For **Data source**, select **Nanoflow**.
    3. **Select** the **CreateTranslator** nanoflow for **Nanoflow**.
    4. Click **OK** to save the settings. 
5. Inside the **Data view** widget, add a **Group box** widget.
6. Change the settings of the **Group box** widget as follows:
    1. Double-click the **Group box** widget to open the **Edit Group Box** dialog box.
    2. For **Caption**, enter *Input*.
    3. Click **OK** to save the settings.
7. Inside the **Group box** widget, add a **Reference selector** widget.
8. Change the settings of the **Reference selector** widget as follows:
    1. Double-click the **Reference selector** widget to open the **Edit Reference Selector** dialog box.
    2. Go to the **Selectable objects** tab.
    3. Set **Source** to **Microflow**.
    4. Set the **Microflow** as **GetTranslatorSupportedLanguages**.
    5. Go to the **General** tab.
    6. Set **Data source** to **Translation.Translator_InputLanguage/Translation.Language/Translation.Language.Name**.
    7. For **Label caption**, enter *Language*.
    8. Click **OK** to save the settings.
9. Inside the **Group box** widget, add a **Text area** widget below the **Reference selector** widget.
10. Change the settings of the **Text area** widget as follows:
    1. Double-click the **Text area** widget to open the **Edit Text Area** dialog box.
    2. For **Data source**, **Select** the **InputText** attribute from **Data view**.
    3. For **Label caption**, enter *Text*.
    4. Click **OK** to save the settings. 
11. Inside the **Data view** widget, add a second **Group box** widget.
12. Change the settings of the **Group box** widget as follows:
    1. Double-click the **Group box** widget to open the **Edit Group Box** dialog box.
    2. For **Caption**, enter *Output*.
    3. Click **OK** to save the settings.
13. Inside the **Data view** widget, add a **Reference selector** widget.
14. Change the settings of the **Reference selector** widget as follows:
    1. Double-click the **Reference selector** widget to open the **Edit Reference Selector** dialog box.
    2. Go to the **Selectable objects** tab.
    3. Set **Source** to **Microflow**.
    4. Set the **Microflow** as **GetTranslatorSupportedLanguages**.
    5. Go to the **General** tab.
    6. Set **Data source** to **Translation.Translator_OutputLanguage/Translation.Language/Translation.Language.Name**.
    7. For **Label caption**, enter *Language*.
    8. Click **OK** to save the settings.
15. Inside the **Group box** widget, add a **Text area** widget below the **Reference selector** widget.
16. Change the settings of the **Text area** widget as follows:
    1. Double-click the **Text area** widget to open the **Edit Text Area** dialog box.
    2. For **Data source**, Select the **OutputText** attribute from **Data view**.
    3. For **Label caption**, enter *Text*.
    4. Click **OK** to save the settings. 

        {{< figure src="/attachments/appstore/app-services/translation/data-view.png" alt="data-view" >}}

16. Inside the **Data view** widget, Add a new **Data view** widget below the **Text area** widget.
17. Set the **GetCredential** microflow as the data source of the **Data view** widget as follows:
    1. Double-click the **Data view** widget to open the **Edit Data View** dialog box.
    2. For **Data source**, select **Microflow**.
    3. **Select** the **GetCredential** microflow for **Microflow**.
    4. Click **OK** to save the settings. 
18. Inside the GetCredential **Data view** widget, add a **Button** widget.
19. Change the settings of the **Button** widget as follows:
    1. Double-click the **Button** widget to open the **Action Button** dialog box.
    2. For **Caption**, enter *Translate*.
    3. In the **Event** section, set **On click** to **Call a nanoflow**.
    4. For **Nanoflow**, **Select** the **TranslateText** nanoflow from the **Translation** > **USE_ME** folder.
    5. Click **OK** to save the settings.  
20. Make sure you have [configured the AWS credentials](#configure-aws-credentials).
21. Run your app locally. You can perform text translation directly in the browser:

    {{< figure src="/attachments/appstore/app-services/translation/runlocally-translation.png" alt="runlocally-translation" >}}

### 4.2 Checking Statistics on the Usage Dashboard {#check-usage}

The **Usage Dashboard** shows the real-time statistics about the usage of an app service. Perform the following steps to check the real-time statistics:

1. Log into the Marketplace.
2. Go to **My Marketplace** and then do as follows:

    * If you have a trial, click [My Subscriptions](https://marketplace.mendix.com/link/mysubscriptions) on the left navigation menu. This page shows all the products that you have trials for.
    * If you have a subscription, click [Company Subscriptions](https://marketplace.mendix.com/link/company/subscriptions) on the left navigation menu. This page gives an overview of all the subscriptions of your organization.
3. Find **Translation** in the list.
4. Click **Usage Dashboard** to show the usage details.

## 5 Read More

* [Text Analytics and Translation App](https://academy.mendix.com/link/paths/118/Text-Analytics-and-Translation-App)
