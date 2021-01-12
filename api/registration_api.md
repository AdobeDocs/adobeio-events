<!--:nav_order:2-->

# Adobe I/O Events Registration API

## Prerequisites

1. Create a project in the [`Adobe I/O Developer console`](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md)
2. Add the `I/O Management API` in your `Adobe I/O Developer console` project 
  2.1 Click on Add to Project > Service > API > Add an API to Project
  2.2 Select `I/O Management API`, Click Next
  2.3 Create a new service account (JWT) credential screen, 
  2.4 Click on Save configured API
  2.5 Bookmark your workspace, as you might need to come back to it more than once, to fine tune or troubleshoot your configurations.
  2.6 Once done, note you have a JWT credentials defined
3. Note all your org, api-key and other contextual Ids
  3.1 Browse to your `Adobe I/O Developer console` > `Project overview`
  3.2 Click on `Download`, open the downloaded `json` file with your favorite editor, in there you'll find :
        * your consumer Org Id (also called `consumer id`) (at `project.org.id`)
        * your IMS Org Id (at `project.org.ims_org_id`)
        * your credential Id (also called `application id`) (at `project.workspace.details.credentials[0].id`, note that `credentials` is an array, 
        if you have more than one, pick the one where you defined your jwt
        * your client_id (also called `x-api-key`) (at `project.workspace.details.credentials[0].jwt.client_id`                
4. Define your webhook registration you will need :
  * a webhook url (accessible from the internet, reachable over HTTPS and that correctly respond to a "challenge" request) see https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/intro/webhook_docs_intro.md
  * a name (a friendly user nae for you and that will used for display in the console)
  * an array of events of interests, that are defined with 2 ids
    * a `provider_id`: defining one of the events source system (the events provider) your organization is entitled to,
    * a `event_code`: defining a type of the events the above system (the events provider) is emitting.
5. [Generate a JWT token](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/credentials.md)

## Test Drive

Once the above are defined (and stuffed as environment variables),
you are ready to use our [registration API](https://www.adobe.io/apis/experienceplatform/events/ioeventsapi.html#/Registrations/createRegistrationCli).

To help you further and document the typical `Webhook Registration Payload`, 
here is a sample `POST` `curl` query that will create a new Webhook Registration against `https://your.webhook`.

    curl -v --request POST \
          --url https://api.adobe.io/events/organizations/${consumer_id}/integrations/${application_id}/registrations \
          --header "x-api-key: $api_key" \
          --header "x-ims-org-id: $ims_org_id" \
          --header "Authorization: Bearer $jwt_token" \
          --header "x-ams-consumer-id: $consumer_id" \
          --header "x-ams-application-id: $application_id" \
          --header 'content-type: application/json' \
          --header 'Accept: application/json' \
          --data '{
                    "client_id": "'"${api_key}"'",
                    "webhook_url": "https://your.webhook",
                    "name": "a user friendly name",
                    "events_of_interest": [
                        { "provider_id": "some_provider",
                          "event_code": "some_event_code"
                   }]}'
 
Once done/`200` this webhook will be POST-ed all `some_event_code` events provided by `some_provider`. 
If you wonder how to fetch the valid provider Ids and event codes, look at our [Provider API](provider_api.md)

