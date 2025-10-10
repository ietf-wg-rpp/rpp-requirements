%%%
title = "RESTful Provisioning Protocol (RPP) - Requirements"
abbrev = "RPP - Requirements"
area = "Internet"
workgroup = "Network Working Group"
submissiontype = "IETF"
keyword = [""]
TocDepth = 4
date = 2025-09-15

[seriesInfo]
name = "Internet-Draft"
value = "draft-ietf-rpp-requirements-01"
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

RPP client - An HTTP user agent performing an RPP request.

RPP server - An HTTP server responsible for processing requests and returning results in any supported media type.

Sponsoring Client - The RPP client that currently has sponsorship of the object.

# Conventions Used in This Document

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT","SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [@!RFC2119].

# General

**R1.1.** A well defined architecture MUST be defined for RPP, including a description of the responsibilities of the defined protocol layers.

**R1.2.** *Removed*

**R1.3.** Wherever applicable RPP SHOULD leverage existing best practices and well adopted standards for building and documenting RESTful APIs. There MUST be a clear justification when deviating from this.

**R1.4.** RPP MUST include support for application level status codes, and MAY reuse the EPP status codes defined in [@!RFC5730].

**R1.5.**
RPP MUST include support for providing detailed information about application status codes, for example as described in [@!RFC7807]

**R1.6**
RPP MUST support additional information about a successful operation (information or warning) to convey additional information to the client for example about deprecation or partial success.

# HTTP

**R2.1.** The Hypertext Transfer Protocol (HTTP) [@!RFC9110] MUST be used as the transport mechanism for RPP.

**R2.2.** RPP SHOULD use the best common practices for designing HTTP based applications, described in [@!BCP56]. There MUST be a clear justification when deviating from this.

**R2.3.** Consistent, predictable and meaningful URL structures MUST be used for identifying, accessing object resources and enable request routing.

**R2.4.** RPP MUST use the existing HTTP status codes and MUST define application level status codes and map these to HTTP status codes. RPP MUST NOT redefine existing HTTP status code semantics and when overloading (generic) HTTP status codes with multiple RPP status codes, the provided RPP status code MUST be used by the client to determine the exact nature of the problem.

# REST

**R3.1.** The RPP architecture MUST use the principles of the [@!REST] architectural style. A RPP server MUST conform to at least level 2 of the [@!RICHARDSON] Maturity Model (RMM).

**R3.2** The RPP architecture MUST follow Resource-Oriented Architecture [@!ROI].

**R3.3.** The RPP specification MUST strive to minimise round trips between client and server. Approaches, where client would need to make multiple requests each time to discover resource URL or server capabilities in order to perform operation SHOULD be used sparingly and be always well justified.

**R3.4.** *Merged with R12.1*

**R3.5.** RPP specifications SHOULD incorporate a machine-readable and well-established API specification, such as [!@OpenAPI] or [@!RAML]. This will facilitate documentation, testing, code generation, and user-friendly extension descriptions. RPP MUST NOT require what API specification technology is to be used. The RPP core documents and extension documents may also choose different API specification solutions, this choice is left to the document authors.

# Data Model

**R4.1** The base data model structures MUST be data format agnostic. It MUST be possible to map the base data model to multiple data formats such as JSON, XML or YAML.

**R4.2** Commonly used EPP extensions SHOULD be added to the RPP core data model. An example of this is the DNSSEC extension.

**R4.3** RPP MUST allow an extension mechanism that allows clients to signal data omission or redaction, indicating data collected but not transmitted to the registry or redacted.

A> TODO: [Issue #34](https://github.com/ietf-wg-rpp/rpp-requirements/issues/34)

**R4.4** RPP MUST have mechanisms to define profiles to indicate:

- Required parts of the data model
- Mapping definition
- Functional subsets for compatibility.

A> TODO: [Issue #15](https://github.com/ietf-wg-rpp/rpp-requirements/issues/15)

**R4.5** The RPP architecture MUST include loose coupling between the server and the client, allowing for non-coordinated introduction of non-breaking version changes on both sides.

**R4.6** A RPP MUST have either a lenient validation mode, where unknown properties are ignored, or a strict validation mode, where unknown properties are treated as an error. The mode is up to client and server policy with mode signalling.”

**R4.7** *Pending review in PR #107* 

**R4.8** RPP MUST allow a client to reference a shared object (e.g., a host or contact) sponsored by a different client, while ensuring the sponsoring client retains full administrative control over the shared object.

# Data Representation

**R5.1** RPP MUST use JSON as the default data format.

**R5.2** It MUST be possible to extended RPP to include support other data formats (e.g. XML, YAML).

**R5.3** Validation of request and response message MUST be supported for both clients and the servers, in order to determine if the content is valid and no required attributes are missing.

A> TODO: [Issue #36](https://github.com/ietf-wg-rpp/rpp-requirements/issues/36)

**R5.4** RPP MUST define a default media type however the protocol SHALL be extensible to enable support for other media types.

**R5.5** A client MUST be able to signal to the server what media type the server should expect for the request content and to use for the response content.

**R5.6** *Removed*
<!-- see https://github.com/ietf-wg-rpp/rpp-requirements/issues/11
RPP MUST support the use of server profiles to define required components of the data model and/or mapping definitions (see: R4.4). -->

**R5.7** RPP SHOULD consider mechanisms for supporting data formats outside of core RPP domain. Especially formats, which lose their properties if transformed, like Verifiable Credentials for contacts which are digitally signed.

**R5.8** RPP MUST support partial update of data objects.

**R5.9** RPP MUST support full update of data objects.

**R.5.10** A generated RPP response representation that includes an object identifier (for example a contact handle) MUST also include a URL reference to the location of the object representation.

# Operations and responses

**R6.1** RPP MUST include support for a client requesting different depth of data representations, depending on the use case:

- Minimal representation (ID, or ID+name)
- Full representation (all data of the object)
- Full representation + dereferenced referrals (for example domain with contact and host details)

**R6.2** RPP MAY return different representations of the same object in different contexts:

- GET request to the resource itself
- GET request to get a collection of objects
- Responses to PUT/POST/PATCH requests

**R6.3** The data representation in a RPP response MUST only contain data related to the object, transactional information MUST be represented as one or more separate HTTP headers.

A> TODO: [Issue #56](https://github.com/ietf-wg-rpp/rpp-requirements/issues/56)

**R6.4** RPP MUST support a functional equivalent of the EPP Poll command described in EPP RFC [@!RFC5730], allowing for clients discovering and retrieving service messages available on the server. The RPP equivalent MAY contain additional options or features for discovering and retrieving service messages, such as:

- Allowing clients to subscribe to specific types of service messages.
- Allowing clients to receive multiple service messages in a single request.
- Allowing clients to use multiple concurrent readers.
- Support for streaming service messages to clients.

**R6.5** RPP operations that modify repository state MUST be atomic. A single request MUST either succeed completely or fail completely, leaving the repository in its original state.

**R6.6** RPP MUST provide services for the client to assure a re-tried operation changing resource state is executed only once if a request has been terminated or timed out before complete response has been received by the client (idempotency).

**R6.7** The protocol specification MUST define the expected server state for a request that times or terminates out before a response is fully sent out the the client.

**R6.8** For every request the server MUST generate a permanent, server-unique transaction identifier. This identifier MUST be returned to the client in the response.

# Discoverability

**R7.1** RPP MAY include a bootstrap mechanism designed to allow clients to locate the network identifier for the RPP service of a registry operator, e.g. rpp.sidn.nl for the registry operator for the .nl ccTLD.

Solutions may include:

- IANA bootstrap Service Registry
- DNS TXT records

**R7.2** An RPP server MUST publish a service discovery document in the well-known directory, described in [@!RFC5785]. This document contains structured machine readable information that is required or useful for the client to be able to generate valid RPP requests. The information may contain, but is not limited to:

- Available services,
- Used Extensions
- Versions used for services and extensions
- Environment name (production, test etc.)
- Server datetime
- Maintenance notices
- Supported profiles

<!-- A> //TODO: [Issue #8](https://github.com/ietf-wg-rpp/rpp-requirements/issues/8) -->

**R7.3** Server provided functionality, such as the set of supported profiles, languages or extensions, MUST discoverable using the discovery document.

**R7.4** RPP MUST support versioning of:

- The protocol itself
- Data object types
- Representations
- Operations
- Profiles
- Extensions

**R7.5** Versioning schema MUST carry information about breaking vs. non-breaking changes and allow clients to decide whether it is able to interact with the server. The versioning scheme SHOULD be like the scheme used for HTTP where minor version changes do not break compatibility.

**R7.6** Notices related to scheduled server maintenance timeslots MAY be included in the discovery document, this could be a human readable, non machine parsable character string.
<!-- A> //TODO: [Issue #9](https://github.com/ietf-wg-rpp/rpp-requirements/issues/9) -->

**R7.7** RPP MAY only support a subset of EPP functionality, the supported functionality MUST be discoverable by the client

**R7.8** *Removed*
<!-- A> //SEE: [Issue #21](https://github.com/ietf-wg-rpp/rpp-requirements/issues/21) -->

**R7.9** An RPP response that includes unique object identifiers, MAY also include URL references for these objects.

**R7.10** Versions used by the RPP and used extensions MUST be discoverable by the client.

# EPP compatibility

**R8.1** RPP MUST provide functional equivalents for core EPP functionalities related to domain name, host, and contact objects as defined in [@!RFC5731], [@!RFC5732] and [@!RFC5733].

**R8.2** The automatic or mechanical mapping or conversion between EPP and RPP data model MUST be possible.

**R8.3** Compatibility definitions for a RPP to EPP mapping MAY be defined in compatibility profiles (see: R4.4).

A> TODO: [Issue #15](https://github.com/ietf-wg-rpp/rpp-requirements/issues/15)

**R8.4** RPP MUST include an extension framework able to define equivalents of most commonly used EPP extensions, which are not a part of core protocol (see: R4.2)

**R8.5** EPP password based Authorisation Information defined in [@!RFC5731] and [@!RFC5733] MUST be supported in RPP.

**R8.6** RPP SHOULD support client_id/password authentication to match EPP client authentication.

# Security

**R9.1** RPP MUST support state-of-the-art authentication and authorisation schemes allowing for easy integration in modern HTTP infrastructure.

**R9.2** RPP MUST support robust authentication and authorisation mechanisms, such as OAuth 2.0 and OpenID Connect, to ensure that only authorised clients and users can access or modify resources.

**R9.3** Support for a simplified and quicker object transfer process MAY be included, where approval from the losing registrar is to be obtained interactively by the registrant during the transfer process.

**R9.4** RPP MUST include an authorisation model/framework that goes beyond the current EPP password based Authorisation Information (AuthInfo) used for object transfers. The following use cases MAY be supported:

- Object transfers without using an EPP password based Authorisation Information
- Registrants using OpenID Connect can interactively allow DNS operator to update their NS records, directly in the registry database or indirectly using a registrar.

**R9.5** All RPP communications MUST use HTTPS (TLS) to protect data in transit from eavesdropping and man-in-the-middle attacks.

**R9.6** Security mechanisms SHOULD be flexible to allow operators to choose appropriate methods and support federated authentication scenarios.

**R9.7** RPP MAY include a mechanism for cryptographic verification of request and response messages as an additional security layer.

**R9.8** RPP MUST allow for multiple user accounts linked to a single registrar, registrar user management MAY be delegated to an administrator account linked to a registrar, allowing for self service account management by the registrar.

**R9.9** RPP MUST support a granular authorisation matrix, where one or more permissions are coupled to a user account. Allowing for the creation of different types of user accounts, such a readonly users only allowed to fetch data about existing objects, and power users allowed to create and modify objects.

**R9.10** RPP MUST allow users to update their credentials and enforce strong passwords and limited lifetime for passwords and other tokens.

**R9.11** RPP must support the Least Privilege Principle, to allow server operators to ensure that clients have only the permissions necessary.

**R9.12** RPP MUST support secure credentials management, ensuring that credentials are protected against replay and theft, and have limited lifetimes.

**R9.13** Any protocol extensions MUST be subject to the same security review and requirements as the core protocol.

**R9.14** There MUST be mechanisms to revoke or deprecate credentials, tokens, or permissions when no longer needed or if compromised.

**R9.15** RPP must support mechanisms to prevent Denial-of-Service attacks, whether from malicious actors or misbehaving clients. These mechanisms can include rate limiting and throttling of requests with related protocol signalling.

# Extensibility

**R10.1** The protocol MUST be extensible to accommodate new functionalities, data elements, and operations beyond the initial scope.

**R10.2** RPP MUST allow for flexibility in extending the data model e.g. adding new objects or a new attribute to an existing object MUST be possible.

**R10.3** RPP SHOULD promote standardisation of commonly used extension attributes.

**R10.4** Extensions for new operations on existing resources MUST be supported.

A> TODO: [Issue #47](https://github.com/ietf-wg-rpp/rpp-requirements/issues/47)

**R10.5** RPP MUST support extensions that define new status codes not already defined in the core RPP RFCs.
<!-- A> //TODO: [Issue #20](https://github.com/ietf-wg-rpp/rpp-requirements/issues/20) -->

**R10.6** RPP MUST support extensions adding new HTTP headers.

**R10.7** RPP SHALL have mechanisms to assure conflict avoidance when extending the protocol, including but not limited to data model, representations, operations, parameters, error codes and signalling. There MUST be a mechanism of conflict-free, non-coordinated extending in private/vendor discretion as well as a coordinated process for core, generic or shared elements.
<!-- A> //TODO: [Issue #10](https://github.com/ietf-wg-rpp/rpp-requirements/issues/10) -->

**R10.8** When a public registry for RPP extensions is required, then IANA MUST be used for this function.

**R10.9** RPP extensions MUST include support for versioning, the version of the extension supported by the server MUST be included in the discovery document.
<!-- A> //TODO: [Issue #11](https://github.com/ietf-wg-rpp/rpp-requirements/issues/11) -->

**R10.10** *Removed*
<!--
//dropped R10.10: see https://github.com/ietf-wg-rpp/rpp-requirements/issues/20
//**R10.10** RPP status codes are maintained in an IANA registry for RPP status codes, these include all status codes defined in the core RPP RFCs and by any
//extension that is also a registered IANA RPP extension. -->

**R10.11** Extension designers or RPP implementers MAY add new status codes, if a newly created status code is generic enough to be useful for the wider RPP community, then the extension designer SHOULD register the new status code in the RPP IANA registry.

# Scalability

**R11.1** RPP MUST be stateless and MUST NOT maintain application state on the server required for processing future RPP requests. Every client request needs to provide all the information required for the server to be able to successfully process the request. The client MAY maintain application session state, for example by using a JWT token.

**R11.2** RPP MUST support cacheability of responses, if applicable to the operation semantics and MUST not include transaction related identifiers and values.

A> TODO: [Issue #50](https://github.com/ietf-wg-rpp/rpp-requirements/issues/50)

**R11.3** RPP MUST support load balancing at the level of request messages (URL) and load balancing MUST be possible without processing HTTP body.

**R11.4** Every request message MUST at most contain a single object for the server to operate on, with the exception of operations that are explicitly defined as a bulk operation.

**R11.5** RPP MUST support asynchronous processing for operations on multiple objects, otherwise resource intensive or involving manual steps. The client request results in a confirmation of receipt and a means for retrieving the final completed processing result at a later time.

# Performance

**R12.1** In order to minimise message sizes and needed processing RPP SHOULD be designed not to include a HTTP message body in the request or response when this is not necessary, for example when the required data can be transmitted using the URL and/or HTTP headers.

**R12.2** RPP MAY allow for common bulk operations, resource listing, and filtering capabilities. RPP MUST NOT mandate such functionalities where this may impact scalability or performance negatively.

**R12.3** *Removed*
<!--
 RPP MAY support compound object create request having embedded contact/host vs. request serialisation (client waiting for contact/host creation to succeed before sending a domain request). Return complete representation (similar to object info in EPP) after compound request completed or return redirect to newly created object location.
[Issue #12](https://github.com/ietf-wg-rpp/rpp-requirements/issues/12)
 -->

 **R12.4** The protocol MUST be usable in both high volume and low volume operating environments.

# Internationalisation

**R13.1** RPP MUST support internationalisation, for object types and messages defined in the core protocol and extensions

**R13.2** RPP MUST support human-readable localised response messages.

# Clients

**R14.1** RPP MUST support server applications as clients. This will be a primary use-case of registry/registrar integration.

**R14.2** RPP MUST support interaction from command-line tools or desktop applications capable of sending HTTP requests. These can be generic clients such as `curl` or `Postman` but also specialised RPP command line tools or scripts.

**R14.3** RPP SHOULD support web browsers as clients, such as SPA (single page applications) without any proxy backend between web browser and the RPP server.

**R14.4** RPP SHOULD support mobile applications as clients, also here through direct integration without any proxy backend.

# Requirements for object types

## Common

A> NOTE: *O1.x Requirements are defined in PR [#100](https://github.com/ietf-wg-rpp/rpp-requirements/pull/100)

### Object Transfers {#obj_transfers}

For the purposes of requirements related to transfers, the following specific terms are used:

- Gaining Client: The client seeking to gain sponsorship of the object.
- Initiating Client: The client that starts the transfer request.

**O2.1** The RPP MUST support two types of object transfer operations:

- Pull Transfer: Initiated by the Gaining Client.
- Push Transfer: Initiated by the Sponsoring Client, who designates a Gaining Client.

**O2.2** A Gaining Client MUST provide valid authorization information to initiate a Pull Transfer request.

**O2.3** For Pull Transfers, the RPP MUST provide operations for the Sponsoring Client to explicitly approve or reject a pending transfer request. The RPP MUST reject any approval or rejection attempts not initiated by the Sponsoring Client.

**O2.4** For Push Transfers, the RPP MUST provide operations for the Gaining Client to explicitly approve or reject a pending transfer request. The RPP MUST reject any approval or rejection attempts not initiated by the designated Gaining Client.

**O2.5** The RPP MUST provide an operation for the Initiating Client to cancel its own pending transfer request. The RPP MUST reject any cancellation attempts not initiated by the Initiating Client.

**O2.6** The RPP MUST provide an operation to query the status of a pending or recently completed transfer request. This operation MUST be accessible to the Sponsoring Client and the Gaining Client.

**O2.7** The response to a successful object transfer MUST include a representation of the transferred object and a list of any associated objects that were also transferred.

## Domain Object Type

[//]: # (Editor note: use Dx.x for Domains)

### Internationalisation

**D13.1** RPP MUST support Internationalised Domain Names (IDNs) - both UTF-8 as well as Punycode representation of a domain name MUST be supported, however one of them MAY be chosen as primary for object URL

## Host Object Type

[//]: # (Editor note: use Hx.x for Hosts)

## Contact Object Type

**C1.1** The RPP contact object data model MUST include, at a minimum an equivalent of RFC5733 contact data model: a unique identifier, repository object ID, current status, name, organisation, full postal address, voice and fax numbers, email addresses,the sponsoring client identifier, the creating client identifier, creation timestamp, the last updating client identifier, last update timestamp, last transfer timestamp, and authorisation information.

**C1.2** RPP MUST support server‑generated opaque IDs, support for client‑supplied IDs is OPTIONAL.

**C1.3** RPP SHOULD support an explicit indication of entity type (person or organisation) in the contact model.

**C1.4** When RPP is used with thick registries, full contact data MAY be returned, for thin registries only the contact identifier MUST be returned.

**C1.5** RPP MUST support disclosure and privacy preferences equivalent to EPP “disclose”.

**C1.6** RPP MUST support contact object to refer to external identity provider (e.g. when digital identity schemes are used), where the personal data would not be persisted within RPP server. RPP SHOULD allow to only store a stable identifier, reference or credential for future verification (see Privacy Considerations).

**C1.7** RPP MUST enforce referential integrity. A contact MUST not be deleted when it is referenced by other objects. RPP MUST return a conflict error when deletion is disallowed and the contact representation MAY include an attribute with information about linked objects.

**C1.8** RPP SHOULD consider renaming the EPP contact object type to "entity" to better align with the RDAP data model, defined in [@!RFC9083].

### Operations

**C2.1** The RPP contact object type is mapped to the EPP equivalent and MUST support all operations (commands) defined for the contact object in [@!RFC5733], such as check, create, read, update, and delete with the possible exception of transfer command, and include support for partial update semantics available to allow for efficient updates.

**C2.2** RPP MAY support the contact transfer command from EPP.

**C2.3** RPP MAY support searching and listing contacts filtered by name (exact/prefix), and sponsoring client, with pagination, the server MAY use a maximum limit on results.

**C2.4** Functional equivalents for EPP contact statuses (e.g., ok, linked, client/serverUpdateProhibited, client/serverDeleteProhibited, pendingTransfer) MUST be supported, with clear mapping to HTTP/RPP responses. The protocol MUST define which statuses can be set by the server and which can be set by the sponsoring client.

**C2.5** RPP MUST prevent creation of contacts with duplicate ids within registry namespace (TLD) and return a HTTP 409 (Conflict) status on collision.

**C2.6** The protocol MUST provide an operation to retrieve full contact representation. An authorisation mechanism MUST ensure that sensitive data, such as authorisation information, is only returned to the current sponsoring client.

**C2.7** The protocol MUST provide an operation to retrieve an appropriate contact representation to non-sponsoring clients. The representation MAY vary depending if the authorisation information is provided - depending on server policy.

### Data Representation

**C3.1** RPP SHOULD consider using JSContact [@!RFC9553] format for contact representation.

**C3.2** RPP MUST support contact attribute disclosure preferences per field (or field group) and this MUST be mapped to the EPP disclosure preferences described in [@!RFC5733].

### Internationalisation

**C4.1** RPP MUST support internationalisation (character encoding) for contact objects in the following areas:

- name
- address data
- any other contact-related data containing human provided or readable text

**C4.2** RPP MUST support both the localized and internationalized version of the EPP postalInfo element from [@!RFC5733].

**C4.3** RPP MUST support internationalised Email addresses [@!RFC6530] in Contact objects.

**C4.4** RPP MUST support multiple localised expressions of the same data, e.g. fields mentioned in C4.1 having both international and localised variants.

**C4.5** All future RPP contact object extensions MUST be able to handle internationalisation and localisation requirements.

# IANA Considerations

This document has several requirements for the RESTful Provisioning Protocol (RPP) that create considerations for IANA. Future architecture and design documents may identify additional needs for IANA registries.

Therefore, the core RPP specifications MUST include "IANA Considerations" sections that formally request the creation of any necessary IANA registries. These sections MUST also provide the initial registration of values defined within those core documents.

# Security Considerations

The security section of this document defines the security related requirements for RPP, these requirements MUST be addressed in the design and implementation of RPP. Implementations MUST follow best practices, described in [@!BCP56] for HTTP API design.

RRP core specifications MUST include appropriate Security Considerations sections, specifying implementation and operational security requirements for both RPP clients and servers.

# Privacy Considerations

**DP.1** The protocol MUST provide mechanisms to support the implementation of data privacy principles, such as those found in modern data protection frameworks (e.g., GDPR). These mechanisms MUST support, at a minimum, the principles of data minimisation and purpose limitation.

**DP.2** To support data minimisation, the protocol MUST allow clients to provide and manage only the data that is strictly necessary for a specific purpose. The protocol MUST also allow for different representations of an object, so that a client can request a representation containing only the data it needs and server can return the data a client is authorised to access (See also R4.3 and R6.1).

**DP.3** The protocol's operations and data models MUST be sufficiently flexible to allow an operator to implement workflows for exercising data subject rights, such as access, rectification, and erasure of personal data, in a manner consistent with the operational and policy constraints of the provisioning environment.

**DP.4** The protocol MUST provide services to identify data collection policies and privacy practices. Information about data collection, retention, and privacy policies MUST be included in the service discovery document, enabling clients to understand how personal and sensitive data is handled.

{removeInRFC="true"}
# Changes History

## Version -01 to -02

* Added relevant and not yet covered requirements from [@?RFC3375] (R6.5-R6.8, R12.4, (#obj_transfers))
* Added R6.4 RPP MUST include a functional equivalent of the EPP Poll command.
* Added requirements for the contact object type
* The security considerations section has been restructured and expanded to provide more detailed guidance on security best practices for RPP implementations.
* Added additional security requirements.
* R1.2 removed
* added essential and optional extensions sections in (#appendix_extensions)
* Added generic IANA considerations

## Version -00 to -01

* Added Privacy Considerations section
* R1.5 has been changed to MUST instead of MAY.
* R1.6 has been changed to MUST instead of SHOULD.
* Updated the entire text to make consistent use of the British spelling style.
* stripped down version history of pre-WG -00 to -01

## Version -01 to -00 (WG)

* The document has been adopted by the working group, the version number has been reset from -01 to -00.

## Version -00 to -01

* Structurally reorganised the document, renumbering all requirements and adding new sections for Operations, Clients, and Internationalisation.
* Replaced specific technology mandates with a general requirement to leverage RESTful best practices.
* Introduced a formal framework for extensions, including versioning and discoverability.
* Significantly expanded the security model to support granular, multi-user authorisation.
* Mandated support for partial object updates and asynchronous processing for long-running operations.
* Detailed the requirements for a machine-readable discovery document (/.well-known).
* Added mandatory support for internationalisation (i18n) and specified considering JSContact for contact objects.

{backmatter}

# Extensions {#appendix_extensions}

<!--
A> // List of required extensions here
A> // see: https://github.com/ietf-wg-rpp/rpp-requirements/issues/19
-->
## Essential extensions

A> TODO: These lists are far from being complete -> input from Tiger Team on EPP Extensibility will fill these lists

The following list of extensions is considered essential for the completeness of RPP as provisioning protocol for domain names.
The core RPP and its extensibility framework MUST enable creation of those extensions.

**A.1** *Moved to (#appendix_extensions_optional) as A2.2*

**A1.1** An extension that allows a DNS operator to update the DNSSEC key material for a domain object. This extension MAY be used by the DNS operator to update the DNSSEC key material for a domain object, without the need for the registrar to be involved in this process.

## Optional extensions {#appendix_extensions_optional}

The following list of extensions is considered as possible need for certain deployments of RPP, however other solutions outside of RPP would be possible. Therefore RPP and its extensibility framework MAY enable creation of those extensions, however it is not a MUST criteria.

**A2.1** An extension that allows generating a representation of a historical overview for an object, e.g. show all events linked to the object (create, update ...). The historical time window is determined by server policy and ist included in the discovery service document.

A> TODO: [Issue #57](https://github.com/ietf-wg-rpp/rpp-requirements/issues/57)

**A2.2** An extension for a Search API to allow for searching for objects in the registry database. Includes advanced search capabilities for object info request.


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

<reference anchor="RAML" target="https://raml.org/">
  <front>
    <title>RESTful API Modeling Language</title>
    <author>
      <organization>raml.org</organization>
    </author>
    <date year="2025"/>
  </front>
</reference>

<reference anchor="OpenAPI" target="https://www.openapis.org/">
  <front>
    <title>OpenAPI Specification</title>
    <author>
      <organization>openapis.org</organization>
    </author>
    <date year="2025"/>
  </front>
</reference>
