# Webhooks

## Create a webhook or journal registration

`createWebhookRegistration(consumerOrgId, integrationId, body) ⇒ Promise.<object>`

|Param	|Type	|Description|
|---|---|---|
|consumerOrgId	|string	|Consumer Org Id from the console integration URL|
|integrationId	|string	|Integration Id from the console integration URL|
|body	|object	|Json data contains details of the registration|

There are two ways to consume events. 1. Webhooks 2. Journalling. 

You can register a webhook endpoint by providing the `webhook_url` as part of the request body. If you want to register only a journal URL, you can set the `delivery_type` to `'JOURNAL'` in the request body and leave the `webhook_url` empty.

### Sample JSON request body to register a journal URL

The request body includes a name, description, client ID, and delivery type ("JOURNAL" or "WEBHOOK"), as well as an array listing each "event of interest" as an individual object containing the event code and provider ID.

```json
{
	"name": "<name>",
	"description": "<desc>",
	"client_id": "<client_id>",
	"delivery_type": "JOURNAL",
	"events_of_interest": 
  [
    {
		  "event_code": "<event_code>",
		  "provider_id": "<provider_id>"
	  }
  ]
}
```
For more information on journalling - [journalling](journalling.md).

### Sample JSON request body to register a webhook URL

```json
{
    name: '<name>',
    description: '<desc>',
    client_id: '<client_id>',
    webhook_url: '<url>',
    events_of_interest: [
    {
        event_code: '<event_code>',
        provider_id: '<provider_id>'
    },
    ... < list of event code and provider id that you want to subscribe to >
  ]
}
```

## View webhook registration details

Get details of a webhook registration

`getWebhookRegistration(consumerOrgId, integrationId, registrationId) ⇒ Promise.<object>`

|Param	|Type	|Description|
|---|---|---|
|consumerOrgId	|string	|Consumer Org Id from the console integration URL|
|integrationId	|string	|Integration Id from the console integration URL|
|registrationId	|string	|Registration id whose details are to be fetched|


### Get webhook registration sample response

```json
{
        "id": 248713,
        "name": "<name>",
        "description": "<desc>",
        "client_id": "<client_id>",
        "parent_client_id": "<client_id>",
        webhook_url: '<url>',
        status: 'VERIFIED',
        type: 'APP',
        integration_status: 'ENABLED',
        events_of_interest:
        [  
            {
                event_code: '<event_code>',
                event_label: '<label>',
                event_description: '<event_desc>',
                provider_id: '<provider_id>',
                provider: '<provider_name>',
                provider_label: '<provider label>',
                event_delivery_format: 'cloud_events'
            }
        ],
        registration_id: '<reg_id>',
        delivery_type: '<WEBHOOK/JOURNAL>',
        events_url: '<journal_url>',
        created_date: '2020-02-21T07:31:24.000Z',
        updated_date: '2020-02-21T07:31:24.000Z',
        runtime_action: ' '
 }
```

## List all webhook registrations

Get the list of all registrations for the org and integration id. 

`getAllWebhookRegistrations(consumerOrgId, integrationId) ⇒ Promise.<object>`

|Param	|Type	|Description|
|---|---|---|
|consumerOrgId	|string	|Consumer Org Id from the console integration URL|
|integrationId	|string	|Integration Id from the console integration URL|

### List all webhook registrations response

The response array contains an object providing the details for each webhook registration. This response has been truncated to show only the first item in the array.

```json
[
    {
        id: 248713,
        name: '<name>',
        description: '<desc>',
        client_id: '<client_id>',
        parent_client_id: '<client_id>',
        webhook_url: '<url>',
        status: 'VERIFIED',
        type: 'APP',
        integration_status: 'ENABLED',
        events_of_interest:
        [  
            {
                event_code: '<event_code>',
                event_label: '<label>',
                event_description: '<event_desc>',
                provider_id: '<provider_id>',
                provider: '<provider_name>',
                provider_label: '<provider label>',
                event_delivery_format: 'cloud_events'
            }
        ],
        registration_id: '<reg_id>',
        delivery_type: '<WEBHOOK/JOURNAL>',
        events_url: '<journal_url>',
        created_date: '2020-02-21T07:31:24.000Z',
        updated_date: '2020-02-21T07:31:24.000Z',
        runtime_action: ' '
    },
    ...
]
```