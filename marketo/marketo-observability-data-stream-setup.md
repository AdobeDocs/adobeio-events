# Setting up the Marketo Observability Data Stream

These instructions describe how to set up and get started using Adobe I/O Events to subscribe to Marketo Observability events.

- [Introduction](#introduction)
- [Pre-Requisite Setup](#pre-requisite-setup)
- [Setup Adobe I/O](#setup-adobe-i/o)
- [Developer Guidelines](#developer-guidelines)
- [Debug](#debug)

## Introduction

The Observability Data Stream provides insight into the data flow into Marketo through the Data Ingestion Service, whether coming from AEP or directly called via the public API.  The stream offers several different event types:

- Metrics - Periodic aggregated metrics showing all data sent to the API and processed into Marketo
- Results - Per API request results for tracking and verification
- Status - Periodic notices providing insight into the remaining API quota and any request processing backlog

Note: MODS (Marketo Observability Data Stream) is currently a Beta Product

## Pre-Requisite Setup

In order to subscribe to the data stream you need to have an Adobe Org with the Experience Cloud Entitlement, a Marketo subscription, and for the Marketo subscription to be mapped to the Adobe Org.

## Setup Adobe I/O

See [Getting Started with Adobe I/O Events](../readme.md)

For basic instructions for this use case, starting from [console.adobe.io](https://console.adobe.io/):

*When prompted, click the designated button to proceed*

- Select `Create new project`

  ![Create new project](../img/UserAuditDataStreamIOSetup1.png "Quick Start")

- Select `Add event`

  ![Add event](../img/UserAuditDataStreamIOSetup2.png "Get started with your new project by adding an event subscription")

- Filter by `Experience Cloud`
- Select `Marketo Observability Data Stream`

  ![Provider selection](../img/MarketoObservability1.png "Select event provider")

- Subscribe to the user driven change events of your choosing

  ![Event selection](../img/MarketoObservability2.png "Select event subscriptions")

- Set up authentication (either JWT or OAuth) to be used for accessing the Journaling API

  ![Set up credentials](../img/MarketoObservability3.png "Set up credentials")

- Set up Event Registration

  ![Complete registration](../img/MarketoObservability4.png "Complete registration")

  - Provide a name and description for this event subscription
  - Optionally choose whether to enable Webhook or Runtime action
    - Enable Webhook
      - We recommend batch over single webhooks
      - For `Webhook URL` a public https endpoint must be provided
      - The endpoint much be able to handle get and post requests
      - The get request must respond with the challenge query if it exists
      - The post request must respond that it received the message or the webhook will re-attempt to send several times before giving up and automatically disabling the webhook sends
    - Enable Runtime action
      - [See Setting up your Runtime Environment](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/getting-started/setup.md)
      - Select a pre-made runtime action/runtime namespace
- After Saving

  ![Verify setup](../img/MarketoObservability5.png "Verify setup")
  
  - Verify that the Status is `Active`
  - If Webhook was selected, verify that it successfully passed the challenge without errors

## Developer Guidelines

When setting up a project to subscribe to events, there are three ways to interact with those event subscriptions in order to receive the events.  The first is Journaling, which provides a pull model in which events can be pulled via API and stores up to 7 days of past events.  The second is Webhooks, which can be configured to send events either as single events or batched to a webhook endpoint in near real-time with the event occurrence.  Third is Runtime, where you can set up your own custom function within Adobe that events will automatically run through near-real time.

### Journaling
[Getting Started with Journaling](../intro/journaling_intro.md)

Important Takeaways:

- Stores up to 7 days of history
- Can be iterated through from any previous event within the history
- Will still receive and store events even if webhook is failing
- Useful for fetching events that were missed due to webhook issues or for a pulling mechanism instead of webhook push

### Webhooks
[Getting Started with Adobe I/O Events Webhooks](../intro/webhooks_intro.md)

Webhook Endpoint Requirements:

- Handle GET and POST requests
- Respond with a 200-type response within a reasonable time period
- Challenge Request
  - GET request with challenge query parameter
  - Must respond with value of challenge query parameter
- Webhook Events
  - POST request with JSON data body with one or more events
  - *Recommended to set up webhook as batch*

### Event Data Structure Examples

Events are structured in JSON format using the [CloudEvents](https://cloudevents.io/) spec

#### Metrics

    {
        "id": "b90382d8-6b23-11ee-b962-0242ac120002",
        "specversion": "1.0",
        "type": "com.adobe.platform.marketo.input.stream.output.metrics",
        "source": "urn:data_ingestion_service",
        "time": "2023-08-14T18:00:00Z",
        "datacontenttype": "application/json",
        "data": {
            "munchkinId": "123-ABC-456",
            "requests": {
                "received": 3,
                "processed": 1,
                "rejected": {  
                    "606": 1,
                    "607": 1
                }
            },
            "records": {
                "person": {
                    "high": {
                        "received": 100,
                        "created": 90,
                        "updated": 5,
                        "failed": {
                            "503": 5
                        }
                    },
                    "normal": {
                        "received": 100,
                        "created": 90,
                        "updated": 5,
                        "failed": {
                            "503": 5
                        }
                    }
                },
                "customObjects": {
                    "high": {
                        "received": 100,
                        "created": 90,
                        "updated": 5,
                        "failed": {
                            "503": 5
                        }
                    },
                    "normal": {
                        "received": 100,
                        "created": 90,
                        "updated": 5,
                        "failed": {
                            "503": 5
                        }
                    }
                }
            }
        }
    }

#### Results

    {
        "id": "b90382d8-6b23-11ee-b962-0242ac120002",
        "specversion": "1.0",
        "type": "com.adobe.platform.marketo.input.stream.output.results",
        "source": "urn:data_ingestion_service",
        "time": "2023-08-14T17:30:00Z",
        "datacontenttype": "application/json",
        "data": {
            "munchkinId": "123-ABC-456"
            "requests": [
                {
                    "requestId": "cf0a1a20-668e-492a-8ec2-ce8747507068",
                    "requestTime": "2023-08-14T17:20:00Z",
                    "clientId": "foo@marketo.com",
                    "correlationId": "6180bb48-8dc7-4fc5-85ca-a59dd1edb0f3",
                    "origin": "Adobe Journey Optimizer",
                    "objectType": "person",
                    "priority": "high",
                    "records": {
                        "received": 100,
                        "created": 90,
                        "updated": 10,
                        "failed": {}
                    }
                },
                {
                    "requestId": "67b28858-f5ee-45ac-aa40-63a04085e6be",
                    "requestTime": "2023-08-14T17:20:00Z",
                    "clientId": "foo@marketo.com",
                    "correlationId": "6180bb48-8dc7-4fc5-85ca-a59dd1edb0f3",
                    "origin": "Public API",
                    "objectType": "customObject",
                    "priority": "normal",
                    "records": {
                        "received": 100,
                        "created": 10,
                        "updated": 10,
                        "failed": {
                            "503": 10,
                            "404": 10
                        }
                    }
                }
            ]
        }
    }

#### Status

    {
        "id": "b90382d8-6b23-11ee-b962-0242ac120002",
        "specversion": "1.0",
        "type": "com.adobe.platform.marketo.input.stream.output.status",
        "source": "urn:data_ingestion_service",
        "time": "2023-08-14T18:00:00Z",
        "datacontenttype": "application/json",
        "data": {
            "munchkinId": "123-ABC-456",
            "clientId": "foo@marketo.com",
            "quota": 65432100,
            "queue": {
                "high": {
                    "lagSeconds": "2023-08-14T17:31:00Z",
                    "requestBacklog": 1,
                    "recordBacklog": 100
                },
                "normal": {
                    "lagSeconds": "2023-08-14T17:31:00Z",
                    "requestBacklog": 10,
                    "recordBacklog": 1000
                }
            },
        }
    }

#### Data Field Definitions

Field | Description
--- | ---
id | Unique UUID generated per event
specversion | CloudEvents version specification being used
type | Type of event used for event subscription routing
source | Context in which an event happened
time | Timestamp of the completion of the action
datacontenttype | Content type of the data object
data | Event data object
munchkinId | Internal Marketo subscription identifier
clientId | Marketo API User Id or Adobe IMS Client Id
correlationId | An id representation of where the request originated from (empty if from public API)
origin | Name representation of the correlationId component
objectType | Marketo object type that's being processed (account, custom object, person, etc.)
priority | Priority of the request (high or normal)
received | Count of requests/records received to the Data Ingest Service 
created | Count of records created from processing the requests to the Data Ingest Service
updated | Count of records updated from processing the requests to the Data Ingest Service
failed | Count of records failed while processing the requests to the Data Ingest Service broken down by error code
rejected | Count of requests rejected (never accepted by the Ingest Service) by error code
quota | Remaining daily quota for the Data Ingest Service
queue | Stats of the processing queue backlog
lagSeconds | Difference between now and the oldest message in the processing queue
requestBacklog | Number of requests in the processing queue
recordBacklog | Number of records in the processing queue
recipient_client_id | Auto generated by IO Events, can be used for security checks
event_id | Auto generated by IO Events, it matches the x-adobe-event-id header

### Debug

[Debug Tracing](../support/tracing.md)

Once you have successfully completed your setup and event subscription registration, events should start being stored in the journal.  In addition, if you have webhooks or runtime set up, the events will go through those flows.  From the project's page in the event registration details, you should see a tab for Debug Tracing.  For webhooks, this will show a record of failed and successful challenge attempts as well as webhook attempts.  Each request includes the request/response details to help debug.