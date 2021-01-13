<!--:nav_order:2-->

# Adobe I/O Events Provider API

## Prerequisites

1. Create a project in the [`Adobe I/O Developer console`](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md)
2. Add the `I/O Management API` in your `Adobe I/O Developer console` project 
  2.1 Click on Add to Project > Service > API > Add an API to Project
  2.2 Select `I/O Management API`, Click Next
  2.3 Create a new service account (JWT) credential screen, 
  2.4 Click on Save configured API
  2.5 Bookmark your workspace, as you might need to come back to it more than once, to fine tune or troubleshoot your configurations.
  2.6 Once done, note you have a JWT credentials defined
3. Note all your project Ids
  3.1 Browse to your `Adobe I/O Developer console` > `Project overview`
  3.2 Find your `IMS Org Id`, and `api-key` 
  3.2 Click on `Download`, open the downloaded `json` file with your favorite editor, in there you'll find :
        * your project Id (at `project.id`) 
        * your consumer Org Id (also called `consumer id`) (at `project.org.id`)
        * your workspace Id (at `project.workspace.id`)           
5. [Generate a JWT token](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/credentials.md)

## Test Drive

Once the above are defined (and stuffed as environment variables),
you are ready to user the [Provider API](https://www.adobe.io/apis/experienceplatform/events/ioeventsapi.html#/Providers/getProvidersByConsumerOrgId)

To help you further, here is below a sample `curl` query that will `GET` the list of all the event providers you are entitled to use.

       curl -v --request GET \
        --url ${api_url}/events/${consumerId}/providers \
        --header "x-api-key: $api_key" \
        --header "Authorization: Bearer $jwt_token" \
        --header "Accept: application/hal+json"
        
once you have the provider id of interest, you can list of the events (and event codes) 
it supports by using another endpoint: [getProvidersById](https://www.adobe.io/apis/experienceplatform/events/ioeventsapi.html#/Providers/getProvidersById):  
Here is the associated sample `curl` query 

      curl -v --request GET \
        --url ${api_url}/events/providers/${providerId}?eventmetadata=true \
        --header "x-api-key: $api_key" \
        --header "Authorization: Bearer $jwt_token" \
        --header "Accept: application/hal+json" 
