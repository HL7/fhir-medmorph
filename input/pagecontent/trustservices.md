This section of the implementation guide defines the specific requirements for a Trust Service Provider in the MedMorph context.

### Trust Services

Trust Service Provider enables trust services such as anonymization, de-identification, re-identification, and pseudonymization. These services are uses when required. (For example, to send data to research organizations the data may have to be de-identified whereas for public health reporting for cancer does not require de-identification of data).

The next section identifies specific requirements for Trust Service Provider:

* Trust Service Provider SHALL support the APIs defined by the [Trust Service Provider Capability Statement](CapabilityStatement-medmorph-trust-service-provider.html).

* Trust Service Providers SHALL be able to re-identify bundles that were de-identified by itself using the de-identify operation.

* Trust Service Providers MAY choose their own anonymization, de-identification, re-identification, and pseudonymization algorithms.

* Content IGs will identify specific data elements within resources that need to be processed by Trust Services based on the use case.

