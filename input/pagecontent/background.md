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
The [Electronic Case Reporting (eCR) FHIR IG v1.0.0](http://hl7.org/fhir/us/ecr/) is currently published. The MedMorph Reference Architecture IG and a newer version of the eCR FHIR IG are being created for the January 2021 HL7 ballot cycle. However, the US Public Health (PH) Library currently does not exist. The US PH Library will be eventually created to establish a baseline of common artifacts that will be used by multiple public health implementation guides including the MedMorph Reference Architecture IG, eCR FHIR IG, and the other future Content Implementation Guides shown in Figure 2.1 below. The initial content for the US PH Library will be derived from the MedMorph Reference Architecture and eCR FHIR IGs. For the January 2021 HL7 ballot, both the MedMorph Reference Architecture and eCR FHIR IGs will create artifacts that are aligned so that they can be moved to the US PH Library when it is created. All artifacts which are candidates for being promoted to the US PH Library will use the words "us" or "US" and "ph" or "PH" as part of the profile definition, name and title elements. Although US PH Library does not exist currently, it is being discussed in this paragraph to indicate the future direction to harmonize multiple implementation guides that will reduce implementer burden for supporting public health use cases.

At the content IG level, the eCR FHIR IG contains content profiles (identified as eICR FHIR IG in Figure 2.1 below) related to electronic case reporting. There are three different permutations for how a new use case might be able to leverage (or not) this or other existing FHIR IGs, as shown in Figure 2.1 with the MedMorph modeled use cases. 

For example, the MedMorph Hep C use case overlaps with electronic case reporting. Creation of a new Hep C IG would replicate most, if not all, of the content in the eICR FHIR IG, which is undesirable. In order to minimize proliferation of profiles and implementation guides, the MedMorph Hep C use case will reuse the eICR FHIR IG from a content perspective. Similarly, other content IGs that get created in the future would first examine existing content IGs such as eICR FHIR IG and only create new profiles and data elements that are not present in the eICR FHIR IG or other content IGs. For example, the Cancer Registry Reporting use case partially overlaps with the eICR FHIR IG and so would create a content IG that references the portions of the eICR FHIR IG that overlap and then add extensions, etc. needed for the Cancer Registry Reporting use case that are not included in the eICR FHIR IG. On the other hand, the Health Care Surveys IG does not overlap at all with the eICR FHIR IG from a content perspective but it can use the MedMorph Reference Architecture IG, which includes a broader architecture than the eICR FHIR IG that considers use cases where case reporting is not the main purpose. Therefore, the Health Care Surveys content IG would reference the MedMorph Reference Architecture IG and then add profiles and/or extensions, etc. needed for the Health Care Surveys use case that are not included in the MedMorph Reference Architecture IG (e.g., Health Care Surveys content specifications).

**Figure 2.1 – Relationship among Different IG Types and Basic FHIR Capabilities**

{::options parse_block_html="false" /}

<figure>
  <img height="600px" src="IGRelationship.png" alt="Figure 1: IG Types and Relationships" style="vertical-align:middle"/>
</figure>

{::options parse_block_html="true" /}

**NOTE to Reviewers:** 

Although the US PH Library does not exist yet, it is included to show the direction that we will be taking under the guidance of the HL7 Public Health WG. The US PH Library will harmonize similar profiles from multiple IGs being produced under the sponsorship of the HL7 Public Health WG. This will ensure consistency across implementation guides, reduce profile proliferation, and provide a starting point for future Content Implementation Guides.  

#### Relationship between MedMorph IG and US Core FHIR IG  

As shown in Figure 2.1 above, the MedMorph Reference Architecture IG is built on other basic FHIR capabilities and the US Core IG. The MedMorph Reference Architecture IG will use US CORE IG profiles as required for each use case. In addition to the profiles, the patient level FHIR APIs specified in US Core will be leveraged for all public health reporting use cases that can leverage the MedMorph Reference Architecture.

#### Relationship between MedMorph IG and FHIR Bulk Data Access IG
 
For research use cases, specifically where data about multiple patients need to be retrieved, MedMorph will use FHIR Bulk Data Access specified in the [BulkDataAccess IG](http://hl7.org/fhir/uv/bulkdata/index.html). The MedMorph Reference Architecture IG will also leverage Backend Services Authorization protocols specified in the [BulkDataAccess IG](http://hl7.org/fhir/uv/bulkdata/index.html) to secure system to system communication transactions in all MedMorph workflows.

#### Other Exiting Public Health IGs that could contribute to a common US PH Library
 
The Vital Records Common Profile Library is being balloted in Jan 2021. While it is focused around vital records there are overlapping profiles with eCR and Occupational Data for Health (ODH). This project was created through the recognition of overlapping birth defects and birth and fetal death FHIR profiles. However, to help harmonize overlapping profiles with other public health use cases such as MedMorph, the VR common library may be a seed for the establishment of the US PH library mentioned above in the MedMorph Reference Architecture IG. 

#### Relationship between MedMorph IG and Content Implementation Guides 

The MedMorph Reference Architecture IG does not prescribe content specific to any use case. Rather, it specifies the workflow processing, trigger mechanisms using PlanDefinition, ActivityDefinition, ValueSets, and APIs for message routing using Bundle, MessageHeader, etc. The Content Implementation Guide, such as the Health Care Surveys IG, would specify the profiles, search parameters, and operations for use case specific requirements but would reuse the MedMorph Reference Architecture IG to trigger events, submit data, process knowledge artifacts, manage workflows, etc. 

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

This guide is based on the [HL7 FHIR]({{site.data.fhir.path}}index.html) standard, as well as [US Core IG](https://www.hl7.org/fhir/us/core/index.html), [Bulk Data IG](http://hl7.org/fhir/uv/bulkdata/index.html), and [SMART on FHIR Backend Services Authorization](http://hl7.org/fhir/uv/bulkdata/authorization/index.html) specifications, which build additional capabilities on top of FHIR.  This reference architecture is intended to maximize the number of clinical systems that can conform to this guide as well as to allow for easy growth and extensibility of system capabilities in the future.

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
    <td><a href="{{site.data.fhir.path}}task.html">Task</a></td>
	<td>Used to create and execute tasks based on PlanDefinitions and ActivityDefinitions.</td>
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
    <td><a href="{{site.data.fhir.path}}practitioner.html">Practitioner</a></td>
	<td>Used to represent Practitioners.</td>
  </tr>
  <tr>
    <td><a href="{{site.data.fhir.path}}practitionerrole.html">PractitionerRole</a></td>
	<td>Used to represent Practitioner and association with the Organization.</td>
  </tr>
  <tr>
    <td><a href="{{site.data.fhir.path}}subscription.html">Subscription</a></td>
	<td>Used to create subscriptions based on trigger events.</td>
  </tr>
</table>

#### Electronic Case Reporting (eCR) FHIR IG

The MedMorph Reference Architecture implementation guide aligns with the eCR FHIR IG where profiles exist for the resources identified in the previous section. Implementers need to familiarize themselves with the eCR FHIR IG.

<table>
  <tr>
    <td><a href="http://hl7.org/fhir/us/ecr/">eCR FHIR IG STU 1 - FHIR R4 based IG</a></td>
  </tr>
</table>

#### US Core FHIR IG

The MedMorph Reference Architecture implementation guide also builds on the US Core Implementation Guide where profiles exist for the resources identified in the previous section. Implementers need to familiarize themselves with the US Core FHIR IG.

<table>
  <tr>
    <td><a href="{{site.data.fhir.uscoreR4}}/index.html">US Core 3.1.0 - FHIR R4 based IG</a></td>
  </tr>
</table>


#### Bulk Data Access IG
For research use cases, the MedMorph Reference Architecture implementation guide will use the Bulk Data Access Implementation Guide to retrieve population level information from EHRs, subject to applying appropriate authorities and policies.

<table>
  <tr>
    <td><a href="http://hl7.org/fhir/uv/bulkdata/index.html">Bulk Data Access IG - FHIR R4 based IG</a></td>
  </tr>
</table>


#### Backend Services Authorization
The MedMorph Reference Architecture implementation guide will use Backend Services Authorization to secure all system-to-system interactions among the various actors in the MedMorph Reference Architecture.

<table>
  <tr>
    <td><a href="http://hl7.org/fhir/uv/bulkdata/authorization/index.html">Backend Services Authorization  - FHIR R4 based IG</a></td>
  </tr>
</table>
