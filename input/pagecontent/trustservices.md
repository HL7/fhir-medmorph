This section defines the specific requirements for a Trust Service Provider as specified in this MedMorph Reference Architecture (RA) Implementation Guide (IG). In order to better understand the trust services, readers can refer to the following documentation.

* [HHS Guidance on De-Identification](https://www.hhs.gov/hipaa/for-professionals/privacy/special-topics/de-identification/index.html)

* [IHE IT Infrastructure Handbook on De-Identification](https://www.ihe.net/uploadedFiles/Documents/ITI/IHE_ITI_Handbook_De-Identification_Rev1.1_2014-06-06.pdf). This includes literature on de-identification, pseudonymization and re-linking.

* [National Institute of Standards and Technology (NIST), Secure Hashing website](https://csrc.nist.gov/projects/hash-functions)

* [NIST Documentation on de-identify and re-identify](https://nvlpubs.nist.gov/nistpubs/ir/2015/nist.ir.8053.pdf)

* [Additional information on trust services from CDC](https://www.cdc.gov/nchs/data/series/sr_02/sr02_143.pdf).

### Trust Service Provider Requirements

A Trust Service Provider enables trust services such as anonymization, de-identification, re-identification, hashing, and pseudonymization. These services are used when required (e.g., to de-identify data sent to research organizations).

The next section identifies specific requirements for a Trust Service Provider:

* Trust Service Provider SHALL support the APIs defined by the [Trust Service Provider Capability Statement](CapabilityStatement-medmorph-trust-service-provider.html).

* Trust Service Providers SHALL allow re-identification of bundles that were previously de-identified.

* Trust Service Providers SHOULD implement  algorithms specified by the content IGs for the different use cases. Trust Service Provider MAY  choose their own anonymization, de-identification, re-identification, hashing and pseudonymization algorithms in case none are specified in the content IGs.

* Trust Service Providers MAY ignore extensions on data elements when they are not understood by the Trust Service Provider. 

* Content IGs will identify specific data elements within resources that need to be processed by Trust Services based on the use case.


