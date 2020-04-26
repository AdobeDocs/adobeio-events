# Providers



`getAllProviders(consumerOrgId) ⇒ Promise.<object>`

Get the list of all providers that are applicable to the organization. Here consumerOrgId is the AMS Org Id.

Sample JSON response:

Get All Providers Sample Response
```
{
  _links: {
    self: {
      href: 'http://localhost:11111/<consumerOrgId>/providers'
    }
  },
  _embedded: {
    providers: [
      {
        _links: {
          'provider:event_metadata': {
            href: 'http://localhost:11111/providers/<providerId>/eventmetadata'
          },
          self: {
            href: 'http://localhost:11111/providers/<providerId>'
          }
        },
        id: '<providerId>',
        label: '<label>',
        source: 'urn:uuid:<providerId>',
        publisher: '<imsOrgId>'
      },
     ...
    ]
  }
}
```

`createProvider(consumerOrgId, projectId, workspaceId, body) ⇒ Promise.<object>`

Creating a provider is very simple. All it needs is a unique label for the provider which will be the name that appears on the console.

Sample request body:

Request Body for creating a Provider
```
{
    label: 'Test Provider SDK'
}
```

`getProvider(providerId) ⇒ Promise.<object>`

Get the details of the provider with the specified provider Id. The "source" is URI to be used while publishing events to the event receiver. 

Sample JSON response:

Get Provider Sample Response

```
{ _links:
    { 'provider:event_metadata': 
        {
            href:  'http://localhost:11111/providers/<providerId>/event_metadata'
        },
        self:
       {
            href:  'http://localhost:11111/providers/<providerId>'
        }
    },
    id: '<providerId>',
    label: '<label>',
    source: 'urn:uuid:<providerId>',
    publisher: '<publisher>'
}
```

`updateProvider(consumerOrgId, projectId, workspaceId, providerId, body) ⇒ Promise.<object>`

The label of the provider can be updated. 

`deleteProvider(consumerOrgId, projectId, workspaceId, providerId) ⇒ Promise.<object>`

Returns 204 once the deletion is successful. If the provider does not exist, 404 is returned. 