# Providers

For information on installing and using the SDK, please begin by reading the [getting started guide](getting-started.md).

## List All Providers

Get the list of all providers that are applicable to the organization. Here consumerOrgId is the AMS Org Id.

#### Method

```javascript
getAllProviders(consumerOrgId) ⇒ Promise.<object>`
```

|Parameters	|Type	|Description|
|---|---|---|
|`consumerOrgId`	|string	|Consumer Organization Id from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|

#### Sample Response

Returns a list of all providers. This response has been truncated to show only the first provider returned.

```json
{
  "_links": {
    "self": {
      "href": "http://localhost:11111/<consumerOrgId>/providers"
    }
  },
  "_embedded": {
    "providers": [
      {
        "_links": {
          "provider:event_metadata": {
            "href": "http://localhost:11111/providers/<providerId>/eventmetadata"
          },
          "self": {
            "href": "http://localhost:11111/providers/<providerId>"
          }
        },
        "id": "<providerId>",
        "label": "<label>",
        "source": "urn:uuid:<providerId>",
        "publisher": "<imsOrgId>"
      },
     ...
    ]
  }
}
```

## Create a Provider

Create a new provider based on the given provider details.

#### Method

```javascript
createProvider(consumerOrgId, projectId, workspaceId, body) ⇒ Promise.<object>
```

|Parameter	|Type	|Description|
|---|---|---|
|`consumerOrgId`	|string	|Consumer Organization ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`projectId`	|string	|Project ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`workspaceId`	|string	|Workspace ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`providerId`	|string	|Provider ID for which the event metadata is to be added.|
| [body](#sample-request-body)	|object	|JSON data that describes the event metadata.|

#### Sample Request Body

Creating a provider requires a unique label for the provider which will be the name that appears in [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).

```json
{
    "label": "Sample Event Provider"
}
```

#### Response

Returns the details of the newly created provider.

## Get Provider Details

Get the details of a specific provider by its provider ID.  

#### Method

```javascript
getProvider(providerId) ⇒ Promise.<object>
```

|Parameters|Type	|Default	|Description|
|---|---|---|---|
|`providerId`	|string		||The ID that uniquely identifies the provider to be fetched.|
|[fetchEventMetadata]	|boolean	|`false`	|Set this to `true` if you want to fetch the associated eventmetadata of the provider.|

#### Sample Response

Returns the details of the provider specified by the provider ID. The "source" value is the URI to be used while publishing events to the event receiver.

```json
{ "_links":
    { "provider:event_metadata": 
        {
            "href":  "http://localhost:11111/providers/<providerId>/event_metadata"
        },
        "self":
       {
            "href":  "http://localhost:11111/providers/<providerId>"
        }
    },
    "id": "<providerId>",
    "label": "<label>",
    "source": "urn:uuid:<providerId>",
    "publisher": "<publisher>"
}
```

## Update Provider 

Update the label of a provider based on its ID.

#### Method

```javascript
updateProvider(consumerOrgId, projectId, workspaceId, providerId, body) ⇒ Promise.<object>
```

|Parameter	|Type	|Description|
|---|---|---|
|`consumerOrgId`	|string	|Consumer Organization ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`projectId`	|string	|Project ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`workspaceId`	|string	|Workspace ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`providerId`	|string	|The ID that identifies the provider to be updated.|
| [body](#sample-request-body)	|object	|JSON data that describes the provider.|

#### Response

Returns the details of the updated provider.

## Delete a Provider

You can delete a provider by specifying a provider ID.

#### Method

```javascript
deleteProvider(consumerOrgId, projectId, workspaceId, providerId) ⇒ Promise.<object>
```

|Parameter	|Type	|Description|
|---|---|---|
|`consumerOrgId`	|string	|Consumer Organization ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`projectId`	|string	|Project ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`workspaceId`	|string	|Workspace ID from [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui).|
|`providerId`	|string	|The ID that identifies the provider to be deleted.|

#### Response

Returns HTTP Status 204 (No Content) once the deletion is successful. If the provider does not exist, HTTP Status 404 (Not Found) is returned. 