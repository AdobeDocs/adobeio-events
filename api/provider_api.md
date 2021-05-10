<!--:nav_order:2-->

# Adobe I/O Events Management API

## Prerequisites

1. Create a project in the [`Adobe Developer Console`](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md)
2. Add the `I/O Management API` in your `Adobe Developer Console` project 
  2.1 Click on Add to Project > Service > API > Add an API to Project
  2.2 Select `I/O Management API`, Click Next
  2.3 Create a new service account (JWT) credential screen, 
  2.4 Click on Save configured API
  2.5 Bookmark your workspace, as you might need to come back to it more than once, to fine tune or troubleshoot your configurations.
  2.6 Once done, note you have a JWT credentials defined
3. Note all your project Ids
  3.1 Browse to your `Adobe Developer Console` > `Project overview`
  3.2 Find your `IMS Org Id`, and `api-key` 
  3.2 Click on `Download`, open the downloaded `json` file with your favorite editor, in there you'll find :
        * your project Id (at `project.id`) 
        * your consumer Org Id (also called `consumer id`) (at `project.org.id`)
        * your workspace Id (at `project.workspace.id`)           
5. [Generate a JWT token](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/credentials.md)

## Test Drive

Once the above are defined (and stuffed as environment variables),
you are ready to use API, confer its [`swagger`/`OpenApi` documentation](https://www.adobe.io/apis/experienceplatform/events/ioeventsapi.html)

To help you further, here are a few sample `curl` commands.
 
The one below will `GET` the list of all the event providers you are entitled to use.

       curl -v --request GET \
        --url https://api.adobe.io/events/${consumerId}/providers \
        --header "x-api-key: $api_key" \
        --header "Authorization: Bearer $jwt_token" \
        --header "Accept: application/hal+json"
        
Now you have the provider ids, you can list their events metadata: 

      curl -v --request GET \
        --url https://api.adobe.io/events/providers/${providerId}?eventmetadata=true \
        --header "x-api-key: $api_key" \
        --header "Authorization: Bearer $jwt_token" \
        --header "Accept: application/hal+json" 
        
To create your own [custom events provider](../using/custom_events.md) :

    curl -v --request POST \
      --url https://api.adobe.io/events/${consumerId}/${projectId}/${workspaceId}/providers \
      --header "x-api-key: $api_key" \
      --header "Authorization: Bearer $jwt_token" \
      --header 'content-type: application/json' \
      --header 'Accept: application/hal+json' \
      --data '{
          "label": "a label of your choice for you custom events provider",
          "description": "a description of your custom events Provider",
          "docs_url": "https://yourdocumentation.url.if.any"
        }'
        
To associate event metadata with the above:

    curl -v --request POST \
      --url  https://api.adobe.io/events/${consumerId}/${projectId}/${workspaceId}/providers/${providerId}/eventmetadata \
      --header "x-api-key: $api_key" \
      --header "Authorization: Bearer $jwt_token" \
      --header 'content-type: application/json' \
      --header 'Accept: application/hal+json' \
       --data '{
      "event_code": "your.reverse.dns.event_code",
      "label": "a label for your event type",
      "description": "a description for your event type"
       }'

With the 2 commands above, your custom events provider is ready to be used, 
you can register [webhooks](../intro/webhooks_intro.md) against it;
to start emitting events on its behalf use our [Publishing API](eventsingress_api.md).

To delete your custom event provider:

    curl -v --request DELETE \
     --url https://api-stage.adobe.io/events/${consumerId}/${projectId}/${workspaceId}/providers/${providerId} \
     --header "x-api-key: $api_key" \
     --header "Authorization: Bearer $jwt_token" \
     --header "Accept: application/hal+json" 


The environment variables used in this `curl` commands are computed from the prerequisites documented above:
* `api_key` is the api-key associated with your workspace in the`Adobe Developer Console`
* `jwt_token` is a jwt token generated using the set up from the same workspace
* `projectId` is the `project.id` found the `json` model of your `Adobe Developer Console` project (see above) 
* `consumerId` is the `project.org.id` found the `json` model of your `Adobe Developer Console` project (see above) 

 
 
