# Subscribe to events using Journalling

EventsJournalOptions: 
The following options can be configured while calling the journalling API:

* **`latest`:** By default, the latest is set to false and all events are read from the first valid position in the journal. If set to true, Messages will be read from the latest position. 
* **`since`:** Provide the position in the journal from where events must be fetched. If not specified and latests=false, messages are fetched from the first valid position in the journal.
* **`limit`:** Maximum number of messages that must be returned in the response. The number of messages can be less than the specified limit value but will never exceed this value.



`getEventsFromJournal(journalUrl, [eventsJournalOptions]) ⇒ Promise`

The getEventsFromJournal SDK expects a journal URL as input and eventsJournalOptions if required. If no options ( query param values ) are specified, messages are fetched from the first valid position in the journal, and the link.next header in the response provides the position for the next event in the journal. Following the link.next link from each response will help fetch all events in order from the journal. 

You can also rewind to start reading from a different position in the journal by providing the "since" query param in the options or as part of the journal URL. 

The response from the SDK contains the following as part of the json result:

events: Array of events retrieved from the journal

links: Links to the next/latest/current position from where the event is to be fetched

retryAfter: If the call to journal returned 204, it means that there are no more events to be read from the journal. The retryAfter value extracted from the retry-after header in the response specifies the time in seconds after which one can try again to look for more events. It is recommended to honor this retryAfter value. 

Initializing the SDK

```
{ events:
   [ {
       position:'<cursor_position_of_this_event>',
       event:
        {
            header: <headers_map>
        },
          body:
           <json_payload>
    } ],
  _page:
   {
      last: '<cursor_position_of_this_event>',
      count: 1
   },
  link:
   { events:
      'https://events-stage-va6.adobe.io/events/organizations/<consumerOrgId>/integrations/<integrationId>/<registrationId>',
     next:
      'https://events-stage-va6.adobe.io/events-fast/organizations/<consumerOrgId>/integrations/<integrationId>/<registrationId>?since=<cursor_to_position_of_next_event>',
     count:
      'https://events-stage-va6.adobe.io/count/organizations/<consumerOrgId>/integrations/<integrationId>/<registrationId>?since=<cursor_position_of_this_event>',
     latest:
      'https://events-stage-va6.adobe.io/events/organizations/<consumerOrgId>/integrations/<integrationId>/<registrationId>?latest=true',
     page:
      'https://events-stage-va6.adobe.io/events/organizations/<consumerOrgId>/integrations/<integrationId>/<registrationId>?since={position}&limit={count}',
     seek:
      'https://events-stage-va6.adobe.io/events/organizations/<consumerOrgId>/integrations/<integrationId>/<registrationId>?seek={duration}&limit={count}' }
}
```

`getEventsObservableFromJournal(journalUrl, [eventsJournalOptions], [eventsJournalPollingOptions]) ⇒ Observable`

Polling journal for events and taking action on each event such as mapping, transformations, filtering are some common functionalities that would be useful for most users of the journalling API. 

This method encapsulates all the complexities of fetching events by following the link.next and retry-After headers while the users can simply focus on implementing the business logic of taking action on receiving events. 

This method returns an RxJS Observable https://rxjs.dev/guide/observable which users can fetch and subscribe to in order to listen to events. The following code explains how to get started with journaling: 

Getting started with Journalling

```javascript
const sdk = require('@adobe/aio-lib-events')
 
async function sdkTest() {
  // initialize sdk
  const client = await sdk.init('<organization id>', 'x-api-key', '<valid auth token>', '<http options>')
  // get the journalling observable
  const journalling = client.getEventsObservableFromJournal('<journal url>', '<journalling options>')
```


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


The simplest way to subscribe and take action on the next event is as follows: 

Simple subscription to observable
```
journalling.subscribe(
    x => console.log('onNext: ' + x), // any action onNext event
    e => console.log('onError: ' + e.message), // any action onError
    () => console.log('onCompleted')) //action onComplete
```

RxJS provides a lot of flexibility in handling events. You can compose functions declaratively in sequence to work on the emitted data. For more complicated use cases, one can make use of RxJS operators https://rxjs.dev/guide/operators to filter certain events, transform the events, etc. Such an implementation would look something like :

Simple subscription to observable

```javascript
const { filter, map, takeWhile } = require('rxjs/operators')

...
const subscription = journalling.pipe(
    filter(e => e.event.header.msgType === '<message_type>'),// any filtering predicate that returns a boolean
    takeWhile(e => e.event.header.msgId !== <message_id>'),// if they wish to read messages from start till a particular position or any other condition
    map(e => e.event.body) // transform the event or any other mapping
  ).subscribe({
    next: (v) => {
      console.log(JSON.stringify(v)) // any actions they want to take with event can be implemented here
    },
    error: (e) => console.log(e),
    complete: () => console.log('Observer A is complete')
  })
  
setTimeout(() => {
  subscription.unsubscribe()
}, 10000)
```

One can also have multiple subscriptions to a single observable, each transforming and filtering events on different criteria or applying any other operators as follows:

Simple subscription to observable

```javascript
const { filter, map, takeWhile } = require('rxjs/operators')
 
 
...
const subscription1 = journalling.pipe(
    filter(e => e.event.header.msgType === '<message_type_1>'),
    map(e => <transformed_event_1>)
  ).subscribe({
    next: (v) => {
      // action to be executed on receving events of message_type_2
    },
    error: (e) => console.log(e),
    complete: () => console.log('Observer A is complete')
  })
  
setTimeout(() => {
  subscription1.unsubscribe()
}, 10000)
 
 
const subscription2 = journalling.pipe(
    filter(e => e.event.header.msgType === '<message_type_2>',
    map(e => <transformed_event_2>)
  ).subscribe({
    next: (v) => {
      // action to be executed on receving events of message_type_2
    },
    error: (e) => console.log(e),
    complete: () => console.log('Observer B is complete')
  })
  
setTimeout(() => {
  subscription2.unsubscribe()
}, 20000)
 
 
...
```