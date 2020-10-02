
## Integrate with AEM (on premise)

This documentation is specific to `AEM on premise` set up.

To integrate with `AEM as cloud service`, please refer to the other associated [documentation](aem_skyline_install.md).
 
### Install the AEM event proxy package

To install the AEM event proxy package:

1. Download the latest version of the package
   * [version 6.3.16](https://github.com/adobeio/adobeio-documentation/files/2649329/aem-event-proxy-6.3.16.zip) for AEM `6.2.xx` and `6.3.xx`
   * [version 6.4.268](https://github.com/adobeio/adobeio-documentation/files/2624686/aem-event-proxy-6.4.268.zip) for AEM `6.4.xx`
   * [version 6.5.6](https://github.com/AdobeDocs/adobeio-events/files/3729022/aem-event-proxy-6.5.6.zip) for AEM `6.5.xx`
   

2. Open AEM Package Manager by selecting the **Tools** icon and then selecting **Deployment** and **Packages**.

   ![Package Manager navigation](../img/events_aem_02.png "Package Manager navigation")

3. In **Package Manager**, select **Upload Package**. Select **Browse** and navigate to the package zip file. Select **OK**.

      >Note: If you have an older version of the package, delete it to avoid potential conflicts. You can delete it from the following location: **crx/de/index.jsp#/apps/eventproxy/install**.

4. Select **Install**.

      ![Package Manager UI](../img/events_aem_03.png "Package Manager UI")

5. On the **Install Package** dialog box, select **Merge** from the **Access Control Handling** drop-down list and select **Install**.

      ![Install the package](../img/events_aem_04.png "Install the package")

6. Watch the **Activity Log**. If installed, the log reports that the package is imported.

      ![Activity Log](../img/events_aem_05.png "Activity Log")

For more information on installing packages in AEM, see [How to Work with Packages](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/package-manager.html).

#### Additional package installation notes:

1. If you are upgrading the package, delete the previous .jar file from the following location: **`/apps/eventproxy/install`**

2. Verify that the Access Control Handling is properly applied by checking permissions for the `eventproxy-service` user at **/useradmin**.
If applied correctly, the `eventproxy-service` user is added to the following:

      *   **`/home/users/system/eventproxy/eventproxy-service` with `jcr:read` and `rep:write` authorizations**
      *   **`/content` with `jcr:read` authorization**
      *   **`/etc/cloudservices/eventproxy` with `jcr:read` and `rep:write` authorizations** (for AEM versions prior to 6.4.0)
      *   **`/var/eventproxy` with `jcr:read` and `rep:write` authorizations** (for AEM 6.4.0 and beyond)


For more information, see AEM [User, Group and Access Rights Administration](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/user-group-ac-admin.html).


### Configure the AEM Link Externalizer

To configure AEM Link Externalizer:

1. Open the Web Console, or select the **Tools** icon, then select **Operations** and **Web Console**.

    The AEM Link Externalizer name can be **author** or any other alias specified in the Adobe Experience Manager Web Console.

    ![AEM Web Console](../img/events_aem_11.png "AEM Web Console")

2. Scroll down the list to find **Day CQ Link Externalizer**, update the domain name, and select **Save** when done.

    >**Note:** The base URL that you specify appears on the AEM Web Console. Do not use only the word “localhost” as the default name because others may use it. This will then cause confusion and make it difficult to determine which instance is yours. for example, for below image you will see "AEM-localhost-ml-sample" in your console as event provider.

    ![AEM Web Console base URL](../img/events_aem_12.png  "AEM Web Console base URL")


### Configure Adobe I/O authentication

To secure the calls between Adobe I/O and AEM, we leverage an oAuth JWT exchange token flow.
This flow uses a certificate to sign the JWT request and therefore requires certificates configurations
on both ends. Please look at our documentations:
* first [set up a keystore on AEM](aem_keystore_setup.md) 
* then [set up an AEM workspace in Adobe I/O console](aem_console_setup.md)

Once these 2 setups are done, you should have:

* created a public/private certificate key and an associated keystore
* added the keystore into the AEM `eventproxy-service` user&rsquo;s keystores vault.
* created an Adobe I/O Console project and workspace using the public certificate

### AEM 6.2 and AEM 6.3 Configuration

To configure Adobe I/O Events in AEM:

1. Open the Cloud Services console, or select the **Tools** icon, then select **Deployment** and **Cloud Services**.

      ![Cloud Services UI](../img/events_aem_17.png "Cloud Services UI")

2. Under **Adobe Marketing Cloud** on the **Cloud Services** page, select **Configure now** for **Adobe I/O Events**.

      ![Configure Adobe Events](../img/events_aem_18.png "Configure Adobe Events")

3. In the Create Configuration dialog box, enter a title and a name for your integration, and then select Create.
      ![Create a configuration](../img/events_aem_19.png "Create a configuration")

4. Select Edit. Configure the service by specifying each field in the **Edit Component** dialog box. You can copy your credentials from the [Adobe I/O Console](https://adobe.io/console) and paste them into each required field.

      ![Edit the configuration](../img/events_aem_20.png "Edit the configuration")

*   For **AEM Link externalizer**: specify **author** (or any other alias previously configured in the AEM Link Externalizer).
*   For **API key**: Provide the key shown on the **Integration Details** page of the Adobe I/O Console.
*   For **Technical Account ID**: Provide the ID shown on the Adobe I/O Console.
*   For **Organization ID**: Provide the ID shown on the Adobe I/O Console.
*   For **Client Secret**: AEM will automatically retrieve the value from the Adobe I/O Console.

### AEM 6.4 and AEM 6.5 Configuration

You are now ready to finalize the Adobe IMS configuration needed by Adobe I/O Events:

1. Open your AEM `Tools` menu, or select the `Security` section, and then select `Adobe IMS Configurations` tile.

   ![Adobe IMS Configurations UI](../img/events_aem_adobe-ims-conf-1.png "Adobe IMS Configurations UI")

2. Click **Create**;
   **Select `AdobeIOEvents`** in the Cloud Solution drop down; once selected, you should see the certificate you just added to the AEM `eventproxy-service` user's keystore;
   Click **Next**

    ![Adobe IMS Configuration Creation](../img/events_aem_adobe-ims-conf-2.png "Adobe IMS Configuration Creation")

3. Fill in the various entries expected to configure the IMS account associated with the integration
 you just [created in the Adobe I/O Console](aem_console_setup.md):
     * For **Title**: specify **Adobe IO Events** (or any other title that makes sense to you).
     * For **Authorization Server**: it should be `https://ims-na1.adobelogin.com` (unless the URL shown in the **JWT** tab of your integration page in the Adobe I/O Console is different),
     * For **API key**: Provide the API key available in the `Credentials details` tab of your credentials `Service Account (JWT)` page in your Adobe I/O Console workspace
     * For **Client Secret**: Provide the Client Secret available in the `Credentials details` tab of your credentials `Service Account (JWT)` page in your Adobe I/O Console workspace
     * For **Payload**: Provide the JWT payload available in the `Generate JWT` tab of your credentials `Service Account (JWT)` page in your Adobe I/O Console workspace
  
   ![Adobe I/O Console Generate JWT tab](../img/console_generate_jwt_tab.png "Adobe I/O Console Generate JWT tab")

4. Click **Create**

    ![Adobe IMS Configuration IMS account form](../img/events_aem_adobe-ims-conf-3.png "Adobe IMS Configuration IMS account form")

5. Now you should see this new Adobe IO Events IMS Configuration, and you can select it to check its health.

     ![Adobe IMS Configuration IMS Health Check](../img/events_aem_adobe-ims-conf-4.png "Adobe IMS Configuration Health Check")
       
  
### Event emitting health check

Now you can start testing that your AEM-originated events are emitted by Adobe I/O.
To do this, first make sure your webhook is subscribed to the type of event you are testing against.
For asset-related events, create, update or delete an asset in AEM DAM;
for page-related events, publish or unpublish a page;
finally, to test our custom OSGI event type sample, you may use our custom OSGI event health check.

  ![Custom osgi event health check](../img/events_aem_28.png "Custom osgi event health check")

>**Note:** In case of [aem-event-proxy-package 6.5.6](https://github.com/AdobeDocs/adobeio-events/files/3729022/aem-event-proxy-6.5.6.zip) for AEM `6.5.xx`, the health checks are disabled by default and can be enabled by the following steps -
1. Select **Tools** in AEM and then select **Operations** and **Web Console**.
2. In the **OSGI** menu, select **Configuration** and search for: **Adobe I/O Events Activate Health Check Feature Flag**.
3. For **Adobe I/O Events Activate Health Check Feature Flag**, select **Edit**, check the **Enabled** checkbox and then click on **Save** button.
<br />Once you are done with debugging and testing your configuration, we would advise you to disable the health checks again by following the same steps as above but unchecking the **Enabled** checkbox for **Adobe I/O Events Activate Health Check Feature Flag**.

If for some reason, your webhook is failing, note that the Adobe I/O console holds a **Debug Tracing** feature:
It allows you to watch all the events payloads emitted by Adobe I/O towards your webhook and the associated webhook response.
In the console, select your integration, the `Events` Tabulation, your webhook, then `Debug tracing`,
see the screenshot below:

![Debug tracing UI](../img/events_aem_debug-tracing.png "Debug tracing UI")


<a id="configure-advanced-adobe-i/o-events">&nbsp;</a>

#### Other AEM health check

You can use the AEM Web Console Sling Health Check to verify that your configurations are correct.

To verify your configurations:

1. Check that all your configurations load properly by executing the health check tagged with **conf-events**.

      ![Health check for eventproxy,conf](../img/events_aem_21.png "Health check for conf-events")

If this health check is failing, check the following:

* Make sure you have the correct event proxy package and update it to the latest version (currently 0.32.226) if needed.
* Verify that when you installed your package, you chose the right install settings.
* Make sure your AEM instance is compatible with Events (must be version 6.2 or higher).
* Check the permissions for your event proxy package.
* Match your I/O Events configuration to the integration that you've created in the Adobe I/O Console.

2. Check that the AEM instance is able to exchange JWT tokens with Adobe I/O Identity Management System (IMS).
To do this, execute the health check tagged with **ims-events**.
This verifies that your IMS-related configurations are correct and working,
including the `eventproxy-service` user keystore configuration, the Adobe I/O console&ndash;originated API key, the Technical Account ID,
the Organization ID, and the client secret.

      ![Health check for eventproxy,ims](../img/events_aem_22.png "Health check for ims-events")

If this health check is failing, check the following:

* Match your I/O Events configuration to the integration that you've created in Adobe I/O Console.
* Make sure that the keystore you've uploaded matches the public certificate in your integration.


3. Check that the event metadata and the provider associated with the AEM instance are registered in Adobe I/O Channel & Subscription Management (CSM) by executing the health check tagged with **csm-events**.
This verifies that the AEM instance is successfully registered as an event provider with Adobe I/O CSM.

      ![Health check for eventproxy,csm](../img/events_aem_23.png "Health check for csm-events")
4. (Optional) Check that the AEM instance is connected with Engress by executing the health check tagged with **evre-events**

### Adobe I/O Events OSGI to XDM event mapping configurations

For all Adobe I/O event types defined by the Adobe I/O Event Model, there is an **Adobe I/O Events OSGI to XDM event mapping configuration**.

For each of these you can edit:

* The OSGI Topic you want to observe: `osgiTopic`
* The OSGI Filter you want to apply in your OSGI event observation. If left empty no osgi filtering is done: `osgiFilter`
* The JCR `osgiJcrPathFilter` to filter the OSGI events further. If left empty, no resource path filtering is done: `osgiJcrPathFilter`
* The OSGI Event Handler Type (use the default `com.day.cq.dam.eventproxy.service.impl.listener.AdobeIoEventHandler` to map any custom OSGI event): `osgiEventHandlerClassName`
* The Adobe I/O XDM Event Type to map to the OSGI event: again, use the default (`com.adobe.xdm.event.OsgiEmittedEvent`) to map your custom OSGI events: `adobeIoXdmEventClassName`
* The Adobe I/O Event Code (unique to your event provider; in other words, unique to your AEM instance/cluster): `adobeIoEventCode`
* The Adobe I/O Event Label as it will appear on the Adobe I/O Console: `adobeIoEventLabel`

The various OSGI event handlers will intercept the events according to these values and then map these OSGI events to the Adobe I/O Event Model before forwarding them to Adobe I/O.

The solution leverages the OSGI configuration factory pattern; hence you can not only edit these configurations, but you can also remove and add such configurations.

To configure using the panel:

1. Select **Tools** in AEM and then select **Operations** and **Web Console**.

      ![Web Console navigation UI](../img/events_aem_31.png "Web Console navigation UI")

2. In the **OSGI** menu, select **Configuration**.

      ![OSGI configuration](../img/events_aem_32.png "OSGI configuration")  
      and search for: **Adobe I/O Events CSM Registration**.

3. For **Adobe I/O Events OSGI to XDM Event Mapping Configuration**, select **+**, **Edit**, or **Delete**.

### Adobe I/O Events queuing and retries

When an OSGI event of interest is triggered (i.e an OSGI events that matches one of your `OSGI to XDM event mapping configurations`), it makes it to a job queue handled by `Sling Job Handler`.

This job is persisted in the resource tree (for failover etc.), then the job is distributed to an instance responsible for processing the job; and on that instance the job is put into a processing queue, where eventually a `Sling Job Consumer` will execute it.

Adobe I/O Events `Sling Job Consumer`'s job is to send this event to Adobe I/O.
* `JobResult.OK` should be returned. If the job has not been processed completely,
* `JobResult.FAILED` should be returned if Adobe I/O fails either to receive or to process it (due to network failure or Adobe I/O failure).
In that case the job will be rescheduled/retried   
* if the max number of retries is met, the process will not be rescheduled and treated like the method would have returned `JobResult.CANCEL`.

Note that
* Adobe I/O Events `Sling Job Consumer` topic is `com/adobe/eventproxy/events`
* Adobe I/O Events `Sling Job Queue configuration` is the default `Apache Sling Job Default Queue` with
  * a Normal job priority
  * 10 maximum retries
  * 2 seconds retry delay
  * 15 maximum parallel jobs


You can tune this configuration according to your needs: using the `OSGI > configuration` menu of AEM's `system console`
and create there a new `Apache Sling Job Queue Configuration`


  ![Apache Sling Job Queue Configuration UI](../img/events_aem_job-config.png "Apache Sling Job Queue Configuration UI")

Note that you can use the `Sling > Jobs`  menu of AEM `system console`,
to look up the statistics and health of your queues

 ![Apache Sling Job UI](../img/events_aem_job-stat.png "Apache Sling Job UI")


Please refer to the Sling documentation:
* https://sling.apache.org/apidocs/sling7/org/apache/sling/event/jobs/consumer/JobConsumer.html
* https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html
