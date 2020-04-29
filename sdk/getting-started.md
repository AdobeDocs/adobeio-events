# Getting Started with the Adobe I/O Events SDK

The Adobe I/O Events SDK is an open-source SDK for the use of third party customers. I/O Events provide a powerful way to integrate your applications with Adobe services and solutions. Events are triggered by an Event Provider, like Adobe Creative Cloud Assets, whenever a certain real-world action occurs, such as creating a new asset.

To start listening for events, your application registers a webhook, specifying which Event Types from which Event Providers it wants to receive. Whenever a matching event gets triggered, your application is notified through an HTTP POST request. 

## Classes

### `EventsCoreAPI`

The `EventsCoreAPI` class provides methods to call your Adobe I/O Events APIs. Before calling any method initialize the instance by calling the `init` method on it with valid values for `organizationId`, `apiKey`, `accessToken` and optional HTTP Options such as `timeout` and max number of `retries`.

## Functions

```
init(organizationId, apiKey, accessToken, [httpOptions]) ⇒ Promise.<EventsCoreAPI>
```

A global function that returns a Promise that resolves with a new `EventsCoreAPI` object.

|Param	|Type	|Description|
|---|---|---|
|`organizationId`	|string	|The IMS Organization Id for which the provider, event metadata, etc are to be created which can be obtained using the Console or Transporter API. |
|`apiKey`	|string	|The API Key (Client ID) for the project or workspace.|
|`accessToken`	|string	|A JSON Web Token (JWT). I/O Management Service needs to be enabled for the project or workspace. Must be created with `adobeio_api` scope, which is required for all the API calls.|
|[httpOptions]	|[EventsCoreAPIOptions](#eventscoreapioptions)	|Options to configure API calls| 

## `EventsCoreAPIOptions`

One can configure the following HTTP Options for the SDK. These options will be applied while making the respective API.

|Name|	Type|	Description|
|---|---|---|
|[timeout]	|number	|*Optional.* HTTP request timeout in ms. Timeout resets on redirect. 0 to disable (OS limit applies).|
|[retries]	|number	|*Optional.* he maximum number of retires that should be attempted for all APIs in case of a network error or 5xx / 429 error. By default, retries are disabled. In other words, retries = 0. Retries can be enabled by setting this option while initializing the SDK.|

## Installation

```
$ npm install @adobe/aio-lib-events
```

## Initialize the SDK

```javascript
const sdk = require('@adobe/aio-lib-events')
 
async function sdkTest() {
  //initialize sdk
  const client = await sdk.init('<organization id>', '<x-api-key>', '<valid JWT token>', '<http options>')
}
```

## Call methods using the SDK

Once the SDK has been initialized, you can then call methods using various CRUD operations. When an operation is executed, a promise is returned. The promise represents the eventual completion of the command. In the example below, a `catch` method is used to log errors if a command fails.

```javascript
const sdk = require('@adobe/aio-lib-events')

async function sdkTest() {
  // initialize sdk
  const client = await sdk.init('<organization id>', 'x-api-key', '<valid auth token>', '<http options>')

  // call methods
  try {
    // get profiles by custom filters
    const result = await client.getSomething({})
    console.log(result)

  } catch (e) {
    console.error(e)
  }
}
```

## Use the poller for journaling

```javascript
const sdk = require('@adobe/aio-lib-events')

async function sdkTest() {
  // initialize sdk
  const client = await sdk.init('<organization id>', 'x-api-key', '<valid auth token>', '<http options>')
  // get the journaling observable
  const journaling = client.getEventsObservableFromJournal('<journal url>', '<journaling options>')
  // call methods
  const subscription = journaling.subscribe({
    next: (v) => console.log(v), // Action to be taken on event
    error: (e) => console.log(e), // Action to be taken on error
    complete: () => console.log('Complete') // Action to be taken on complete
  })
  
  // To stop receiving events from this subscription based on a timeout
  setTimeout(() => this.subscription.unsubscribe(), <timeout in ms>)
}
```

## Next steps

To begin configuring and working with Events elements, please select from the following guides:

* [Providers](providers.md), 
* [Event Metadata](event-metadata.md), 
* [Publish Events](publish-events.md), 
* [Webhooks](webhooks.md), 
* [journaling](journaling.md), 
* [Signature verification](signature-verification.md)