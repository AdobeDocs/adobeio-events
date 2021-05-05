<!--:nav_order:2-->

# Events Publishing API

## Prerequisites

1. Create a project and a workspace in the [`Adobe I/O Developer console`](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md)
2. Add the `I/O Management API` in your `Adobe I/O Developer console` project 
3. [Generate a JWT token](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/credentials.md)
4. Using [Adobe I/O Events Management API](provider_api.md) 
   * create your own custom Adobe I/O Events provider
   * create at least one event metadata associated with the above

## Test Drive

Once its metadata persisted in Adobe I/O Events (see above prerequisites)
your custom Adobe I/O Events provider can start publishing its 
[Cloud Events]( https://cloudevents.io) to Adobe I/O Events publishing endpoint.

Please follow [CloudEvents v1.0 specification](https://github.com/cloudevents/spec/blob/v1.0/spec.md), 
here is a sample `curl` command:

     curl  --location --request POST  \
             --url https://eventsingress.adobe.io \
             --header "x-api-key: $api_key" \
             --header "Authorization: Bearer $jwt_token" \
             --header 'Content-Type: application/cloudevents+json' \
             --data '{
               "datacontenttype": "application/json",
               "specversion": "1.0",
               "source": "urn:uuid:'"${provider_id}"'",
               "type": "'"${event_code}"'",
               "id": "'"${your_event_id}"'",
               "data": "your event json payload"
             }'


The environment variables used in this `curl` command are computed from the above prerequisites
* `api_key` is the api-key associated with your workspace in the `Adobe I/O Developer console`
* `jwt_token` is a jwt token generated using the set up from the same workspace
* `provider_id` is your custom events providers uuid generated by Adobe I/O Events API
* `event_code` is the custom event code as persisted in Adobe I/O Events
* `your_event_id` is any id of your choice, 
ensure that `source + id` is unique for each distinct event see [Cloud Events spec](https://github.com/cloudevents/spec/blob/v1.0/spec.md#id)
*  as for the value of `data` in the cloudEvents body payload, it can be any json payload.



        