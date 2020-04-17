# Events SDK Overview

The Adobe I/O Events SDK is an open-source SDK for the use of third party customers. Third-party APIs in CSM requires bare minimum information from the users so that they can register providers and event metadata with ease, and register a webhook or journalling endpoint and listen to events. They can then start publishing messages to the event receiver for their org and listen to those events by either polling via journalling or by allowing the webhook to be notified about events. 

The Events SDK provides a wrapper over these API calls making it easier for developers to use it as part of their apps and get started with I/O Events more easily. 

The SDK can be used to perform the following operations:

* Create, Update, Delete providers.
* Get All providers for an organization.
* Create, Update, Delete event metadata.
* Get all event metadata for a provider.
* Create a webhook/ journal registration.
* Get all registrations for an integration.
* Publish events to the event receiver.
* Get events from journalling.
* Get an observable to start listening to events from journalling using a poller.
* Signature verifications for events delivered via webhook endpoint.

## Next Steps


