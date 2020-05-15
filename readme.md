# Getting Started with Adobe I/O Events

**Adobe I/O Events** offer you a powerful way to integrate your application with Adobe services and solutions. Starting with Creative Cloud Assets, Adobe Experience Manager, and Adobe Analytics Triggers, we are adding events across our entire range of technologies.

Adobe I/O Events enables building reactive, event-driven applications, based on events originating from various Adobe services, such as Creative Cloud, Adobe Experience Manager, and Analytics Triggers.

Events are triggered by an **Event Provider**, like Adobe Creative Cloud Assets, whenever a certain real-world action occurs, such as creating a new asset.

To start listening for events, your application registers a **webhook**, specifying which **Event Types** from which **Event Providers** it wants to receive. Whenever a matching event gets triggered, your application is notified through an HTTP POST request.

Through the Webhooks API you can create, retrieve, update, or delete webhook registrations, specifying the types of events to be notified of, as well as the URLs where notifications should be sent to.

## Prerequisites

**Creative Cloud Events**
To work with Creative Cloud Events, you would need an active Adobe ID.

**Experience Cloud Events**
To work with events for Adobe services in Experience Cloud, you would need to have corresponding entitlements for this Adobe service in your organization, and administrative permission for your org to create integrations.

**Experience Platform Events**
To work with events for Adobe Experience Platform and related Platform services, as well as to create integrations, you will require the appropriate permissions and access to Experience Platform from your organization.

## Key Concepts
- [Webhook Introduction](intro/webhook_docs_intro.md)
    - [Sample Webhook in Node.js](https://github.com/adobeio/io-event-sample-webhook)
- [Adobe I/O Management API for Events](intro/events-api.md)
- [Journaling API](intro/journaling_api.md)

## Get Started with Available Events
- Creative Cloud Events
    - [Creative Cloud Asset Events](using/cc-asset-event-setup.md)
- Experience Cloud Events
    - [Adobe Experience Manager Events](using/aem-event-setup.md)
    - [Analytics Triggers Events (Private beta)](using/analytics-triggers-event-setup.md)
- [Experience Platform Events](using/experience-platform-event-setup.md)
    - [Privacy Events](using/privacy-event-setup.md)

## Support
- [Debugging Common Issues](support/debug.md)
- [Tracing](support/tracing.md)
- [Forums](https://forums.adobe.com/community/adobe-io/adobe-io-events)
- [FAQ](support/faq.md)
- [Forums](https://forums.adobe.com/community/adobe-io/adobe-io-events)
- [Release Notes](support/release_notes.md)
