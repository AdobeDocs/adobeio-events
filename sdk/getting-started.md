# Getting Started with the I/O Events SDK

Introduction.

## Requirements

* **JWT Token:** I/O Management Service needs to be enabled for the integration. This will help create the JWT token with adobeio_api scope which is required for all the API calls. 
* **IMS Org Id:** The Organization Id for which the provider, event metadata, etc are to be created which can be obtained using the Console or Transporter API. 
* **API Key:** The API Key ( client id ) for the integration ( project workspace ) 

## HTTP Options

One can configure the following HTTP Options for the SDK. These options will be applied while making the respective API calls to CSM. 

1. **`timeout:`** One can configure the request/response timeout in ms, it resets on redirect. 0 to disable (OS limit applies).
1. **`retries:`** The maximum number of retires that should be attempted for all APIs in case of a network error or 5xx / 429 error. By default, retries are disabled, i.e. the number of retries = 0. It can be enabled by setting this option while initializing the SDK.

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

## Use the poller for journalling

```javascript
const sdk = require('@adobe/aio-lib-events')

async function sdkTest() {
  // initialize sdk
  const client = await sdk.init('<organization id>', 'x-api-key', '<valid auth token>', '<http options>')
  // get the journalling observable
  const journalling = client.getEventsObservableFromJournal('<journal url>', '<journalling options>')
  // call methods
  const subscription = journalling.subscribe({
    next: (v) => console.log(v), // Action to be taken on event
    error: (e) => console.log(e), // Action to be taken on error
    complete: () => console.log('Complete') // Action to be taken on complete
  })
  
  // To stop receiving events from this subscription based on a timeout
  setTimeout(() => this.subscription.unsubscribe(), <timeout in ms>)
}
```

## Next steps

Work with SDK - link to documentation for:
* [providers](providers.md), 
* [event-metadata](event-metadata.md), 
* [publish events](publish-events.md), 
* [webhooks](webhooks.md), 
* [journalling](journalling.md), 
* [signature verification](signature-verification.md)