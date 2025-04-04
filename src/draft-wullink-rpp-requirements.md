%%%
title = "RESTful Provisioning Protocol (RPP) - Requirements"
abbrev = "RPP - Requirements"
area = "Internet"
workgroup = "Network Working Group"
submissiontype = "IETF"
keyword = [""]
TocDepth = 4

[seriesInfo]
name = "Internet-Draft"
value = "draft-wullink-rpp-requirements-00"
stream = "IETF"
status = "standard"

[[author]]
initials="M."
surname="Wullink"
fullname="Maarten Wullink"
abbrev = ""
organization = "SIDN Labs"
  [author.address]
  email = "maarten.wullink@sidn.nl"
  uri = "https://sidn.nl/"

[[author]]
initials="P."
surname="Kowalik"
fullname="Pawel Kowalik"
abbrev = ""
organization = "DENIC"
  [author.address]
  email = "pawel.kowalik@denic.de"
  uri = "https://denic.de/"

%%%

.# Abstract

<!-- 
 This is a living document and will be published as an RFC AFTER
 the design of RPP is completed. Allowing for a more agile development process and easier updating of the requirements when needed.
-->

This document describes the requirement for the development of the RESTful Provisioning Protocol (RPP).

{mainmatter}

# Introduction

This document describes the set of requirements for the RESTful Provisioning Protocol (RPP), an Application Programming Interface (API) for provisioning objects in a shared database. RPP is based on the HTTP [@!RFC9110] protocol and the architectural principles of [@!REST].

# Terminology

In this document the following terminology is used.

REST - Representational State Transfer ([@!REST]). An architectural style.

RESTful - A RESTful web service is a web service or API implemented using HTTP and the principles of [@!REST].

EPP RFCs - This is a reference to the EPP version 1.0 specifications [@!RFC5730], [@!RFC5731], [@!RFC5732] and [@!RFC5733].

RESTful Provisioning Protocol or RPP - The protocol described in this document.

URL - A Uniform Resource Locator as defined in [@!RFC3986].

Resource - An object having a type, data, and possible relationship to other resources, identified by a URL.

RPP client - An HTTP user agent performing an RPP request 

RPP server - An HTTP server responsible for processing requests and returning results in any supported media type.

# Conventions Used in This Document

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT","SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [@!RFC2119].

# General

**R1.1.** A wel defined architecture MUST be defined for RPP, including a description of the responsibilities of the definded protocol layers. 

**R1.2.** The API MUST provide a clear, clean, easy to use and self-explanatory interface that can easily be integrated into existing software systems and includes language bindings for the most popular programming languages.
//PK: too vague

**R1.3.** RPP MUST leverage widely deployed web standards, tools, and infrastructure components such as HTTP, JSON, OpenAPI specification, API gateways, load balancing, caching and delegate functional responsibility to the HTTP layer when possible. For example, authentication is part of the HTTP layer and not part of the RPP application layer.
//PK:  too vague

# HTTP

**R2.1.** The Hypertext Transfer Protocol (HTTP) [@!RFC9110] MUST be used as the transport mechanism for RPP messages.  

**R2.2.** RPP SHOULD use the common best practices for designing a HTTP based application described in [@!BCP56] or there MUST be a clear justification of not doing so. 

**R2.3.** Consistent and meaningful URL structures MUST be used for for identifying, accessing resources and enable request routing.

**R2.4.** RPP MUST use standard HTTP status codes and MAY define new RPP status and map these to HTTP status code, RPP MUST NOT redefine or overload HTTP status code semantics.

# REST

**R3.1.** The RPP architecture MUST use the principles of the [@!REST] architectural style. A RPP server MUST conform to at least level 2 of the [@!RICHARDSON] Maturity Model (RMM). 

**R3.2** The RPP architecture MUST follow Resource-Oriented Architecture [@!ROI].

//PK: this part is for me questionable. Especially with all the requirements R6.X. Full HAEOAS is likely not what we need (however we'd need to formulate a design requirement, which would support this call), but some level of hypermedia does not have to be necessarily wrong, like: links to other resources, signalling of available/permittable operations etc.
**R3.3.** The RPP specification MUST specify all resource URLs used and therefore the URLs are already known and do not need to be dynamically discoverable, designing for RMM level 3 is not recommended.

**R3.4.** When the semantics of a resource URL and HTTP method do not require a request payload, the use of a request message MUST be optional.
//PK: it is somehow strange formulated as this optionality would mean that the client might send the payload anyway - then what would have precedence? It was ok when we were approaching direct mapping from EPP, not it seems not to be necessary at all.
Proposal:
RPP SHALL prefer simplicity. Wherever the whole operation can be defined by a resource URL and HTTP method and where HTTP method does not require a message body, RPP MUST NOT use a message body. This approach reduces complexity, improves performance, and aligns with the principles of RESTful design by leveraging the inherent semantics of HTTP methods.

**R3.5.** RPP specifications SHOULD incorporate a machine-readable and well-established API specification, such as OpenAPI, RAML, or JSON Schema. This will facilitate documentation, testing, code generation, and user-friendly extension descriptions. The choice of technology SHOULD remain open for further advances in the field and implementer's preferences.
//PK: I would keep it open in the requirements which technology it should be

# Data Model

**R.4.1** The base data model structures MUST be data format agnostic. It MUST be possible to map the data model to multiple data formats (JSON, XML, YAML etc.)

**R.4.2** Commonly used EPP extensions SHOULD be added to the RPP core data model (example: DNSSEC)

// PK: Extension?
**R.4.3** RPP MUST allow an extension mechanism that would allow clients to signal data omission or redaction, indicating data collected but not transmitted to the registry or redacted.

**R.4.4** RPP MUST have mechanisms to define profiles to indicate required parts of the data model, mapping definitions, or functional subsets for compatibility.

**R.4.5** The protocol MUST allow loose coupling between the server and the client, allowing to add non-breaking version changes on both sides. Both servers and the clients MUST in default ignore unknown properties of representations however there MUST be a mechanism for a client to signal that a strict handling is wished where unknown fields are treated as error. 

# Data Representation

**R.4.1** RPP MUST use JSON as its default data format.

**R.4.2** RPP MUST be able to be extended to support other data formats (e.g. XML, YAML).

**R.4.3** Validation of request and response message MUST be supported, in order to determine if the content is valid and no required attributes are missing.

**R.4.4** RPP must define a default media type however the protocol shall be extendible to support other media types.

**R.4.5** A client MUST be able to signal to the server what media type the server should expect for the request content and to use for the response content.

**R.4.6** Allow for the use of server profiles, indicating required parts for the data model and/or mapping definitions.

**R.4.7** RPP SHOULD consider mechanisms for supporting data formats outside of core RPP domain. Especially formats, which lose their properties if transformed, like Verifiable Credentials for contacts which are digitally signed.

**R.4.8** RPP MUST support partial update of data objects.

**R.4.9** RPP MUST support full update of data objects.

**R.4.10** Contact information MUST be provided using the JSContact [@!RFC9553] format.
//PK: I think we should not have it as MUST requirement. R.4.1 is covering to support JSContact as additional format, but can we decide now whether this would be a default?

# Operations and responses

**R.5.1** A client MAY want to request different depth of data representations, depending on the use case:

- Minimal representation (ID, or ID+name)
- Full representation (all data of the object)
- Full representation + dereferenced referrals (for example domain with contact and host details)

**R.5.2** Different representations may be requested in different contexts:

- GET request to the resource itself
- GET request to get a collection of objects
- Responses to PUT/POST/PATCH requests

**R.5.3** Data representation in a responses MUST only contain the provisioning object itself, the transactional information MUST be represented in separate HTTP headers.

# Discoverability

**R.6.1** RPP MAY include a bootstrap mechanism te help clients locate RPP available services, possible solution include:

- IANA bootstrap Service Registry
- DNS TXT records

**R.6.2** A discovery document MUST be made available in the well-known directory.
//PK: This is somehow contadictory to R.5.1. If it's well known then you don't need other signalling 

**R.6.3** Server provided fucntionality, such as the set of supported profiles or extensions, MUST discoverable using the discovery document.

**R.6.4** RPP MUST support versioning of the protocol itself, data object types, representations, operations and profiles.

**R.6.5** Versioning schema MUST carry information about breaking vs. non-breaking changes and allow clients to decide whether it is able to interact with the server.

**R.6.5** Versions MUST be discoverable by the client.

//PK: extension? This might get quite complex.
**R.6.6** Notices related to scheduled server maintenance timeslots MAY be included in the discovery document

**R.6.7** A RPP service MAY choose to only support a subset of EPP functionality, this MUST be discoverable by the client.

# EPP compatibility

**R.7.1** RPP MUST provide functional equivalents for core EPP functionalities related to domain names, hosts, and contacts as defined in [@!RFC5731], [@!RFC5732] and [@!RFC5733] mappings for core objects (domain, contact, host).

**R.7.2** The automatic or mechanical mapping or conversion between EPP and RPP MUST be possible. Compatibility definitions for RPP to EPP mappings MAY be defined in compatibility profiles.

**R.7.3** The most commonly used EPP extensions MAY be include in the core RPP specification.
//PK: duplicate of R.4.2 ? Is it needed as EPP compatibility? I would rather put: RPP MUST include extension mechanisms to be able to define equivalents of most commonly used EPP extensions, which are not a part of core protocol (see: R.4.2)

**R.7.4** Only the string based EPP token defined in [@!RFC5730] MUST be supported, any EPP token extensions MAY supported.

**R.7.5** RPP MUST support client_id/password authentication to match EPP client authentication 

# Security

**R.8.1** RPP MUST support state-of-the-art authentication and authorization schemes allowing for easy integration in modern HTTP infrastructure.

**R.8.2** RPP MUST support modern authentication and authorization standards (OAuth, OpenId Connect)

**R.8.3** Support for an easier and faster object transfer process MAY be included, where approval from the losing registar can be obtained interactively by the registrant during the transfer process

**R.8.4** The authorisation model MUST support granular authorizations, using framework such as OAuth, beyond current auth-code based authorisation for transfers and read access to the objects only, the following usecases MAY be supported:

- Domain transfers without first getting the "normal" transfertoken
- DNS providers use the API to update the NS records
- OpenID Connect to interactively allow for DNS provider to update NS records, directly at theregistry of indirectly through a supporting registar.
- Renewals

**R.8.6** RPP MUST employ strong authentication and utilize encrypted transport (HTTPS) to protect sensitive data. 

**R.8.7** Security mechanisms SHOULD be flexible to allow operators to choose appropriate methods and support federated authentication scenarios. 

**R.8.8** RPP MAY include a mechanism for cryptographic verification of request and response messages as an additional security layer.

# Extensibility

**R.9.1** The protocol MUST be extensible to accommodate new functionalities, data objects, and operations beyond the initial scope.

**R.9.2** RPP MUST allow for flexibility in extending the data model e.g. adding new objects or a new attribute to an existing object MUST be possible, easy and natural.

**R.9.3** RPP SHOULD promote standardisation of commonly used extension attributes.

**R.9.4** Extensions for new operations on existing resources MUST be supported.

**R.9.5** RPP MUST support adding new error codes by extensions

**R.9.6** RPP MUST support adding new HTTP headers by extensions

**R.9.7** A namespace concept for JSON MUST be support, to prevent name collissions between the RPP core and a extension and between two or more extensions.
//PK: I would rather see a similar model as rfc6838 with simplified namespacing. Standard/registered tree (no namespace) and vendor tree maybe with reversed domain name notation.
Proposal:
RPP SHALL have mechanisms to assure conflict avoidance when extending the protocol, including but not limited to data model, representations, operations, parameters, error codes and signalling. There MUST be a mechanism of conflict-free, non-coordinated extending in private/vendor discresion as well as a coordinated process for core, generic or shared elements. 

**R.9.8** When a registry for RPP extensions is required, then IANA MUST be used for this function.

# Scalability

**R.10.1** RPP MUST be stateless and MUST NOT maintain application state on the server required for processing future RPP requests. Every client request needs to provide all the information required for the server to be able to successfully process the request. The client MAY maintain application session state, for example by using a JWT token.

**R.10.2** Server responses that are cacheable MUST not include RPP transaction related identifiers and values.
//PK: what would that mean? I guess any representation MUST NOT include transaction related identifiers and values, not only the cachable ones.
RPP MUST support cacheability of responses, if applicable to the operation semantics

**R.10.3** RPP MUST support load balancing at the level of request messages

**R.10.4** Every request message MUST at most contain a single object for the server to operate on, with the exception of operations that are explicitely defined as a bulk operation. Bulk operations MAY be processed asynchronously.
// PK Would asynchronous and synchronous processing be a separate requirement? 

**R.10.5** RPP MUST support both synchronous and asynchronous processing, with immediate response of the full processing result or just confirmation of reception and a mean to get final processing result later, accordingly.

# Performance

**R.11.1** RPP MUST allow optional or no request/response message when this is not required, improving performance and network bandwidth requirements for both client and server. Fewer messages have to be created, marshalled, and transmitted.

**R.11.2** RPP MAY allow for common bulk operations, resource listing, and filtering capabilities where this does not impact scalability negatively.

**R.11.3** RPP MAY support compound object create request having embedded contact/host vs. request serialization (client waiting for contact/host creation to succeed before sending a domain request). Return complete representation (similar to object info in EPP) after compound request completed or return redirect to newly created object location.
//PK: from the feedback I got so far, I would drop this requirement

# Internationalisation

**R.12.1** RPP MUST support internationalization, including for Contact objects, email addresses, and Internationalized Domain Names (IDNs).

**R.12.2** Human-readable localized response mesages MUST be supported.

# Clients

**R.31.1** RPP MUST support server applications as clients. This will be a primary use-case of registry/registrar integration.

**R.31.2** RPP MUST support interaction from command-line tools or desktop applications capable of sending HTTP requests. Whese can be generic clients such as `curl` or `Postman` but also specialized RPP command line tools or scripts.

**R.31.3** RPP SHOULD support web browsers as clients, such as SPA (single page applications) without any proxy backend between webbrowser and the RPP server.

**R.31.4** RPP SHOULD support mobile applications as clients, also here through direct integration without any proxy backend.

# New features

//PK: would you see it as a part of generic protocol requirements or an extension
The server MAY support generating a representation of a historical overview for an object, e.g. show all events linked to the object (create, update ...). The historical time window is determined by server policy and MUST be included in the discovery service document.

# Other

The items below have been mentioned on the mailinglist and may need to be added as an requirement.

- Data Omission - what requirements will there be around a registrar's ability to signal that it has collected some data but has not transmitted it to the registry?  
//PK: covered in R.4.3 ?
- Registration attribution - will there be requirements for attribution of registration actions (who did what), and will cryptography be used?
//PK: extension ?  
- Registrant verification - will there be requirements to support registrant verification (NIS2)?
//PK: this is a typical extension case (new attributes and ops)  
- Linking - will the protocol support linking to RDAP objects, other RPP objects, etc...  
- Include identification of legal vs. natural contacts.
//PK: do we collect also here requirements for certain object types, or only core protocol?
- Expanded common models (when compared to EPP), maybe there should be much more attributes then it is in EPP (Vatnumber / Company Number/ properties for Identity papers) Trademark information and more There are a lot of epp extensions in the wild that try to handle this.
//PK: same here
- include also DNS provisioning as potential use-case
//PK: this one is beyond current charter
- possibility to seamlessly compose the API with other registry use-cases to have uniform API layer from the client perspective
- investigate possible multi-party authorisation schemas. Use case: DNS operator would get authorisation to update DS or NS record through RPP.
    //PK: covered in R.8.4 ?
  - This may refer to roles not yet considered in EPP processes, e.g. a third-party DNSSEC signing authority. For a potential use case, see [the discussion on the DD mailing list on that topic](https://mailarchive.ietf.org/arch/msg/dd/iTf8pEMq5-sismlxfNbrFpzAIKU/), which discusses potential approaches for DNS, some of which would require modelling relationships between parent/child/signer zone authorities in a provisioning protocol.
- possibility of mobile app or direct browser integration (use case for registries which directly authenticate their domain holders and allow operations on a domain and/or if RPP would be exposed by a registrar)
//PK: I would add a chapter Clients. Covered in R.31.x

# IANA Considerations

TODO


# Internationalization Considerations

TODO

# Security Considerations

TODO

{backmatter}

<reference anchor="REST" target="http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm">
  <front>
    <title>Architectural Styles and the Design of Network-based Software Architectures</title>
    <author initials="R." surname="Fielding" fullname="Roy Fielding">
      <organization/>
    </author>
    <date year="2000"/>
  </front>
</reference>

<reference anchor="RICHARDSON" target="https://martinfowler.com/articles/richardsonMaturityModel.html">
  <front>
    <title>Richardson Maturity Model</title>
    <author initials="M." surname="Fowler" fullname="Martin Fowler">
      <organization/>
    </author>
    <date year="2010"/>
  </front>
</reference>

<reference anchor="ROI" target="https://www.oreilly.com/library/view/restful-web-services/9780596529260/ch04.html">
  <front>
    <title>RESTful Web Services, Chapter 4</title>
    <author initials="L." surname="Richardson" fullname="Leonard Richardson">
      <organization/>
    </author>
    <author initials="S." surname="Ruby" fullname="Sam Ruby">
      <organization/>
    </author>
    <date year="2007"/>
  </front>
</reference>

<reference anchor="YAML" target="https://yaml.org/spec/1.2.2/">
  <front>
    <title>YAML: YAML Ain't Markup Language</title>
    <author>
      <organization>YAML Language Development Team</organization>
    </author>
    <date year="2000"/>
  </front>
</reference>

<reference anchor="XML" target="https://www.w3.org/TR/xml">
  <front>
    <title>Extensible Markup Language (XML) 1.0 (Fifth Edition)</title>
    <author>
      <organization>W3C</organization>
    </author>
    <date year="2013"/>
  </front>
</reference>
