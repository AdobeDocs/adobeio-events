# Event Metadata

For information on installing and using the SDK, please begin by reading the [getting started guide](getting-started.md).

## Get All Event Metadata for Provider

This method returns a list of all event metadata for a given provider.

#### Method

```javascript
getAllEventMetadataForProvider(providerId) ⇒ Promise.<object>
```

|Parameter	|Type	|Description|
|---|---|---|
|`providerId`	|string	|The ID that uniquely identifies the provider whose event metadata is to be fetched.|

#### Sample Response

```json
{ 
  "_links":
        { "self":
            {
                "href":  "http://localhost:11111/providers/<provider-id>/event_metadata"
            }
        },
        "_embedded":
        {
            "event_metadata":  [
                { "_links":
                    { "self":
                        { "href":  "http://localhost:11111/providers/<provider-id>/event_metadata/<event-code>" }
                    },
                    "description": "<description>",
                    "label": "<label>",
                    "event_code": "<event-code>",
                    "sample_event":
                    {
                        "id": "<id>",
                        "event_id": "<event-id>",
                        "specversion": "1.0",
                        "type": "<event-code-1>",
                        "source": "urn:uuid:<provider-id>",
                        "time": "<datetime>",
                        "datacontenttype": "application/json",
                        "data": {}
                    }
            },
            ...
         ]
    }
}
```

## Get Event Metadata for Given Provider and Event Code

You can return all event data by providing a provider ID and an event code.

#### Method

```javascript
getEventMetadataForProvider(providerId, eventCode) ⇒ Promise.<object>
```

|Parameter|	Type	|Description|
|---|---|---|
|`providerId`	|string|The ID that uniquely identifies the provider whose event metadata is to be fetched.|
|`eventCode`	|string	|The specific event code for which the details of the event metadata is to be fetched.|

#### Sample Response

```json
{ 
  "_links":
    { "self":
        { "href":  "http://localhost:11111/providers/<provider-id>/event_metadata/<event-code>" }
    },
    "description": "<description>",
    "label": "<label>",
    "event_code": "<event-code>",
    "sample_event":
    {
        "id": "<id>",
        "event_id": "<event_id>",
        "specversion": "1.0",
        "type": "<event-code>",
        "source": "urn:uuid:<provider-id>",
        "time": "2020-03-06T05:40:34Z",
        "datacontenttype": "application/json",
        "data": {}
    }
}
```

## Create Event Metadata for a Provider

#### Method

```javascript
createEventMetadataForProvider(consumerOrgId, projectId, workspaceId, providerId, body) ⇒ Promise.<object>
```

|Parameter	|Type	|Description|
|---|---|---|
|`consumerOrgId`	|string	|Consumer Organization ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`projectId`	|string	|Project ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`workspaceId`	|string	|Workspace ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`providerId`	|string	|Provider ID for which the event metadata is to be added.|
| [body](#sample-creation-request-body)	|object	|JSON data that describes the event metadata.|

#### Sample Creation Request Body

```json
{
    "label": "test-label",
    "description": "Test for SDK 1",
    "event_code": "event_code_1"
}
```

## Update Event Metadata for a Provider

You can update the description and label of the event metadata by providing the event code of the event metadata to be updated. 

#### Method

```javascript
updateEventMetadataForProvider(consumerOrgId, projectId, workspaceId, providerId, eventCode, body) ⇒ Promise.<object>
```

|Parameter	|Type	|Description|
|---|---|---|
|`consumerOrgId`	|string	|Consumer Organization ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`projectId`	|string	|Project ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`workspaceId`	|string	|Workspace ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`providerId`	|string	|Provider ID for which the event metadata is to be added.|
|`eventCode`| string|Event Code of the event metadata to be updated.|
| [body](#sample-update-request-body)	|object	|JSON data that describes the updated event metadata.|

#### Sample Update Request Body

```json
{
    "label": "new-label",
    "description": "Updated description for SDK 1"
}
```

## Delete Event Metadata

You can delete metadata for a specific event by providing the event code along with the associated provider ID.

#### Method

```javascript
deleteEventMetadata(consumerOrgId, projectId, workspaceId, providerId, eventCode) ⇒ Promise
```

|Parameter	|Type	|Description|
|---|---|---|
|`consumerOrgId`	|string	|Consumer Organization ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`projectId`	|string	|Project ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`workspaceId`	|string	|Workspace ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`providerId`	|string	|Provider ID for which the event metadata is to be deleted.|
|`eventCode`| string|Event Code of the event metadata to be deleted.|

#### Response

Returns HTTP Status 204 (No Content) once the deletion is successful. If the `eventCode` or `providerId` does not exist, HTTP Status 404 (Not Found) is returned.

## Delete All Event Metadata

You can delete all event metadata for a provider by specifying a provider ID.

#### Method

```javascript
deleteAllEventMetadata(consumerOrgId, projectId, workspaceId, providerId) ⇒ Promise
```

|Parameter	|Type	|Description|
|---|---|---|
|`consumerOrgId`	|string	|Consumer Organization ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`projectId`	|string	|Project ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`workspaceId`	|string	|Workspace ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`providerId`	|string	|Provider ID for which the event metadata is to be deleted.|

#### Response

Returns HTTP Status 204 (No Content) once the deletion is successful. If the `providerId` does not exist, HTTP Status 404 (Not Found) is returned.