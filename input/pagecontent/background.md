### Implementation Guide (IG) Overview

Read the <a href="usecases.html">Use Cases</a> section for background on the initial representative MedMorph use cases that have been used to create the data exchange abstract model and workflows from which the MedMorph Reference Architecture (RA) is derived. 
The MedMorph Reference Architecture (RA) IG aims to minimize the burden on both the senders and receivers of data by providing a common method for obtaining data for research and public health for multiple use cases. The CDC established a Technical Expert Panel (TEP) comprised of a wide variety of representatives with relevant expertise to inform the development of the technical approach and the MedMorph RA IG.

### Guiding Principles 

The following are the guiding principles for the MedMorph Reference Architecture IG.

* Reduce provider burden wherever possible while making electronic data more available to research and public health.
* Align with existing standards (e.g., HL7 FHIR, CDA) and regulations (e.g., ONC 2015 Edition, 2015 Edition Cures Update, Trusted Exchange Framework and Common Agreement (TEFCA), etc.) while improving the timeliness and completeness of data received by research and public health.
* Promote standardized configurability of implementations to allow flexibility for customizations in workflows while supporting interoperability. 

### IG In-Scope Requirements

The following requirements are in-scope for the MedMorph RA IG based on the use cases.

* Define the API mechanisms, Inputs, and Outputs used to access and exchange data.
* Define the mechanisms used to trigger the workflows. 
* Define the provisioning mechanisms used to automate the triggering and reporting of data. 
* Define trust services (e.g., pseudonymization, anonymization, de-identification) needed for use cases when appropriate.

### IG Out-of-Scope 

The following aspects are out-of-scope for the MedMorph RA IG based on the use cases.

* Enabling claims data access to research and public health.
* Changes to the EHR data capture screens and/or changes to clinical workflows. Providers may use their choice of apps/screens/systems to enter the data into the EHR independent of the IG.
* Data not accessible by a FHIR API.
* Policies and processes followed by healthcare organizations to allow data sharing,  collecting of consent, or compliance with regulatory requirements. The technical APIs to exchange required data for the use cases are in-scope as specified in the In-Scope section.


### Underlying Specifications

This guide is based on the [HL7 FHIR R4]({{site.data.fhir.path}}index.html) standard, as well as [US Core IG]({{site.data.fhir.uscoreR4}}/index.html), [Bulk Data Access IG]({{site.data.fhir.bullkig}}/index.html), [SMART App Launch IG for Backend Services Authorization]({{site.data.fhir.smartapplaunch}}/backend-services.html) and the [Subscriptions R5 Backport IG]({{site.data.fhir.subscriptionsig}}/index.html) specifications. This MedMorph RA IG is intended to maximize the number of clinical systems that can conform to this guide as well as to allow for extensibility of system capabilities in the future.

Implementers of the MedMorph RA IG must understand some basic information about the underlying specifications listed above.


#### FHIR Resources used for MedMorph RA

The table below identifies the specific FHIR Resources and their use in the MedMorph RA IG.
Implementers should familiarize themselves with these FHIR resources and their purposes.

<table>
  <thead>
    <tr>
      <th>FHIR Resource</th>
      <th>MedMorph Purpose</th>
    </tr>
  </thead>
  <tr>
    <td><a href="{{site.data.fhir.path}}plandefinition.html">PlanDefinition</a></td>
	<td>Used to define the Knowledge Artifacts which contains the definitions of all the activities, events, conditions that can be executed as part of workflows.</td>
  </tr>
  <tr>
    <td><a href="{{site.data.fhir.path}}library.html">Library</a></td>
	<td>Used to specify logic expressions, queries, trigger codes to be used as part of PlanDefinition.</td>
  </tr>
  <tr>
    <td><a href="{{site.data.fhir.path}}valueset.html">ValueSet</a></td>
	<td>Used to group codes for various activities such as trigger codes, activity codes.</td>
  </tr>
  <tr>
    <td><a href="{{site.data.fhir.path}}bundle.html">Bundle</a></td>
	<td>Used to group resources for creating submission reports.</td>
  </tr>
  <tr>
    <td><a href="{{site.data.fhir.path}}messageheader.html">MessageHeader</a></td>
	<td>Used to specify metadata for message routing.</td>
  </tr>
  <tr>
    <td><a href="{{site.data.fhir.path}}organization.html">Organization</a></td>
	<td>Representing Sending, Receiving and Intermediary organizations.</td>
  </tr>
  <tr>
    <td><a href="{{site.data.fhir.path}}location.html">Location</a></td>
	<td>Used to represent locations in workflows.</td>
  </tr>
  <tr>
    <td><a href="{{site.data.fhir.path}}endpoint.html">Endpoint</a></td>
	<td>Used to represent sender and receiver addresses during data exchange.</td>
  </tr>
  <tr>
    <td><a href="{{site.data.fhir.path}}subscription.html">Subscription</a></td>
	<td>Used to create subscriptions based on trigger events.</td>
  </tr>
</table>



### IG Types and their Relationships 

The MedMorph project’s vision is to provide a common framework (i.e., the RA) that can be used for multiple public health and research use cases. For each use case there will be content specific data, workflow requirements, and operational requirements necessary for implementation. The use case specific content will be specified in additional IGs referred to as content IGs . Each content IG would generally apply to one use case, however it is possible that existing content IGs that are broader in scope may cover all or some of the requirements needed by a use case, as described in the next section with the Hep C example. Future implementers of other use cases and content IGs not documented within the MedMorph RA IG may also seek to implement the MedMorph RA IG based on their data, workflow, and operational requirements.
The relationships among different FHIR IG types are depicted in Figure 2.1, with the more general types at the top and more specific types at the bottom. Typically, the more general an IG is, the more reusable its components; and the more specific an IG is, the more use case specific it is.   

{% include img.html img="IGRelationship.png" caption="Figure 2.1: Relationship among Different IG Types" %}

**NOTE to Readers:** 

Although the US PH Profiles Library is in the process of being balloted at the time of publication of v1.0.0 of the MedMorph RA IG, it is included to show the direction MedMorph will take under the guidance of the HL7 Public Health Work Group (WG). The US PH Profiles Library will harmonize similar profiles from multiple IGs being produced under the sponsorship of the HL7 Public Health WG. This will ensure consistency across IGs, reduce profile proliferation, and provide a starting point for future content IGs.


#### Relationship between MedMorph RA IG, electronic Case Reporting (eCR) FHIR IG, and US Public Health (PH) Profiles Library 

The [United States Public Health (US PH) Profiles Library](https://build.fhir.org/ig/HL7/fhir-us-ph-common-library-ig/index.html) establishes a baseline of common artifacts that will be used by multiple public health IGs as shown in Figure 2.1. It provides a way to harmonize profiles needed for multiple public health use cases that are not already in US Realm foundational IGs such as US Core. The initial content for the US PH Profiles Library is derived from the MedMorph RA and eCR FHIR IGs, and in the future will likely include reusable profiles from other IGs as well (see Section on “Other Existing Public Health IGs contributing to a common US PH Profiles Library” for more details). The MedMorph RA IG and v2.0.0 of the [eCR FHIR IG](http://hl7.org/fhir/us/ecr/STU2/) were balloted in January 2021, and both of these IGs created aligned artifacts that can be reflected in the US PH Profiles Library, which is being balloted in September 2022. All artifacts that are candidates for promotion to the US PH Profiles Library will use the words “us” or “US” and “ph” or “PH” as part of the profile definition, name, and title elements. This RA IG will harmonize with the US PH Profiles Library going forward to reduce implementer burden for supporting public health use cases.
At the architectural level, this RA IG aligned with and then expanded from the architectural components of the eCR FHIR IG. In this way, the MedMorph and eCR architectures are harmonized while the MedMorph RA also allows for public health use cases beyond reportable conditions (which is the focus of eCR), research use cases, and potentially other types of use cases in the future. 

At the content IG level, the eCR FHIR IG contains content profiles (identified as eICR Content Profiles in eCR FHIR IG in Figure 2.1) related to electronic case reporting of reportable conditions. There are three different permutations for how a new use case might leverage these profiles or profiles in other existing FHIR IGs, as shown in Figure 2.1 with the initial MedMorph use cases as examples.

For example, the MedMorph Hep C use case data requirements overlap with the content specified for electronic case reporting. Creation of a new Hep C content IG would replicate most, if not all, of the content specified as eICR Content Profiles in the eCR FHIR IG, which is undesirable. To minimize proliferation of profiles and IGs, which increases implementation burden, the MedMorph Hep C use case will reuse the eICR Content Profiles from eCR FHIR IG from a content perspective rather than create a separate content IG. An example of partial reuse is the Cancer Registry Reporting Content IG, which partially overlaps with the eICR Content Profiles in eCR FHIR IG but also requires additional profiles to fully meet its data requirements. These overlapping profiles have been refactored into the US PH Profiles Library, which can then be reused by the Central Cancer Registry Reporting Content IG. Any specific data elements needed for the Cancer Registry Reporting use case not included in the US PH Profiles Library will be added to the Central Cancer Registry Reporting Content IG. The Health Care Surveys Content IG overlaps minimally with the eICR FHIR IG from a content perspective, however the Health Care Surveys Content IG can use the MedMorph RA components for the reporting function. The Health Care Surveys Content IG references the MedMorph RA IG and add profiles and/or extensions, etc. needed for the Health Care Surveys  use case not included in the MedMorph RA IG (e.g., Health Care Surveys content specifications).

#### Relationship between MedMorph RA IG and US Core FHIR IG  

As shown in Figure 2.1, the MedMorph RA IG is built on other FHIR specifications, including the [US Core IG]({{site.data.fhir.uscoreR4}}/index.html). The MedMorph RA IG will use US Core IG profiles as required for each use case. In addition to the profiles, the patient level FHIR APIs specified in US Core will be leveraged for all public health reporting use cases that leverage the MedMorph RA. The version of the US Core IG referenced is the one widely implemented across the US to support United States Core Data for Interoperability (USCDI) as required in the Office of the National Coordinator (ONC) 21st Century Cures Act certification criteria.

#### Relationship between MedMorph RA IG and FHIR Bulk Data Access IG
 
For research use cases and potentially some public health use cases, specifically when data about multiple patients is needed but does not require real-time reporting, MedMorph will leverage the [Bulk Data Access IG]({{site.data.fhir.bulkig}}/index.html). The Bulk Data Access IG is used to retrieve population level information from EHRs, subject to applying appropriate authorities and policies. The version of the Bulk Data Access IG referenced is the one being widely implemented across the US to support USCDI required in the ONC 21<sup>st</sup> Century Cures Act certification criteria.


#### Relationship between MedMorph RA IG and SMART App Launch IG

The MedMorph RA IG uses [SMART App Launch IG for Backend Services Authorization]({{site.data.fhir.smartapplaunch}}/backend-services.html) to secure all system-to-system interactions among the various actors in the MedMorph RA. The version of the SMART App Launch IG referenced is the being widely implemented across the US to support USCDI as required in the ONC 21<sup>st</sup> Century Cures Act certification criteria.

#### Relationship between MedMorph RA IG and Subscriptions Backport IG

The MedMorph RA IG uses [Subscriptions Backport IG]({{site.data.fhir.subscriptionsig}}/index.html) to enable content IGs to define specific trigger events using Subscription Topics. These subscription topics will be subscribed to by the HDEA per the Knowledge Artifact. Upon receiving notifications, the HDEA will process the notification and automate the MedMorph workflows without additional provider burden.


#### Other Existing Public Health IGs contributing to a common US PH Profiles Library
 
The [Vital Records Common Profile Library](http://hl7.org/fhir/us/vrdr/index.html) was published in August 2021. While it is focused around vital records there are overlapping profiles with [eCR FHIR IG](http://hl7.org/fhir/us/ecr/index.html) and [Occupational Data for Health (ODH) IG](http://hl7.org/fhir/us/odh/index.html). This project was created through the recognition of overlapping birth defects and birth and fetal death FHIR profiles. However, to help harmonize overlapping profiles with other public health use cases such as MedMorph, the VR common library may be a seed for the establishment of the US PH library mentioned above in the MedMorph Reference Architecture IG. 

#### Relationship between MedMorph RA IG and Content IGs 

The MedMorph RA IG does not prescribe content specific to any use case. Rather, it specifies the workflow processing, trigger mechanisms using PlanDefinition, ActivityDefinition, ValueSets, and APIs for message routing using Bundle, MessageHeader, etc. The content IGs would specify the profiles, search parameters, and operations for use case specific requirements but would reuse the MedMorph RA IG to trigger events, submit data, process knowledge artifacts, manage workflows, etc. (e.g., Health Care Surveys Content IG, Cancer Registry Reporting Content IG).


#### Relationship between MedMorph RA IG and Clinical Registry Extraction and Data Submission (CREDS) IG 

The MedMorph RA IG specifies the use of FHIR APIs to collect data from EHRs and potentially other systems and exchange with systems that can receive data in FHIR format. The RA IG can support multiple use cases, including but not limited to case-based surveillance, registry reporting, national health care surveys, and research.

Protocols in the [Clinical Registry Extraction and Data Submission (CREDS) IG](https://build.fhir.org/ig/HL7/fhir-registry-protocols-ig/index.html) focus on providing healthcare provider organizations information on how to collect the data needed to submit to registries. This may include but is not limited to data sources such as EHRs, HIEs, and others, which may use FHIR as well as HL7 CDA documents, and HL7 V3 messages that are not available via FHIR APIs.

Use cases that can obtain all the needed data via MedMorph compliant FHIR APIs should consider the use of the MedMorph RA as the basis for their efforts. Use cases that need to obtain data not available MedMorph should consider the use of CREDS.


#### Relationship between MedMorph RA IG and Hybrid / Intermediary Exchange IG 

The MedMorph RA IG uses FHIR Messaging to route messages with appropriate headers and contents packaged within a reporting bundle, which is a type of Bundle resource. The MedMorph RA IG allows for Trusted Third Parties (TTPs) who act as intermediaries to provide multiple services such as routing, content validation, content conversions, and transport protocol bridging. The [Hybrid/Intermediary Exchange IG](http://hl7.org/fhir/us/exchange-routing/2022Jan/index.html), developed by the FHIR at Scale Taskforce (FAST), identifies best practices for using intermediaries for FHIR Representational State Transfer (RESTful) interactions. The MedMorph RA IG and the Hybrid/Intermediary Exchange IGs are compatible with each other. For example, a MedMorph use case implementation can use an intermediary that conforms to the Hybrid/Intermediary Exchange IG and also implement the TTP capabilities outlined in the MedMorph RA IG.


#### Relationship between MedMorph RA IG and Common Data Model Harmonization (CDMH IG)

The MedMorph RA IG describes a framework for data extraction from EHRs and the process in which Data Marts can load the data for research purposes. While there are many different data models used for research, the MedMorph RA IG examined the [CDMH IG](http://hl7.org/fhir/us/cdmh/index.html), which supports mappings between FHIR and Patient-Centered Outcomes Research Network (PCORnet) Common Data Model(CDM), Informatics for Integrating Biology & the Bedside (i2b2), Sentinel, and Observational Medical Outcomes Partnership (OMOP) data models. These mappings will be useful for the MedMorph Research Data Exchange Content IG and potentially other research use cases. 
