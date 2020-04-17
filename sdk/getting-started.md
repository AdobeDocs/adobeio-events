# Getting Started with the Events SDK

Intro

## Requirements

JWT Token: I/O Management Service needs to be enabled for the integration. This will help create the JWT token with adobeio_api scope which is required for all the API calls. 

IMS Org Id: The Organization Id for which the provider, event metadata, etc are to be created which can be obtained using the Console or Transporter API. 

API Key: The API Key ( client id ) for the integration ( project workspace ) 

## HTTP Options

One can configure the following HTTP Options for the SDK. These options will be applied while making the respective API calls to CSM. 

1. timeout: One can configure the request/response timeout in ms, it resets on redirect. 0 to disable (OS limit applies).
1. retries: The maximum number of retires that should be attempted for all APIs in case of a network error or 5xx / 429 error. By default, retries are disabled, i.e. the number of retries = 0. It can be enabled by setting this option while initializing the SDK.

## Installation

```
$ npm install @adobe/aio-lib-events
```

## Initialize the SDK

```javascript
const sdk = require('@adobe/aio-lib-events')
 
async function sdkTest() {
  //initialize sdk
  const client = await sdk.init('<organization id>', '<x-api-key>', '<valid JWT token>', '<httpOptions>')
}
```

## Call methods using the SDK

Once the SDK has been initialized, it can be used to to perform CRUD operations as outlined in the documentation.

```javascript
const sdk = require('@adobe/aio-lib-events')

async function sdkTest() {
  // initialize sdk
  const client = await sdk.init('<organization id>', 'x-api-key', '<valid auth token>', '<options>')

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