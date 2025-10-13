Source: https://docs.google.com/document/d/1WR00oB43XZCDqD0zvRvRajuWAq_9wQ3c0RrFKlGC3So/edit?usp=sharing

# 4.1.2.2. RPP Extensibility Recommendations

- Process Information: The protocol must foresee mechanisms for returning additional transient, non-persistent information about the result of an operation, such as a flag indicating a bundling discount was applied or the fees are credits applied (3.13 Personal Registration, 3.18 Premium Domain, 3.25 Launch Phase, 3.30 Registry Fee, 3.59 Domain Charge)

A> added in **R10.12**

- Process Parameters: The protocol must foresee mechanisms for providing additional transient parameters or non-persistent data to the standard processes, such like delete or create (3.8 IDN Language Tag, 3.13 Personal Registration, 3.15 Whois Info, 3.25 Launch Phase, 3.29 Change Poll, 3.30 Registry Fee, 3.45 IDN Mapping, 3.52 .NO EPP Extensions, 3.53 Namestore, 3.59 Domain Charge, 3.61 IDN Mapping - Identity Digital)

A> added in **R10.11**

- Status Information: Allow extensions to add additional information to statuses (3.50 .BR extension)

A> added **R10.13**

- Error Handling: Support extensible and more specific error codes and messages to provide clearer feedback for extension-related operations (3.13 Personal Registration).

A> covered in refined **R10.5** and **R1.5**

- Alternative signed data representations: Support extensibility by inclusion of alternative representations with digitally signed attributes to provide verifiable proof of third-party data validation (3.35 Verification Code Extension, 3.41 China Name Verification Mapping)  A form of digitally signed data does not need to be part of the core protocol, but interested parties could define a best practice.

A> already covered in **R5.7**, **R5.2**, **R5.4** and **R10.2**.

- New Object Types: The protocol must provide a clear mechanism for defining and provisioning entirely new and custom object types beyond the core set (3.10 Email Forwarding, 3.48 Autonomous System Number Mapping, 3.11 Defensive Registration, 3.12 NameWatch, 3.20 Verisign Registry Mapping, 3.42. Registry Mapping, 3.49. IP Network).

A> Covered in **R10.2**

- Adding Properties: Provide a generic and standardized extensibility point for adding new persistent properties to any existing object type, such as adding new fields to a contact or domain (3.3 ENUM Validation, 3.16 Jobs Contact, 3.36 .at EPP Verification, 3.54 LvContact extension, 3.50 .BR extension, 3.45. Internationalized Domain Name, 3.51 .BR extension).

A> Covered in **R10.2**

- New Command Types: Allow for the extension of objects with new commands or process triggers beyond basic CRUD operations. The extension point shall not only consider general purpose processes but also uncoordinated extensibility of provider processes including conflict avoidance in naming conventions (3.1 RGP, 3.25 Launch Phase, 3.7 ConsoliDate Mapping, 3.9 WhoWas, 3.14 Suggestion, 3.19 Balance, 3.20 Verisign Registry Mapping, 3.42 Registry Mapping, 3.47 Validate, 3.52 .NO EPP Extensions, 3.60 Finance). This would be corresponding to Command Type Extension and Function Extension of EPP.

A> covered in **R10.1** and **R10.7**

- Read-only Resources: Enable extensions to define first-class objects for read-only entities, such as a registrar account, to which related information like account balance can be logically associated as sub-resources (3.19 Balance Mapping, 3.20 Verisign Registry Mapping, 3.42. Registry Mapping, 3.43. Launch Phase Policy, 3.44. Login Security Policy).

A> covered in new **R4.9** and adjusted **R10.1**

- Read-only Functions: Enable the addition of read-only functions or informational resources that can perform checks or validations without being tied to a persistent provisioning object (3.9 WhoWas, 3.14 Suggestion, 3.19 Balance, 3.47 Validate Mapping, 3.60 Finance).

A> covered in new **R6.9** and adjusted **R10.1**

- Alternative resource addressability: Protocol design shall include a common pattern for extensibility allowing alternative addressing of resources. One resource can be addressed in multiple ways (e.g., A-label and U-label domain name), and there should be one way to address the alternative forms of resources.  Example: domain name addressable through IDN variant with or without IDN table or language tag (3.8 IDN Language Tag, 3.46 IDN Table Name)

A> covered in new **R3.6** and adjusted **R10.1**

- Object Relationships: Define generic patterns for extensibility with managing associations between provisioning objects (aggregation), such as for variants or bundled domain registrations (3.21 Related Domain Extension, 3.27 Organization Mapping, 3.28 Organization Extension, 3.33 Strict Bundling Registration, 3.50 .BR extension).

A> covered in **R4.7**, **R5.12** and adjusted **R10.1**

- Related object(s): Define generic patterns for extensibility with related resources and collections (composition), such as history information related to a domain name (3.9 WhoWas)

A> covered in new **R4.10** and adjusted **R10.1**

# 4.1.2.3. Object Specific Recommendations
## 4.1.2.3.1 Domain Names

- Lifecycle Management: Allow extensions to define new object statuses and alter lifecycle behavior, such as implementing a scheduled deactivation or custom deletion processes (3.1 RGP, 3.22 .SE EPP Extensions, 3.55 LvDomain extension).
- Transfer Authorization: Foresee extensions that can define alternative transfer authorization methods beyond a simple password, such as using one-time tokens (3.52 .NO EPP Extensions, 3.22 .SE EPP Extensions).
- Transfer Data: Allow for DNS data (e.g., nameservers, DNSSEC) to be updated as part of a transfer operation - either as extensibility point (Process Parameters) or directly embedded in the protocol core for domains (3.22 .SE EPP Extensions).
- IDN Management: For Internationalized Domain Names (IDNs), the protocol should consider how different language variants are addressed and represented, and provide a means to discover server-side IDN policies and tables (3.8 IDN Language Tag, 3.46 IDN Table Name).
- Resource Record Provisioning: Provide a generic and extensible interface for provisioning zone resource records (RRs) (3.2 E.164 Number Mapping).
- Nameserver Representation: Allow extensions to define alternative ways of expressing nameserver information, such as using managed nameserver sets (NSsets) instead of individual host objects (3.57 FRED object extension).
Contact types: Allow extensions to define new custom contact types in a fashion reducing duplicate entries (3.50 .BR extension)


## 4.1.2.3.2 Contacts
- Custom Properties: The protocol must allow extensions to add new properties and define new types or roles for contact objects (generalized as Adding Properties above).


## 4.1.2.3.3 Host Objects
- None, but see related extensibility with Domain Names


# 4.1.2 EPP Extension Recommendations

## Embed - Embed support directly into the standard RPP mappings, such as the RPP domain name mapping, in priority order.  

- Domain Name System (DNS) Security Extensions Mapping for the Extensible Provisioning Protocol (EPP)
- Domain Registry Grace Period Mapping for the Extensible Provisioning Protocol (EPP) (Enhance)
- Login Security Extension for the Extensible Provisioning Protocol (EPP)
- Extensible Provisioning Protocol (EPP) Unhandled Namespaces
- Extensible Provisioning Protocol (EPP) Secure Authorization Information for Transfer
- Extensible Provisioning Protocol (EPP) mapping for DNS Time-To-Live (TTL) values
- Organization Mapping for the Extensible Provisioning Protocol (EPP)
- Organization Extension for the Extensible Provisioning Protocol (EPP)
- Use of Internationalized Email Addresses in the Extensible Provisioning Protocol (EPP)
