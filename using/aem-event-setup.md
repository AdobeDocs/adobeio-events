<!--:navorder:1-->

# Setting up AEM Events with Adobe I/O Events

These instructions describe how to set up Adobe Experience Manager (AEM) to integrate with Adobe I/O Events. 
**It is only supported on author instances.**
You can use Adobe I/O Events for notification of AEM events, such as page or asset CUD operations.

- [Introduction](#introduction)
- [Setup Products](#setup-products)
- [Use Adobe I/O](#use-adobe-io)

**Resources**
- [Debugging](../support/debug.md)
- [FAQ](../support/faq.md)

## Introduction

### Obtain authorization

To complete this solution, you will need authorization to use the following services:

*   An AEM instance with administrative permissions. 
*   [`Adobe Developer Console`](https://adobe.io/console) access, with administrative permissions for your enterprise organization.


## Setup Products

* Integrate with `AEM On Premise`
  * [AEM version 6.3.x](../aem/aem_on_premise_install_6.3.md) (deprecated/unsupported),
  * [AEM version 6.4.x](../aem/aem_on_premise_install_6.4.md), 
  * [AEM version 6.5.x](../aem/aem_on_premise_install_6.5.md)
* [Integrate with `AEM As a Cloud Service` ](../aem/aem_skyline_install.md)

## Use Adobe I/O

Once the above is done, you are ready to register a new [webhook](../intro/webhooks_intro.md) 
or to start pulling events from this new source using the [journaling](../intro/journaling_intro.md).


