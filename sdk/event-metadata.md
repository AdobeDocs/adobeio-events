# Event Metadata

CRUD Operations for Event Metadata
getAllEventMetadataForProvider(providerId) ⇒ Promise.<object>

Sample JSON response:

Get All Eventmetadata Sample Response
```
{ 
  _links:
        { self:
            {
                href:  'http://localhost:11111/providers/<provider-id>/event_metadata'
            }
        },
        _embedded:
        {
            event_metadata:  [
                { _links:
                    { self:
                        { href:  'http://localhost:11111/providers/<provider-id>/event_metadata/<event-code-1>' }
                    },
                    description: '<description>',
                    label: '<label>',
                    event_code: '<event-code-1>',
                    sample_event:
                    {
                        id: '<id>',
                        event_id: '<event-id>',
                        specversion: '1.0',
                        type: '<event-code-1>',
                        source: 'urn:uuid:<provider-id>',
                        time: '<datetime>',
                        datacontenttype: 'application/json',
                        data: {}
                    }
            },
            ...
         ]
    }
}
```

getEventMetadataForProvider(providerId, eventCode) ⇒ Promise.<object>

Sample JSON response:

Get Eventmetadata Sample Response
```
{ 
  _links:
    { self:
        { href:  'http://localhost:11111/providers/<provider-id>/event_metadata/<event-code>' }
    },
    description: '<description>',
    label: '<label>',
    event_code: '<event-code>',
    sample_event:
    {
        id: '<id>',
        event_id: '<event_id>',
        specversion: '1.0',
        type: '<event-code>',
        source: 'urn:uuid:<provider-id>',
        time: '2020-03-06T05:40:34Z',
        datacontenttype: 'application/json',
        data: {}
    }
}
```

createEventMetadataForProvider(consumerOrgId, projectId, workspaceId, providerId, body) ⇒ Promise.<object>

Sample request body:

Create Eventmetadata sample request body
```
{
    label: 'test-label',
    description: 'Test for SDK 1',
    event_code: 'event_code_1'
}
```

updateEventMetadataForProvider(consumerOrgId, projectId, workspaceId, providerId, eventCode, body) ⇒ Promise.<object>

One can update the description, label, sample event of the event metadata. 

deleteEventMetadata(consumerOrgId, projectId, workspaceId, providerId, eventCode) ⇒ Promise

Returns 204 once the deletion is successful. If the event_code/ provider_id does not exist, 404 is returned. 

deleteAllEventMetadata(consumerOrgId, projectId, workspaceId, providerId) ⇒ Promise

Returns 204 once the deletion is successful. If the provider_id does not exist, 404 is returned. 