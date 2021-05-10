# Custom I/O Events Overview

Event-driven architectures are a foundational component of the experience business. They enable our customers to react to changes in state, and behaviors and updates to trigger workflows and decisions in near-real time. 
`Adobe I/O Events` currently supports Adobe internal providers, product teams, by providing 3rd party subscription management to emitted events. 

**Custom I/O Events** extends this capability by allowing 3rd party developers to generate external events to integrate with Adobe products in a bi-directional flow.  

Custom I/O Events will enable users to emit custom events from their applications or microservices to I/O Events and also consume events existing in the I/O Events.  

**Benefits of Custom Events** 

- **Simple Event Delivery**: 
`Adobe I/O Events` co-ordinates the delivery of an event from an Event Producer to an Event Consumer. 
Producers are decoupled from consumers — a producer doesn't know which consumers are listening. Consumers are also decoupled from each other, and every consumer sees all of the events.

- **Open Events Specification**:
`Adobe I/O Events` leverages the Open [CloudEvents Specification](https://cloudevents.io/), 
allowing you to leverage its [many sdks](https://github.com/cloudevents/spec#sdks)

- **Near Real-time responses**: 
Events are delivered in near real time, so consumers can respond immediately to events as they occur. This helps in orchestrating streamline workflows, improve marketing performance, and create custom experiences that leverage the data more effectively. 

- **Build Cloud Native Adobe Apps**: 
Build custom apps that interact with core Adobe services, and automate processes 
with event-based integrations using [Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) 
which comes with the ability to publish/consume Custom I/O Events. 

## Get Started

Register your `Custom Adobe I/O Events providers` and associated `Event Metadata`, 
create event based registrations, 
and then start publishing events from your apps, using either
* [Adobe I/O Events API](../api/api.md)  
* [Adobe I/O Events CLI](../cli/cli.md) 
* [Adobe I/O Events SDK](../sdk/sdk.md) 