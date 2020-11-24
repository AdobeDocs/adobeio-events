
### `AEM Link Externalizer` Configuration

The AEM Link Externalizer `author` value is used by default by Adobe I/O Events 
as a unique identifier of the AEM author host withing an IMS organization.

For `AEM as a Cloud Service` cluster this configuration is not needed, 
as the AEM Link Externalizer is already set by for you by the cloud manager.

However for `on premise` AEM, you need to make sure the AEM Link Externalizer is properly configured:

* Open the Web Console, or select the **Tools** icon, then select **Operations** and **Web Console**.
* Scroll down the list to find **Day CQ Link Externalizer**, update the `author` url, and select **Save** when done.

Note that this base URL will be reflected in the Adobe I/O developer console in the AEM event provider label. 
If you keep it default, that is `http://localhost:4502`, here is how it will be shown the Adobe developer console:

![AEM Web Console](../img/console_aem_locahost_event_provider.png "AEM Web Console")



    

