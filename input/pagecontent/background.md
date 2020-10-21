### Implementation Guide Overview

The MedMorph Implementation Guide presented here is also known as the MedMorph Reference Architecture Implementation Guide. The Reference Architecture refers to a common framework (consisting of FHIR Resources, FHIR APIs, FHIR Operations, Security mechanisms) that will be leveraged by multiple public health and research use cases. In order to create a reference architecture that is scalable and flexible the MedMorph project has considered the following use cases 

* Hepatitis C Virus
* Cancer Reporting  
* Healthcare Surveys

These use cases have been used to create the data exchange abstract model and workflows from which the reference architecture is derived. This is further outlined in <a href="usecases.html">Use Cases and Workflows</a>.

Based on the above use cases, the reference architecture IG aims to minimize the burden on both the senders and receivers of data by providing a common method for obtaining data for research and public health for multiple conditions or uses. The CDC has established a Technical Expert Panel (TEP) to inform and guide the development of the technical approach and the reference architecture implementation guide.

### Implementation Guides Types and their Relationships 

The MedMorph project vision is to create this Reference Architecture Implementation Guide which is common to multiple public health and research use cases. However for each use case there will be specific data, workflow requirements and operational requirements that need to be considered for implementation. These use case specific content will be specified in other Implementation Guides called as **Content Implementation Guides**. It is expected that there will be one Content Implementation Guide per use case that needs to be operationalized. The relationship between Reference Architecture IG and the Content Implementation Guide is as shown in Figure 1 below.


{::options parse_block_html="false" /}

<figure>
  <img height="600px" src="IGRelationship.png" alt="Figure 1: IG Types and Relationships" style="vertical-align:middle"/>
</figure>

{::options parse_block_html="true" /}

As shown in the above Figure, the MedMorph Reference Architecture IG is built on other foundational IGs such as US Core and Bulk Data IG which provide APIs for patient level data access and population level data access respectively. The MedMorph IG is used in turn by specific use cases. For example the Hepatitis C Virus IG would specify the profiles, search parameters and operations for use case specific requirements but would reuse the MedMorph IG to trigger events, submit data, process knowledge artifacts etc. 

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

#### US Core IG

This implementation guide also builds on the US Core Implementation Guide where profiles exist for the resources identified in the previous section and implementers need to familiarize themselves with the US Core IG.

<table>
  <tr>
    <td><a href="{{site.data.fhir.uscoreR4}}/index.html">US Core 3.1.0 - FHIR R4 based IG</a></td>
  </tr>
</table>


#### Bulk Data IG
Bulk Data IG will be used to retrieve population level information from EHRs subject to appropriate authorities and policies.

<table>
  <tr>
    <td><a href="http://hl7.org/fhir/uv/bulkdata/index.html">Bulk Data IG - FHIR R4 based IG</a></td>
  </tr>
</table>


#### Backend Services Authorization
The Backend Services Authorization is used to secure all the system interactions between the various actors that are part of the MedMorph Reference Architecture.

<table>
  <tr>
    <td><a href="http://hl7.org/fhir/uv/bulkdata/authorization/index.html">Backend Services Authorization  - FHIR R4 based IG</a></td>
  </tr>
</table>
