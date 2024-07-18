# Flipboard.com: ActivityPub Implementation

This repository contains the documentation for the ActivityPub implementation on Flipboard.com.

If you are having issues with the ActivityPub implementation on Flipboard.com, please open an issue in this repository.

## User Agent

In the implementation of Flipboard's ActivityPub, the `User-Agent` HTTP header is utilized to identify the source of the request. The `User-Agent` string is formatted as:

```
Flipboard ActivityPub/1.0.0 (+https://flipboard.com)
```

All requests from the Flipboard ActivityPub implementation will include this `User-Agent` string.

## Server-Level Actor Discovery Using WebFinger

The Flipboard ActivityPub Implements [FEP-d556: Server-Level Actor Discovery Using WebFinger](https://codeberg.org/fediverse/fep/src/branch/main/fep/d556/fep-d556.md)

The Service-Level actor is located at `https://flipboard.com/actor`.

Webfinger requests for the resources:
- acct:flipboard.com@flipboard.com
- https://flipboard.com
- https://flipboard.com/actor

Will always return the same ServiceLevel Actor

```
{
  "subject": "acct:flipboard.com@flipboard.com",
  "aliases": [
    "https://flipboard.com/actor"
  ],
  "links": [
    {
      "rel": "http://webfinger.net/rel/profile-page",
      "type": "text/html",
      "href": "https://about.flipboard.com/"
    },
    {
      "rel": "self",
      "type": "application/activity+json",
      "href": "https://flipboard.com/actor"
    },
    {
      "rel": "http://ostatus.org/schema/1.0/subscribe",
      "template": "https://flipboard.com/authorize_interaction?uri={uri}"
    }
  ]
}
```

The current webfinger implementation only supports `JSON` responses.

## Accounts on flipboard returned through webfinger

Flipboard accounts that are federated will return an `"application/activity+json` link in the webfinger response.

If an account is not federated, the webfinger response will not include an `"application/activity+json` link.

All active accounts will return a `profile-page` link in the webfinger response.

## Http Signatures

We follow the latest draft of the http signatures specification.

For `GET` requests the server will provide a signature in the `Signature` header that verifies the `(request-target)`, `date`, and `host` headers:
```
Signature: keyId="https://flipboard.com/actor#main-key",algorithm="rsa-sha256",headers="(request-target) date host",signature="..."
```

For `POST` requests the server will provide a signature in the `Signature` header that verifies the `(request-target)`, `date`, `host`, and `digest` headers:

```
Signature: keyId="https://flipboard.com/actor#main-key",algorithm="rsa-sha256",headers="(request-target) date host digest",signature="..."
```


### Docs and useful information
- [FAQ](https://flipboard.helpshift.com/hc/en/1-flipboard/section/119-flipboard-the-fediverse/?l=id)
- [About](https://about.flipboard.com/)
- [Privacy Policy](https://about.flipboard.com/privacy-policy/)