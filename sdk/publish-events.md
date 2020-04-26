# Publish events

`publishEvent(cloudEvent) ⇒ Promise.<string>`

Event publishers can publish events to the event receiver using this SDK. 

The events should follow Cloud Events 1.0 Image result for cloud events specification: https://github.com/cloudevents/spec/blob/v1.0/spec.md. 

As of now, only `application/json` is accepted as the `content-type` for the "data" field of the cloud event. If retries are set, publish events are retried on network issues, 5xx and 429 error response codes. 

### Cloud Events Sample

A sample cloud event accepted by the event receiver:

```
{
    id: '<id>',
    event_id: '<event_id>',
    specversion: '1.0',
    type: '<event-code>',
    source: 'urn:uuid:<provider-id>',
    time: '2020-03-06T05:40:34Z',
    datacontenttype: 'application/json',
    data: { hello: 'world' } // any json payload
}
```

The API returns 200 OK if there the event has been processed correctly and there are active registrations for the event and 204 in case there are no registrations for the event. In addition, it returns the appropriate error codes if there was an issue in processing the request. 

