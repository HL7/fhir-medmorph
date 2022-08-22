This section defines the specific requirements for a Trust Service Provider as specified in this MedMorph Reference Architecture (RA) Implementation Guide (IG).

### Trust Service Provider Requirements

A Trust Service Provider enables trust services such as anonymization, de-identification, re-identification, hashing, and pseudonymization. These services are used when required (e.g., to de-identify data sent to research organizations).

The next section identifies specific requirements for a Trust Service Provider:

* Trust Service Provider SHALL support the APIs defined by the [Trust Service Provider Capability Statement](CapabilityStatement-medmorph-trust-service-provider.html).

* Trust Service Providers SHALL allow re-identification of bundles that were previously de-identified.

* Trust Service Providers SHOULD implement  algorithms specified by the Content IGs for the different use cases. Trust Service Provider MAY  choose their own anonymization, de-identification, re-identification, and pseudonymization algorithms in case none are specified in the Content IGs.

* Content IGs will identify specific data elements within resources that need to be processed by Trust Services based on the use case.

