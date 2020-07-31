<!--:navorder:1-->

# Setting up AEM Events with Adobe I/O Events

These instructions describe how to set up Adobe Experience Manager (AEM) for Adobe I/O Events. You can use Adobe I/O for notification of AEM events, such as page or asset changes.

- [Introduction](#introduction)
- [Setup Products](#setup-products)
- [Use Adobe I/O](#use-adobe-io)

**Resources**
- [Debugging](../support/debug.md)
- [FAQ](../support/faq.md)

## Introduction

### Obtain authorization

To complete this solution, you will need authorization to use the following services:

*   An AEM instance, version 6.2.x, 6.3.x, 6.4.x or 6.5.x with administrative permissions. (**Note:** AEM screens in this topic are captured from version 6.3.)
*   [Adobe I/O Console](https://adobe.io/console) access, with administrative permissions for your enterprise organization.


## Setup Products

* [Integrate with `AEM On Premise`](../aem/aem_on_premise_install.md) version 6.2.x, 6.3.x, 6.4.x or 6.5.x
* [Integrate with `AEM As a Cloud Service` ](../aem/aem_skyline_install.md)

## Use Adobe I/O

Once the above is done, you are ready to register a new [webhook](../intro/webhook_docs_intro.md) 
or to start pulling events from this new source using the [journaling API](../intro/journaling_api.md).


