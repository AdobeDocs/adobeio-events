<!--:navorder: 3-->

# Journaling API

- [Journaling API](#journaling-api)
  - [What is a Journal](#what-is-a-journal)
  - [Fetching events from the journaling API](#fetching-events-from-the-journaling-api)
    - [Finding the journaling endpoint URL](#finding-the-journaling-endpoint-url)
    - [Obtaining an access token to call the API](#obtaining-an-access-token-to-call-the-api)
    - [Fetching the first batch of events from the journal](#fetching-the-first-batch-of-events-from-the-journal)
    - [Fetching the next batch of "newer" events from the journal](#fetching-the-next-batch-of-%22newer%22-events-from-the-journal)
    - [Fetching events from the end of the journal](#fetching-events-from-the-end-of-the-journal)
  - [Controlling the response](#controlling-the-response)
    - [Limiting the size of the batch](#limiting-the-size-of-the-batch)
    - [Consuming the most recent events](#consuming-the-most-recent-events)
  - [Event expiry](#event-expiry)
    - [Purging policy](#purging-policy)
    - [Positon Validation](#positon-validation)

For enterprise developers, Adobe offers another way to consume events besides webhooks: journaling. The Adobe I/O Events Journaling API enables enterprise integrations to consume events according to their own cadence and process them in bulk. Unlike webhooks, no additional registration or other configuration is required; every enterprise integration that is registered for events is automatically enabled for journaling. Journaling data is retained for 7 days.

## What is a Journal

A Journal, is an ordered list of events - much like a ledger or a log where new entries (events) are added to the end of the ledger and the ledger keeps growing. A client can start reading the ledger from any position and then continue reading "newer" entries (events) in the ledger (much like turning pages forward).

Journaling, in contrast to webhooks, is a _pull_ model of consuming events, whereas webhooks are a _push_ model. In journaling the client application issues a series of API calls to pull batches of one or more events from the journal.

Each batch of events not only contains the information about the event but also contains the corresponding unique position of those events in the journal. The position of the last event in the batch needs to be supplied back to the Journaling API in the subsequent API call to return events that are "newer" than the previously fetched events.

A batch of events returned by the Journaling API looks similar to the following JSON object.

```json
{
   "events": [ // an ordered list of events
      { 
         "position": "string", // unique position of this event in the journal
         "event": { 
            "key": "value" // actual event data
         }
      }
   ],
   "_page": {
      "last": "string", // position corresponding to the last event returned in this batch
      "count": 1 // number of events returned in this batch
   }
}
```

## Fetching events from the journaling API

### Finding the journaling endpoint URL

Every event registration has a corresponding unique journaling endpoint URL. This URL is displayed on the I/O Console - 

1. Log into [console.adobe.io](https://console.adobe.io) and open your integration. 

2. Select the Events tab. 

3. Under Event Providers, find the event registration for which you want to pull events and select View.

4. Find the Journaling section of the event details and copy the URL for the unique endpoint. 

5. Be sure to add the `I/O Management API` as a service in your integration (using the `Services` tab in the I/O Console), in order to be able to invoke the journaling API.

### Obtaining an access token to call the API

To issue the API call, you need to provide two additional parameters: 

* Your integration's API key. This is displayed in the Overview tab for your integration in the Adobe I/O Console.
* A JWT token. See [Authentication: Creating a JWT Token](https://www.adobe.io/apis/cloudplatform/console/authentication/createjwt.html) for how to create a JWT token.

You combine the URL you got from the Journaling section of the event details with your API key and JWT token to make the call.

```
curl -X GET \
  https://api.adobe.io/events/organizations/xxxxx/integrations/xxxx/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
  -H "Authorization: Bearer $JWT_TOKEN" \
  -H "x-api-key: $API_KEY"
```

### Fetching the first batch of events from the journal

The journaling endpoint URL, when called without any query parameters fetches the batch of ["oldest" available events](//TODO deep link purgining) in the journal. These events are "older" than all other events in the journal. Hence, once a client application starts consuming "newer" events from this position they will eventually consume all events in the journal.


For example:

```
curl -X GET \
  https://api.adobe.io/events/organizations/23294/integrations/54108/f89067f2-0d50-4bb2-bf78-209d0eacb6eb \
  -H "Authorization: Bearer $USER_TOKEN" \
  -H "x-api-key: $API_KEY"
```

```
HTTP/1.1 200 OK
Link: </events/organizations/23294/integrations/54108/f89067f2-0d50-4bb2-bf78-209d0eacb6eb?since=moose:e7ba778b-dace-4994-96e7-da80e7125233:2159b72c-e284-4899-b572-08da250e3614>; rel="next"

{
   "events":[
      {
         "position":"camel:5aeb25cc-1b15-4f26-a082-9c213005dba8:ff244403-ca7c-4993-bbda-3c8915ce0b32",
         "event":{
            "@id":"urn:oeid:aem:199e85da-dd54-4966-95ba-13cf544964dc",
            "@type":"xdmCreated",
            "activitystreams:published":"2018-03-06T15:19:03-08",
            "activitystreams:to":{
               "@type":"xdmImsOrg",
               "xdmImsOrg:id":"01DC1FC45956A5810A494138@AdobeOrg"
            },
            "activitystreams:generator":{
               "@type":"xdmContentRepository",
               "xdmContentRepository:root":"http://localhost-roberto-aem63:4502"
            },
            "activitystreams:actor":{
               "@id":"healthcheck-user",
               "@type":"xdmAemUser"
            },
            "activitystreams:object":{
               "@type":"xdmAsset",
               "xdmAsset:asset_name":"healthcheck.png",
               "xdmAsset:path":"/content/dam/healthcheck.png",
               "xdmAsset:format":"image/png"
            },
            "xdmEventEnvelope:objectType":"xdmAsset"
         }
      },
      {
         "position":"moose:e7ba778b-dace-4994-96e7-da80e7125233:2159b72c-e284-4899-b572-08da250e3614",
         "event":{
            "@id":"urn:oeid:aem:61930ae6-ff2a-48fb-881d-078e862c3811",
            "@type":"xdmCreated",
            "activitystreams:published":"2018-03-06T15:19:59-08",
            "activitystreams:to":{
               "@type":"xdmImsOrg",
               "xdmImsOrg:id":"01DC1FC45956A5810A494138@AdobeOrg"
            },
            "activitystreams:generator":{
               "@type":"xdmContentRepository",
               "xdmContentRepository:root":"http://localhost-roberto-aem63:4502"
            },
            "activitystreams:actor":{
               "@id":"healthcheck-user",
               "@type":"xdmAemUser"
            },
            "activitystreams:object":{
               "@type":"xdmAsset",
               "xdmAsset:asset_name":"healthcheck.png",
               "xdmAsset:path":"/content/dam/healthcheck.png",
               "xdmAsset:format":"image/png"
            },
            "xdmEventEnvelope:objectType":"xdmAsset"
         }
      }
   ],
   "_page": {
      "last": "moose:e7ba778b-dace-4994-96e7-da80e7125233:2159b72c-e284-4899-b572-08da250e3614",
      "count": 2
   }
}
```

### Fetching the next batch of "newer" events from the journal

Once the client application has fetched a batch of events from the journal, it can fetch the next batch of "newer" events by supplying the `position` of the `last` event in the current batch. The `position` of the `last` event needs to be supplied back in the query parameter `since`. The API call can then be read semantically as: `GET` a batch of events `since` the given `position` in the journal. 

Instead of constructing the URL to the next batch of "newer" events, it is **strongly recommended** that the client application utilize the link provided in the response headers. Every successful response from the journaling API contains a `Link` response header with the relation type `rel=next`. The URL in the `next` link is pre populated with the `since` query parameter to fetch the next batch of "newer" events.

For example:

```
curl -X GET \
  https://api.adobe.io/events/organizations/23294/integrations/54108/f89067f2-0d50-4bb2-bf78-209d0eacb6eb?since=moose:e7ba778b-dace-4994-96e7-da80e7125233:2159b72c-e284-4899-b572-08da250e3614 \
  -H "Authorization: Bearer $USER_TOKEN" \
  -H "x-api-key: $API_KEY"
```

```
HTTP/1.1 200 OK
Link: </events/organizations/23294/integrations/54108/f89067f2-0d50-4bb2-bf78-209d0eacb6eb?since=rabbit:f9645ec8-34f2-4188-bf6e-cea4b2784fda:7dd9e3c4-0d3f-42d5-abb4-1776e209b080>; rel="next"

{
   "events":[
      {
         "position":"rabbit:f9645ec8-34f2-4188-bf6e-cea4b2784fda:7dd9e3c4-0d3f-42d5-abb4-1776e209b080",
         "event":{
            "@id":"urn:oeid:aem:f6851819-5fb7-4232-ad25-f523ae44186c",
            "@type":"xdmCreated",
            "activitystreams:published":"2018-03-01T17:54:14-08",
            "activitystreams:to":{
               "@type":"xdmImsOrg",
               "xdmImsOrg:id":"01DC1FC45956A5810A494138@AdobeOrg"
            },
            "activitystreams:generator":{
               "@type":"xdmContentRepository",
               "xdmContentRepository:root":"http://localhost-roberto-aem63:4502"
            },
            "activitystreams:actor":{
               "@id":"healthcheck-user",
               "@type":"xdmAemUser"
            },
            "activitystreams:object":{
               "@type":"xdmAsset",
               "xdmAsset:asset_name":"healthcheck.png",
               "xdmAsset:path":"/content/dam/healthcheck.png",
               "xdmAsset:format":"image/png"
            },
            "xdmEventEnvelope:objectType":"xdmAsset"
         }
      }
   ],
   "_page": {
      "last": "rabbit:f9645ec8-34f2-4188-bf6e-cea4b2784fda:7dd9e3c4-0d3f-42d5-abb4-1776e209b080",
      "count": 1
   }
}
```

### Fetching events from the end of the journal

By continuously iterating through the journal and consuming "newer" events, eventually a client application will reach the "end" of the journal. The "end" of the journal corrsponds to that position of the journal, where there are no "newer" events yet. Hence, if a client application tries to fetch events "newer" than the "end" position, no events will be returned - just a `204 No Content` response.

```
curl -X GET \
  https://api.adobe.io/events/organizations/23294/integrations/54108/f89067f2-0d50-4bb2-bf78-209d0eacb6eb?since=rabbit:f9645ec8-34f2-4188-bf6e-cea4b2784fda:7dd9e3c4-0d3f-42d5-abb4-1776e209b080 \
  -H "Authorization: Bearer $USER_TOKEN" \
  -H "x-api-key: $API_KEY"
```

```
HTTP/1.1 204 No Content
retry-after: 10
Link: </events/organizations/23294/integrations/54108/f89067f2-0d50-4bb2-bf78-209d0eacb6eb?since=rabbit:f9645ec8-34f2-4188-bf6e-cea4b2784fda:7dd9e3c4-0d3f-42d5-abb4-1776e209b080>; rel="next"
Link: </events/organizations/23294/integrations/54108/f89067f2-0d50-4bb2-bf78-209d0eacb6eb/validate?since=rabbit:f9645ec8-34f2-4188-bf6e-cea4b2784fda:7dd9e3c4-0d3f-42d5-abb4-1776e209b080>; rel="validate"

```

However, after some time depending on the frequency of events in your event registration, "newer" events will be added in near real-time to the end of the journal. These events can then be fetched by calling the same URL that earlier returned a `204 No Content` response, but this time it will return a `200 OK` response with a list of events in the response body.

For the benefit of the client application whenever the client tries to fetch events `since` the "end" position in the journal, the `next` link in the `204 No Content` is the same as the current request URL. Hence, the client application can always rely on the `next` link to iterate through the journal. And whenever it receives a `204 No Content` response it should just back off by the number of seconds specified in the `retry-after` response header and then continue pulling events from the `next` link.

```
curl -X GET \
  https://api.adobe.io/events/organizations/23294/integrations/54108/f89067f2-0d50-4bb2-bf78-209d0eacb6eb?since=rabbit:f9645ec8-34f2-4188-bf6e-cea4b2784fda:7dd9e3c4-0d3f-42d5-abb4-1776e209b080 \
  -H "Authorization: Bearer $USER_TOKEN" \
  -H "x-api-key: $API_KEY"
```

```
HTTP/1.1 200 OK
Link: </events/organizations/23294/integrations/54108/f89067f2-0d50-4bb2-bf78-209d0eacb6eb?since=penguin:41322b44-c2e9-4b44-8354-ba2173064d24:752f6e67-d7e4-48d3-9f51-452936268fbb>; rel="next"

{
   "events":[
      {
         "position":"penguin:41322b44-c2e9-4b44-8354-ba2173064d24:752f6e67-d7e4-48d3-9f51-452936268fbb",
         "event":{
            "@id":"urn:oeid:aem:5492d8d4-6fca-4d6c-a788-043ec4bf32af",
            "@type":"xdmCreated",
            "activitystreams:published":"2018-03-01T19:33:11-08",
            "activitystreams:to":{
               "@type":"xdmImsOrg",
               "xdmImsOrg:id":"01DC1FC45956A5810A494138@AdobeOrg"
            },
            "activitystreams:generator":{
               "@type":"xdmContentRepository",
               "xdmContentRepository:root":"http://localhost-roberto-aem63:4502"
            },
            "activitystreams:actor":{
               "@id":"healthcheck-user",
               "@type":"xdmAemUser"
            },
            "activitystreams:object":{
               "@type":"xdmAsset",
               "xdmAsset:asset_name":"healthcheck.png",
               "xdmAsset:path":"/content/dam/healthcheck.png",
               "xdmAsset:format":"image/png"
            },
            "xdmEventEnvelope:objectType":"xdmAsset"
         }
      }
   ],
   "_page": {
      "last": "penguin:41322b44-c2e9-4b44-8354-ba2173064d24:752f6e67-d7e4-48d3-9f51-452936268fbb",
      "count": 1
   }
}
```

## Controlling the response

### Limiting the size of the batch

Depending on the frequency of the events in your registration, the number of events returned in a single response batch varies. A batch of events will always have one event but there is no upper limit to the number of events that can be returned in a single batch. In case, a client application wishes to set an upper bound, it needs to supply the `limit` query parameter with the maximum number of events that can be returned.

Once a `limit` query parameter is supplied, the value of the parameter is retained in the `next` link as well - hence, the client application can use the `next` link as is without needing to construct it. The `limit` query parameter can be combined with any other query parameter, just make sure to pass a positive integer as the value.

For example, here is the same request as before but we have limited the number of events returned to 1:


```
curl -X GET \
  https://api.adobe.io/events/organizations/23294/integrations/54108/f89067f2-0d50-4bb2-bf78-209d0eacb6eb?limit=1 \
  -H "Authorization: Bearer $USER_TOKEN" \
  -H "x-api-key: $API_KEY"
```

```
HTTP/1.1 200 OK
Link: </events/organizations/23294/integrations/54108/f89067f2-0d50-4bb2-bf78-209d0eacb6eb?since=camel:5aeb25cc-1b15-4f26-a082-9c213005dba8:ff244403-ca7c-4993-bbda-3c8915ce0b32&limit=1>; rel="next"

{
   "events":[
      {
         "position":"camel:5aeb25cc-1b15-4f26-a082-9c213005dba8:ff244403-ca7c-4993-bbda-3c8915ce0b32",
         "event":{
            "@id":"urn:oeid:aem:199e85da-dd54-4966-95ba-13cf544964dc",
            "@type":"xdmCreated",
            "activitystreams:published":"2018-03-06T15:19:03-08",
            "activitystreams:to":{
               "@type":"xdmImsOrg",
               "xdmImsOrg:id":"01DC1FC45956A5810A494138@AdobeOrg"
            },
            "activitystreams:generator":{
               "@type":"xdmContentRepository",
               "xdmContentRepository:root":"http://localhost-roberto-aem63:4502"
            },
            "activitystreams:actor":{
               "@id":"healthcheck-user",
               "@type":"xdmAemUser"
            },
            "activitystreams:object":{
               "@type":"xdmAsset",
               "xdmAsset:asset_name":"healthcheck.png",
               "xdmAsset:path":"/content/dam/healthcheck.png",
               "xdmAsset:format":"image/png"
            },
            "xdmEventEnvelope:objectType":"xdmAsset"
         }
      }
   ],
   "_page": {
      "last": "camel:5aeb25cc-1b15-4f26-a082-9c213005dba8:ff244403-ca7c-4993-bbda-3c8915ce0b32",
      "count": 1
   }
}
```

NOTE: The `limit` query parameter DOES NOT guarantee that the number of events returned will always be equal to the value supplied. That is true even if there are more events to be consumed in the journal. It only serves as a way to specify an upper bound on the count of events.

For example, our journal above has at least 4 events that we know of, however, even when the `limit` parameter is supplied with the value `3`, we do not get that many events in the respsonse.


```
curl -X GET \
  https://api.adobe.io/events/organizations/23294/integrations/54108/f89067f2-0d50-4bb2-bf78-209d0eacb6eb?limit=3 \
  -H "Authorization: Bearer $USER_TOKEN" \
  -H "x-api-key: $API_KEY"
```

```
HTTP/1.1 200 OK
Link: </events/organizations/23294/integrations/54108/f89067f2-0d50-4bb2-bf78-209d0eacb6eb?since=moose:e7ba778b-dace-4994-96e7-da80e7125233:2159b72c-e284-4899-b572-08da250e3614&limit=3>; rel="next"

{
   "events":[
      {
         "position":"camel:5aeb25cc-1b15-4f26-a082-9c213005dba8:ff244403-ca7c-4993-bbda-3c8915ce0b32",
         "event":{
            "@id":"urn:oeid:aem:199e85da-dd54-4966-95ba-13cf544964dc",
            "@type":"xdmCreated",
            "activitystreams:published":"2018-03-06T15:19:03-08",
            "activitystreams:to":{
               "@type":"xdmImsOrg",
               "xdmImsOrg:id":"01DC1FC45956A5810A494138@AdobeOrg"
            },
            "activitystreams:generator":{
               "@type":"xdmContentRepository",
               "xdmContentRepository:root":"http://localhost-roberto-aem63:4502"
            },
            "activitystreams:actor":{
               "@id":"healthcheck-user",
               "@type":"xdmAemUser"
            },
            "activitystreams:object":{
               "@type":"xdmAsset",
               "xdmAsset:asset_name":"healthcheck.png",
               "xdmAsset:path":"/content/dam/healthcheck.png",
               "xdmAsset:format":"image/png"
            },
            "xdmEventEnvelope:objectType":"xdmAsset"
         }
      },
      {
         "position":"moose:e7ba778b-dace-4994-96e7-da80e7125233:2159b72c-e284-4899-b572-08da250e3614",
         "event":{
            "@id":"urn:oeid:aem:61930ae6-ff2a-48fb-881d-078e862c3811",
            "@type":"xdmCreated",
            "activitystreams:published":"2018-03-06T15:19:59-08",
            "activitystreams:to":{
               "@type":"xdmImsOrg",
               "xdmImsOrg:id":"01DC1FC45956A5810A494138@AdobeOrg"
            },
            "activitystreams:generator":{
               "@type":"xdmContentRepository",
               "xdmContentRepository:root":"http://localhost-roberto-aem63:4502"
            },
            "activitystreams:actor":{
               "@id":"healthcheck-user",
               "@type":"xdmAemUser"
            },
            "activitystreams:object":{
               "@type":"xdmAsset",
               "xdmAsset:asset_name":"healthcheck.png",
               "xdmAsset:path":"/content/dam/healthcheck.png",
               "xdmAsset:format":"image/png"
            },
            "xdmEventEnvelope:objectType":"xdmAsset"
         }
      }
   ],
   "_page": {
      "last": "moose:e7ba778b-dace-4994-96e7-da80e7125233:2159b72c-e284-4899-b572-08da250e3614",
      "count": 2
   }
}
```

### Consuming the most recent events

The Journaling endpoint URL when supplied with no query parameters returns the oldest entries in ledger, however, sometimes a client application is not interested in consuming older events. In such a case, the client application can jump straight to the "end" of the journal and start consuming events that are written to the journal after the request was made. 

This can be done by specifying the query parameter `latest=true` on the first API call. For example:


```
curl -X GET \
  https://api.adobe.io/events/organizations/23294/integrations/54108/f89067f2-0d50-4bb2-bf78-209d0eacb6eb?latest=true \
  -H "Authorization: Bearer $USER_TOKEN" \
  -H "x-api-key: $API_KEY"
```

```
HTTP/1.1 204 No Content
retry-after: 10
Link: </events/organizations/23294/integrations/54108/f89067f2-0d50-4bb2-bf78-209d0eacb6eb?since=penguin:41322b44-c2e9-4b44-8354-ba2173064d24:752f6e67-d7e4-48d3-9f51-452936268fbb>; rel="next"
Link: </events/organizations/23294/integrations/54108/f89067f2-0d50-4bb2-bf78-209d0eacb6eb/validate?since=penguin:41322b44-c2e9-4b44-8354-ba2173064d24:752f6e67-d7e4-48d3-9f51-452936268fbb>; rel="validate"

```

In most cases, there will not be any events to consume yet and the response will be a `204 No Content` repsonse. In an extremely rare case, there might actually be events that were written in near-real time to the journal after the request was made and in such a case they will be able returned with a `200 OK` response with the same response body structure as above.

NOTE: the `latest=true` query parameter is just a way to jump to the "end" of the journal and then client applications should use the `next` links as usual to iterate over the journal from that position onward. In case, the client application continues to make requests with `latest=true`, it is very likely that they will not receive any events - jsut because it is semantically equilvalent of asking for "events from now onward". And the definition of "now" changes with every request that is made.

## Event expiry

### Purging policy

### Positon Validation
