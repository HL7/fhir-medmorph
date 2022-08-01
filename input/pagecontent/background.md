### Implementation Guide Overview

The MedMorph Implementation Guide presented here is also known as the MedMorph Reference Architecture Implementation Guide. Reference Architecture refers to a common framework (e.g., FHIR resources, FHIR APIs, FHIR operations, security mechanisms) that will be leveraged by multiple public health and research use cases. To create a reference architecture that is scalable and flexible the MedMorph project has considered the following use cases: 

* Chronic Hepatitis C Surveillance (an infectious disease, and in the context of this IG relates to chronic hepatitis C reporting instead of acute hepatitis C reporting which is currently ongoing)
* Cancer Registry Reporting (a chronic disease)
* Health Care Surveys (a health care utilization example not pertaining to a specific condition)

These use cases have been used to create the data exchange abstract model and workflows from which the reference architecture is derived. This is further outlined in <a href="usecases.html">Use Cases</a>.

Based on the above use cases, the Reference Architecture IG aims to minimize the burden on both the senders and receivers of data by providing a common method for obtaining data for research and public health for multiple conditions or uses. The CDC has established a Technical Expert Panel (TEP) comprised of a wide variety of representatives with relevant expertise to inform and guide the development of the technical approach and the Reference Architecture Implementation Guide.

### Implementation Guides Types and their Relationships 

The MedMorph project's vision is to create this Reference Architecture Implementation Guide to provide a common framework for multiple public health and research use cases. However, for each use case there will be specific data, workflow requirements, and operational requirements that need to be considered for implementation. The use case specific content will be specified in additional Implementation Guides referred to as **Content Implementation Guides** in this specification, if necessary. It is expected that there will be one Content Implementation Guide per use case that needs to be operationalized, however it is possible that existing Content Implementation Guides that are broader in scope may cover all or some of the requirements needed by a use case, as described in the next section. Future implementers of other public health use cases and content implementation guides not documented within the MedMorph Reference Architecture IG may also seek to implement the MedMorph Reference Architecture IG based on their data, workflow and operational requirements.

#### Relationship between MedMorph IG, Electronic Case Reporting (eCR) FHIR IG, and US Public Health (PH) Library 
The [Electronic Case Reporting (eCR) FHIR IG v2.0.0](http://hl7.org/fhir/us/ecr/STU2/) is currently published. The MedMorph Reference Architecture IG and a newer version of the eCR FHIR IG are being created for the January 2021 HL7 ballot cycle. However, the US Public Health (PH) Library currently does not exist. The US PH Library will be eventually be created and balloted to establish a baseline of common artifacts that will be used by multiple public health implementation guides including the MedMorph Reference Architecture IG, eCR FHIR IG, and the other future Content Implementation Guides shown in Figure 2.1 below. The initial content for the US PH Library will be derived from the MedMorph Reference Architecture and eCR FHIR IGs. For the January 2021 HL7 ballot, both the MedMorph Reference Architecture and eCR FHIR IGs will create artifacts that are aligned so that they can be moved to the US PH Library when it is created. All artifacts which are candidates for being promoted to the US PH Library will use the words "us" or "US" and "ph" or "PH" as part of the profile definition, name and title elements. Although US PH Library does not exist currently, it is being discussed in this paragraph to indicate the future direction to harmonize multiple implementation guides that will reduce implementer burden for supporting public health use cases.

At the content IG level, the eCR FHIR IG contains content profiles (identified as eICR Content Profiles in eCR FHIR IG in Figure 2.1 below) related to electronic case reporting. There are three different permutations for how a new use case might be able to leverage (or not) this or other existing FHIR IGs, as shown in Figure 2.1 with the MedMorph modeled use cases. 

For example, the MedMorph Hep C use case overlaps with electronic case reporting. Creation of a new Hep C IG would replicate most, if not all, of the content specified as eICR Content Profiles in eCR FHIR IG, which is undesirable. In order to minimize proliferation of profiles and implementation guides, the MedMorph Hep C use case will reuse the eICR Content Profiles from eCR FHIR IG from a content perspective. Similarly, other content IGs that get created in the future would first examine existing content IGs and only create new profiles and data elements that are not present in other content IGs. For example, the Cancer Registry Reporting use case partially overlaps with the eICR Content Profiles in eCR FHIR IG. These overlapping profiles have been refactored into the US Public Health Library which will be used by the Central Cancer Registry Reporting Content IG.  Any specific data elements needed for the Cancer Registry Reporting use case that are not included in the US Public Health Library will be added to the Central Cancer Registry Reporting Content IG. On the other hand, the Health Care Surveys IG overlaps minimally with the eICR FHIR IG from a content perspective however the IG can use the MedMorph Reference Architecture components for reporting function. Therefore, the Health Care Surveys content IG would reference the MedMorph Reference Architecture IG and then add profiles and/or extensions, etc. needed for the Health Care Surveys use case that are not included in the MedMorph Reference Architecture IG (e.g., Health Care Surveys content specifications).

{% include img.html img="IGRelationship.png" caption="Figure 2.1: Relationship among Different IG Types" %}

**NOTE to Readers:** 

Although the US PH Library has not been balloted yet, it is included to show the direction that we will be taking under the guidance of the HL7 Public Health WG. The US PH Library will harmonize similar profiles from multiple IGs being produced under the sponsorship of the HL7 Public Health WG. This will ensure consistency across implementation guides, reduce profile proliferation, and provide a starting point for future Content Implementation Guides.  

#### Relationship between MedMorph IG and US Core FHIR IG  

As shown in Figure 2.1 above, the MedMorph Reference Architecture IG is built on other FHIR specifications including the [US Core IG.]({{site.data.fhir.uscoreR4}}/index.html) The MedMorph Reference Architecture IG will use US Core IG profiles as required for each use case. In addition to the profiles, the patient level FHIR APIs specified in US Core will be leveraged for all public health reporting use cases that leverage the MedMorph Reference Architecture. The version of the US Core IG selected is the one that is being widely implemented across the US to support USCDI required by the ONC 21<sup>st</sup> Century Cures Act. 


#### Relationship between MedMorph IG and FHIR Bulk Data Access IG
 
For research use cases, specifically where data about multiple patients need to be retrieved, MedMorph will use FHIR Bulk Data Access specified in the [BulkDataAccess IG]({{site.data.fhir.bulkig}}/index.html). The Bulk Data Access Implementation Guide is used to retrieve population level information from EHRs, subject to applying appropriate authorities and policies. The version of the Bulk Data Access IG selected is the one that is being widely implemented across the US to support USCDI required by the ONC 21<sup>st</sup> Century Cures Act.


#### Relationship between MedMorph IG and SMART App Launch IG
The MedMorph Reference Architecture IG will also leverage SMART App Launch IG for the Backend Services Authorization protocols. The Backend Services Authorization in [SMART App Launch IG]({{site.data.fhir.smartapplaunch}}/index.html) is used to secure system to system communication transactions in all MedMorph workflows.

#### Other Existing Public Health IGs that could contribute to a common US PH Library
 
The [Vital Records Common Profile Library](http://hl7.org/fhir/us/vrdr/index.html) was published in August 2021. While it is focused around vital records there are overlapping profiles with [eCR FHIR IG](http://hl7.org/fhir/us/ecr/index.html) and [Occupational Data for Health (ODH) IG](http://build.fhir.org/ig/HL7/us-odh/index.html). This project was created through the recognition of overlapping birth defects and birth and fetal death FHIR profiles. However, to help harmonize overlapping profiles with other public health use cases such as MedMorph, the VR common library may be a seed for the establishment of the US PH library mentioned above in the MedMorph Reference Architecture IG. 

#### Relationship between MedMorph IG and Content Implementation Guides 

The MedMorph Reference Architecture IG does not prescribe content specific to any use case. Rather, it specifies the workflow processing, trigger mechanisms using PlanDefinition, ActivityDefinition, ValueSets, and APIs for message routing using Bundle, MessageHeader, etc. The Content Implementation Guide, such as the Health Care Surveys IG, would specify the profiles, search parameters, and operations for use case specific requirements but would reuse the MedMorph Reference Architecture IG to trigger events, submit data, process knowledge artifacts, manage workflows, etc. 

#### Relationship between MedMorph IG and CREDS IG 

The MedMorph Reference Architecture (RA) specifies the use of FHIR APIs to collect data from EHRs and potentially other systems and exchange with systems that can receive data in FHIR format that aligns with the MedMorph Content IG. The RA can support multiple use cases, including but not limited to case-based surveillance, registry reporting, national health care surveys, and research.

[CREDS IG](https://build.fhir.org/ig/HL7/fhir-registry-protocols-ig/index.html) focuses on providing healthcare provider organizations information on how to collect the data needed to submit to registries. This may include but is not limited to data sources such as EHRs, HIEs and other sources using FHIR as well as HL7 CDA documents, and HL7 V3 messages that are not available via FHIR APIs.

Use cases that can obtain all the needed data via MedMorph compliant FHIR APIs should consider the use of the MedMorph RA as the basis for their efforts. Use cases that need to obtain data not available MedMorph should consider the use of CREDS.


#### Relationship between MedMorph IG and Hybrid / Intermediary Exchange IG 

The MedMorph Reference Architecture IG uses FHIR Messaging to route messages with appropriate headers and contents packaged within a reporting bundle which is a resource of type Bundle. The MedMorph Reference Architecture IG allows for Trusted Third Parties who act as intermediaries to provide multiple services such as routing, content validation, content conversions and transport protocol bridging. The [Hybrid/Intermediary Exchange IG](http://hl7.org/fhir/us/exchange-routing/2022Jan/index.html) developed by the FHIR at Scale Taskforce (FAST) team, identifies the best practices to use intermediaries for FHIR RESTful interactions. MedMorph and the Hybrid/Intermediary Exchange IGs are compatible with each other. For example, a MedMorph use case implementation can use an intermediary that conforms to the Hybrid/Intermediary Exchange IG and also implement the Trusted Third Party capabilities outlined in the MedMorph IG.

#### Relationship between MedMorph IG and  SMART App Launch IG for Backend Services Authorization
The MedMorph Reference Architecture implementation guide will use Backend Services Authorization to secure all system-to-system interactions among the various actors in the MedMorph Reference Architecture. The version of the SMART App Launch IG selected is the one that is being widely implemented across the US to support USCDI required by the ONC 21<sup>st</sup> Century Cures Act.

<table>
  <tr>
    <td><a href="{{site.data.fhir.smartapplaunch}}/backend-services.html">Backend Services Authorization  - FHIR R4 based IG</a></td>
  </tr>
</table>

#### Relationship between MedMorph IG and Common Data Model Harmonization (CDMH IG)

The MedMorph Reference Architecture implementation guide describes a framework for data extraction from EHRs and allow Data Marts to load the data for research purposes. While there are many different data models that are used for research, the MedMorph RA IG examined the CDMH IG which supports mappings between FHIR and PCORnet CDM, i2b2, Sentinel, OMOP data models. These mappings will be extremely useful for the Research Content IG and hence it is advised that readers who are interested in the research use cases should familiarize with the CDMH IG.

<table>
  <tr>
    <td><a href="http://hl7.org/fhir/us/cdmh/index.html">CDMH IG  - FHIR R4 based IG</a></td>
  </tr>
</table>


### Guiding Principles 

The following are the guiding principles for the MedMorph Reference Architecture IG.

* Reduce provider burden wherever possible while making EHR data more available to research and public health.
* Align with existing standards (e.g., HL7 FHIR, CDA) and regulations (e.g., ONC 2015 Edition, 2015 Edition Cures Update, Trusted Exchange Framework and Common Agreement (TEFCA), etc.) while improving the timeliness and completeness of data received by research and public health.
* Promote standardized configurability of implementations to allow flexibility for customizations in workflows. 

### IG In-Scope Requirements

The following requirements are in-scope for the MedMorph Reference Architecture IG based on the use cases.

* Define the API mechanisms, Inputs, and Outputs used to access and exchange data. 
* Define the mechanisms that will be used to trigger the workflows. 
* Define the provisioning mechanisms that could be used to automate the triggering and reporting of data. 
* Define trust services (e.g., pseudonymization, anonymization, de-identification) that will be needed for certain use cases.

### IG Out-of-Scope 

The following aspects are out-of-scope for the MedMorph Reference Architecture IG based on the use cases.

* Enabling claims data access to research and public health.
* Changes to the EHR data capture screens and/or changes to clinical workflows. Providers may use their choice of apps/screens/systems to enter the data into the EHR independent of the IG.
* Any data not accessible by a FHIR API.
* Policies and processes followed by healthcare organizations to allow data sharing,  collecting of consent, compliance with regulatory requirements are out-of-scope. The technical APIs to exchange required data for the use cases are in-scope as specified in the In-Scope section.


### Underlying Specifications

This guide is based on the [HL7 FHIR]({{site.data.fhir.path}}index.html) standard, as well as [US Core IG]({{site.data.fhir.uscoreR4}}/index.html), [Bulk Data IG]({{site.data.fhir.bullkig}}/index.html), [SMART App Launch IG for Backend Services Authorization]({{site.data.fhir.smartapplaunch}}/backend-services.html) and the [Subscriptions R5 Backport IG]({{site.data.fhir.subscriptionsig}}/index.html) specifications. This reference architecture is intended to maximize the number of clinical systems that can conform to this guide as well as to allow for easy growth and extensibility of system capabilities in the future.

Implementers of this specification must understand some basic information about the underlying specifications listed above.


#### FHIR

The MedMorph Reference Architecture implementation guide uses terminology, notations and design principles that are
specific to the HL7 FHIR standard. Before reading this implementation guide, it is important to be familiar with the basic principles of FHIR and how to read FHIR specifications. Readers who are unfamiliar with FHIR are encouraged to review the following prior to reading the rest of this implementation guide.

* [FHIR overview]({{site.data.fhir.path}}overview.html)
* [Developer's introduction]({{site.data.fhir.path}}overview-dev.html) (or [Clinical introduction]({{site.data.fhir.path}}overview-clinical.html))
* [FHIR data types]({{site.data.fhir.path}}datatypes.html)
* [Using codes]({{site.data.fhir.path}}terminologies.html)
* [References between resources]({{site.data.fhir.path}}references.html)
* [How to read resource & profile definitions]({{site.data.fhir.path}}formats.html)
* [Base resource]({{site.data.fhir.path}}resource.html)

This implementation guide supports the recently published [FHIR R4]({{site.data.fhir.path}}index.html) version of the FHIR standard to ensure alignment with the current direction of the FHIR standard. 

#### FHIR Resources used for MedMorph Reference Architecture

The table below identifies the specific FHIR Resources and their purposes that will be used in the MedMorph Reference Architecture IG.
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
	<td>Used to define the Knowledge Artifacts which contains the definitions of all the activities, events, conditions that can be executed as part of the workflows.</td>
  </tr>
  <tr>
    <td><a href="{{site.data.fhir.path}}library.html">Library</a></td>
	<td>Used to specify logic expressions, queries, trigger codes to be used as part of ActivityDefinition and PlanDefinition.</td>
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
	<td>Used to specify metadata for routing.</td>
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
	<td>Used to represent routing information for various WebServices.</td>
  </tr>
  <tr>
    <td><a href="{{site.data.fhir.path}}subscription.html">Subscription</a></td>
	<td>Used to create subscriptions based on trigger events.</td>
  </tr>
</table>
