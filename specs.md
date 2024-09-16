---
layout: default
show_quote: true
---

## [OpenAPI Specs](https://swagger.io/specification/)

> The OpenAPI Specification (OAS) defines a standard, language-agnostic interface to HTTP APIs which allows both humans and computers to discover and understand the capabilities of the service without access to source code, documentation, or through network traffic inspection. When properly defined, a consumer can understand and interact with the remote service with a minimal amount of implementation logic.

## [OpenId Connect](https://openid.net/connect/)

> OpenID Connect is an interoperable authentication protocol based on the OAuth 2.0 framework of specifications (IETF RFC 6749 and 6750). It simplifies the way to verify the identity of users based on the authentication performed by an Authorization Server and to obtain user profile information in an interoperable and REST-like manner.

## [CloudEvents Specs](https://github.com/cloudevents/spec/blob/v1.0.2/cloudevents/spec.md)
> CloudEvents is a specification for describing event data in common formats to provide interoperability across services, platforms and systems.

Example event:

``` json
{
    "specversion" : "1.0",
    "type" : "com.github.pull_request.opened",
    "source" : "https://github.com/cloudevents/spec/pull",
    "subject" : "123",
    "id" : "A234-1234-1234",
    "time" : "2018-04-05T17:31:00Z",
    "comexampleextension1" : "value",
    "comexampleothervalue" : 5,
    "datacontenttype" : "text/xml",
    "data" : "<much wow=\"xml\"/>"
}
```

## [HL7 FHIR](https://www.hl7.org/fhir/overview.html)

> a standard for exchanging healthcare information electronically.