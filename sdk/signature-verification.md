# Signature verification for events

Signature Verification for Events
verifySignatureForEvent(event, clientSecret, signatureHeaderValue) ⇒ boolean

Your webhook URL must necessarily be accessible from the open internet. This means third-party actors can send forged requests to it, tricking your application into handling fake events.

To prevent this from happening, Adobe I/O Events will add a x-adobe-signature header to each POST request it does to your webhook URL, which allows you to verify that the request was really made by Adobe I/O Events.

This signature or “message authentication code” is computed using a cryptographic hash function and a secret key applied to the body of the HTTP request. In particular, a SHA256 HMAC is computed of the JSON payload, using your client secret as a secret key, and then turned into a Base64 digest. 

This SDK method allows you to pass the x-adobe-signature header received and the json payload delivered to the webhook to check its authenticity. The method returns true if the calculated signature matches that of the header, else returns false. This can be incorporated as part of any webhook implementation in order to verify the signature for each event received at the webhook endpoint. 


Headers received as part POST to webhook URL

```
Request URL: <webhook_url>
Request method: POST
Content-Type: application/json; charset=utf-8
accept-encoding: deflate,compress,identity
user-agent: Adobe/1.0
x-adobe-delivery-id: <id>
x-adobe-event-code: <event_code>
x-adobe-event-id: <event_id>
x-adobe-provider: <provider_name>
x-adobe-signature: <signature>
```