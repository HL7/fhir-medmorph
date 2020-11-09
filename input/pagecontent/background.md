### Implementation Guide Overview

The MedMorph Implementation Guide presented here is also known as the MedMorph Reference Architecture Implementation Guide. The Reference Architecture refers to a common framework (consisting of FHIR Resources, FHIR APIs, FHIR Operations, Security mechanisms) that will be leveraged by multiple public health and research use cases. In order to create a reference architecture that is scalable and flexible the MedMorph project has considered the following use cases 

* Hepatitis C Virus
* Cancer Reporting  
* Healthcare Surveys

These use cases have been used to create the data exchange abstract model and workflows from which the reference architecture is derived. This is further outlined in <a href="usecases.html">Use Cases and Workflows</a>.

Based on the above use cases, the reference architecture IG aims to minimize the burden on both the senders and receivers of data by providing a common method for obtaining data for research and public health for multiple conditions or uses. The CDC has established a Technical Expert Panel (TEP) to inform and guide the development of the technical approach and the reference architecture implementation guide.

### Implementation Guides Types and their Relationships 

The MedMorph project vision is to create this Reference Architecture Implementation Guide which is common to multiple public health and research use cases. However for each use case there will be specific data, workflow requirements and operational requirements that need to be considered for implementation. These use case specific content will be specified in other Implementation Guides called as **Content Implementation Guides**. It is expected that there will be one Content Implementation Guide per use case that needs to be operationalized. 

#### Relationship between MedMorph IG, Electronic Case Reporting (eCR) FHIR IG, and US Public Health (PH) Library 
The eCR FHIR IG is currently published. The MedMorph Architecture IG and a newer version of eCR FHIR IG are being created for the January 2020 HL7 ballot cycle. However the US Public Health (PH) Library currently does not exist. The US PH Library will be created eventually to establish a baseline of common artifacts that will be used by multiple public health implementation guides including MedMorph, [Electronic Case Reporting (eCR) FHIR IG](http://hl7.org/fhir/us/ecr/),  and the other content implementation guides shown below. The initial content for the US PH Library will be derived from MedMorph and eCR FHIR IG. For the January 2020 HL7 ballot both MedMorph and eCR FHIR IGs will create artifacts that are aligned so that they can be moved to the US PH Library eventually. All artifacts which are candidates for being promoted to the US PH Library will use the words "us" or "US" and "ph" or "PH" as part of the profile definition, name and title elements. The relationships between the three IGs are as shown in Figure 1 below.



{::options parse_block_html="false" /}

<figure>
  <img height="600px" src="IGRelationship.png" alt="Figure 1: IG Types and Relationships" style="vertical-align:middle"/>
</figure>

{::options parse_block_html="true" /}

**NOTE to Reviewers:** 

Although the US PH Library does not exist, it is being shown here as the direction that we will be taking under the guidance of the Public Health WG. The US PH Library will harmonize similar profiles from multiple IGs being produced under the sponsorship Public Health WG. This will ensure consistency across implementation guides, reduce profile proliferation and provide a starting point for future content implementation guides.  

#### Relationship between MedMorph IG and US Core FHIR IG  

As shown in the above Figure, the MedMorph Reference Architecture IG is built on other basic FHIR capabilities and the US Core IG.  From the US Core IG, MedMorph will use profiles as required for each use case. In addition to the profiles, For all public health reporting use cases being considered for MedMorph, the patient level FHIR APIs specified in US Core will be leveraged extensively by MedMorph and other content implementation guides.

#### Relationship between MedMorph IG and FHIR Bulk Data Access
 
For research use cases, specifically where data about multiple patients need to be retrieved, MedMorph will use FHIR Bulk Data Access specified in the BulkDataAccess IG. MedMorph IG will also leverage Backend Services Authorization protocols specifed in the BulkDataAccess IG to secure system to system communication transactions.

#### Relationship between MedMorph IG and Content Implementation Guides 

The MedMorph IG is an architecture IG and does not prescribe content specific to any use case. Rather it specifies the  the workflow processing, trigger mechanisms using PlanDefinition, ActivityDefinition, ValueSets etc, and  APIs for message routing using Bundle, MessageHeader etc. The Content Implementation Guide such as Health Care Survey IG would specify the profiles, search parameters and operations for use case specific requirements but would reuse the MedMorph IG to trigger events, submit data, process knowledge artifacts and manage workflows etc. 

### Guiding Principles 

The following are the guiding principles for the MedMorph architecture.

* The reference architecture must try to reduce provider burden while making EHR data available to research and public health.
* The reference architecture must align with existing standards (HL7 FHIR, CDA ) and regulations (ONC 2015 Edition, 2015 Edition Cures Update, Trusted Exchange Framework and Common Agreement (TEFCA), etc.) while improving the timeliness and completeness of data received by public health and research.
* Promote configurability of implementations to allow for customizations in workflows. 

### IG In-Scope Requirements
The following requirements are in-scope for the IG based on the use cases

* Define the API mechanisms, Inputs and Outputs used to access and exchange data 
* Define the mechanisms that will be used to trigger the workflows 
* Define the provisioning mechanisms that could be used to automate the triggering and reporting of data 
* Define trust services such as (pseudonymization, anonymization, de-identification, etc) that will be needed for certain use cases

### IG Out-of-Scope 
The following aspects are out-of-scope for the IG based on the use cases

* Enabling Claims data access to research and public health
* Changes to the data capture screens in the EHR and/or changes to clinical workflows are not prescribed as part of the IG.Providers may use their choice of apps/screens/systems to enter the data into the EHR independent of the IG.
* Any data not accessible by FHIR API.
* Clinical care setting policies such as the collection of consent for data sharing


### Underlying Specifications

This guide is based on the [HL7 FHIR]({{site.data.fhir.path}}index.html) standard, as well as [US Core IG](https://www.hl7.org/fhir/us/core/index.html), [Bulk Data IG](http://hl7.org/fhir/uv/bulkdata/index.html) and [SMART on FHIR Backend Services Authorization](http://hl7.org/fhir/uv/bulkdata/authorization/index.html) specifications which build additional capabilities on top of FHIR.  This architecture is intended to maximize the number of clinical systems that conform to this guide as well as to allow for easy growth and extensibility of system capabilities in the future.

Implementers of this specification therefore need to understand some basic information about the above underlying specifications.


#### FHIR

This implementation guide uses terminology, notations and design principles that are
specific to FHIR.  Before reading this implementation guide, it's important to be familiar with some of the basic principles of FHIR as well
as general guidance on how to read FHIR specifications.  Readers who are unfamiliar with FHIR are encouraged to read (or at least skim) the following
prior to reading the rest of this implementation guide.

* [FHIR overview]({{site.data.fhir.path}}overview.html)
* [Developer's introduction]({{site.data.fhir.path}}overview-dev.html) (or [Clinical introduction]({{site.data.fhir.path}}overview-clinical.html))
* [FHIR data types]({{site.data.fhir.path}}datatypes.html)
* [Using codes]({{site.data.fhir.path}}terminologies.html)
* [References between resources]({{site.data.fhir.path}}references.html)
* [How to read resource & profile definitions]({{site.data.fhir.path}}formats.html)
* [Base resource]({{site.data.fhir.path}}resource.html)

This implementation guide supports [FHIR R4]({{site.data.fhir.path}}index.html) versions of the FHIR standard. FHIR R4 is just recently published and the goal is to ensure the implementation guide is aligned with the current direction of the FHIR standard. Initial implementations will focus on FHIR R4.

#### FHIR Resources used for MedMorph Reference Architecture

This section identifies the specific FHIR Resources that will be used in the MedMorph IG and the purpose of these resources.
Implementers should familiarize themselves with the FHIR resources and their purposes.

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
    <td><a href="{{site.data.fhir.path}}activitydefinition.html">ActivityDefinition</a></td>
	<td>Used to define the activities that are part of the Plan Definition.</td>
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
  <tr>
    <td><a href="{{site.data.fhir.path}}provenance.html">Provenance</a></td>
	<td>Used to capture provenance information as part of workflows.</td>
  </tr>
    <tr>
    <td><a href="{{site.data.fhir.path}}documentreference.html">DocumentReference</a></td>
	<td>Used to wrap document payloads that need to be submitted.</td>
  </tr>
</table>

#### Electronic Case Reporting eCR FHIR IG

This implementation guide algins with the eCR FHIR IG where profiles exist for the resources identified in the previous section and implementers need to familiarize themselves with the eCR FHIR IG.

<table>
  <tr>
    <td><a href="http://hl7.org/fhir/us/ecr/">eCR FHIR IG STU 1 - FHIR R4 based IG</a></td>
  </tr>
</table>

#### US Core FHIR IG

This implementation guide also builds on the US Core Implementation Guide where profiles exist for the resources identified in the previous section and implementers need to familiarize themselves with the US Core IG.

<table>
  <tr>
    <td><a href="{{site.data.fhir.uscoreR4}}/index.html">US Core 3.1.0 - FHIR R4 based IG</a></td>
  </tr>
</table>


#### Bulk Data Access
For research use cases, Bulk Data Access implementation guide will be used to retrieve population level information from EHRs subject to appropriate authorities and policies.

<table>
  <tr>
    <td><a href="http://hl7.org/fhir/uv/bulkdata/index.html">Bulk Data Access IG - FHIR R4 based IG</a></td>
  </tr>
</table>


#### Backend Services Authorization
The Backend Services Authorization is used to secure all system to system interactions between the various actors that are part of the MedMorph Architecture IG.

<table>
  <tr>
    <td><a href="http://hl7.org/fhir/uv/bulkdata/authorization/index.html">Backend Services Authorization  - FHIR R4 based IG</a></td>
  </tr>
</table>
