

### Add the `I/O Events` API to your Adobe developer console project


For your AEM to be authorized to emit events against Adobe I/O:
you do need to add the `I/O Events` API to your project in the Adobe developer console (if you are new to this please 
refer to the [step-by-step instructions for creating an empty project in Adobe Developer Console](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md) )

Before you begin, on the top-right corner of the Adobe Developer Console, 
verify you are holding a `System Administrator` role
 
 !["System Administrator shown in the console](../img/console_role_system_admin.png "System Administrator shown in the console") 

1. Click on `Add to Project` > `API`

   ![Add an API to Project](../img/add_api_to_project.png "Add an API to Project")

2. Select `I/O Events API`, Click `Next`

   ![Select `I/O Events API`](../img/select_io_events_api.png "Select `I/O Events API`")

3. In the next `Create a new service account (JWT) credential` screen, choose `Option 2` 
and upload your public certificate, the very same one you put in the AEM service user's keystore,
see our [AEM keystore setup documentation](aem_keystore_setup.md).

6. Click on `Save configured API`

   ![Save `I/O Events API`](../img/save_io_events_api.png "Save `I/O Events API`")

7. Done ! You should now see `I/O Events` in your API list:

   ![`I/O Events` in the list of API](../img/list_io_events_api.png "`I/O Events` in the list of API`")
 
Note you also have a new JWT integration tab, that you will come back to in order to finalize the configuration
of your AEM instances.
You may also refer to the overal [Adobe I/O console documentation](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/services-add-api-jwt.md) 
for more details on this.