This section defines the specific requirements for systems wishing to conform to actors specified in this MedMorph Reference Architecture (RA)  Implementation Guide (IG). The specification focuses on the creation of the Knowledge Artifacts and their usage by the Health Data Exchange App (HDEA), MedMorph’s backend services app. It also describes the use of [SMART on FHIR Backend Services Authorization]({{site.data.fhir.smartapplaunch}}/backend-services.html) and provides guidance on privacy, security, and other implementation requirements.


### Context

#### Pre-reading
Before reading this formal specification, implementers should first be familiar with two other key portions of the IG:

* The [Use Cases](usecases.html) page provides the business context and general process flow enabled by this  specification.

* The [Implementation Guide overview](background.html) page provides information about the underlying specifications and indicates what portions of each should be reviewed in order to have the necessary foundation to understand the constraints and usage guidance described in this specification.


#### Conventions
The IG uses specific terminology to flag statements that have relevance for the evaluation of conformance with this specification:

* **SHALL** indicates requirements that must be met to be conformant with the specification.

* **SHOULD** indicates behaviors that are strongly recommended (and which may result in interoperability issues or sub-optimal behavior if not adhered to), but which do not, for this version of the specification, affect the determination of specification conformance.

* **MAY** describes optional behaviors that are free to consider but where the is no recommendation for or against adoption.


#### Claiming Conformance 

Actors and Systems asserting conformance to this implementation guide must implement the requirements outlined in the corresponding capability statements. 

**NOTE**: This MedMorph RA IG is a framework IG and hence actors and systems will claim conformance to this MedMorph IG in combination with Content IGs. The Content IGs specify detailed requirements including portions of this IG that will be used for the specific use case implementation. However for Knowledge Artifact Repository, TTP and Trust Service Provider actors Content IGs MAY reuse the corresponding MedMorph RA IG capability statements completely.   

The following definition of MUST SUPPORT is to be used in the implementation of the requirements.

##### MUST SUPPORT Definition

* Systems SHALL be capable of populating data elements as specified by the profiles and data elements are returned using the specified APIs in the capability statement.
* Systems SHALL be capable of processing resource instances containing the MUST SUPPORT data elements without generating an error or causing the application to fail. 
* Systems SHOULD be capable of displaying the MUST SUPPORT data elements for human use or storing it for other purposes.
* In situations where information on a particular data element is not present and the reason for absence is unknown, Systems SHALL NOT include the data elements in the resource instance returned from executing the API requests.
* When accessing MedMorph data, Systems SHALL interpret missing data elements within resource instances returned from API requests as data not present.


#### Profiles
This specification makes use of [FHIR profiles]({{site.data.fhir.path}}profiling.html), search parameter definitions, and terminology artifacts to describe the content to be shared as part of MedMorph workflows. The IG is based on [FHIR R4]({{site.data.fhir.path}}) and profiles are used as part of each interaction.

The full set of profiles defined in this implementation guide can be found on the [FHIR Artifacts](artifacts.html) page.


#### Authentication and Authorization Requirements

This section outlines how the SMART on FHIR Backend Services Authorization will be used by the MedMorph RA IG. The requirements in this section is applicable to Data Source, Knowledge Artifact Repository, Health Data Exchange App (HDEA), Trusted Third Party (TTP), and Data Receiver actors. These actors will be designated as MedMorph System Actors in this section.

* MedMorph System Actors **SHOULD** advertise conformance to SMART Backend Services Authorization IG by hosting a Well-Known Uniform Resource Identifiers (URIs) as defined in the [SMART on FHIR - Backend Services]({{site.data.fhir.smartapplaunch}}/backend-services.html) specification.

* When conforming to the SMART Backend Services Authorization IG, MedMorph System Actors **SHALL** include token_endpoint, scopes_supported, token_endpoint_auth_methods_supported and token_endpoint_auth_signing_alg_values_supported as defined in the [SMART on FHIR - Backend Services]({{site.data.fhir.smartapplaunch}}/backend-services.html) specification.

* When MedMorph System Actors act as clients, they **SHOULD** share their JSON Web Key Set (JWKS) with the server System Actors using Uniform Resource Locators (URLs) as defined in the [SMART on FHIR - Backend Services]({{site.data.fhir.smartapplaunch}}/backend-services.html) specification.

* All MedMorph System Actors **SHOULD** support the system/*.read scope to faciliate the MedMorph RA IG workflows.

* Client System Actors **SHALL** obtain the access token as defined in the [SMART on FHIR - Backend Services]({{site.data.fhir.smartapplaunch}}/backend-services.html) specification.

* Server System Actors **SHALL** process and validate incoming requests for resources as defined in the [SMART on FHIR - Backend Services]({{site.data.fhir.smartapplaunch}}/backend-services.html) specification.

* Content IGs SHALL specify granular scopes required for authorization based on use case requirements using [SMART on FHIR - Backend Services]({{site.data.fhir.smartapplaunch}}/backend-services.html)


#### MedMorph Actors, Requirements and Capability Statements

This MedMorph RA IG defines common requirements and generalized capability statements using the MedMorph workflows. Implementers are advised to review the requirements below along with Content IG requirements before implementing the corresponding actor.

**NOTE**: Content IGs **SHALL** define conformance requirements applicable to each actor based on the use case and **SHOULD** reuse the MedMorph RA IG common requirements from below.     


<table>
  <thead>
    <tr>
      <th> Actor Name</th>
      <th> Requirements for MedMorph Workflow</th>
      <th> Capability Statement</th>
    </tr>
  </thead>
  <tr>
    <td>Knowledge Artifact Repository</td>
    <td><a href="provisioning.html">Provisioning</a></td>
    <td><a href="CapabilityStatement-medmorph-knowledge-artifact-repository.html">Knowledge Artifact Repository Capability Statement Example</a></td>
  </tr>
  <tr>
    <td/>
    <td/>
    <td/>
  </tr>
  <tr>
    <td>Health Data Exchange App (HDEA)</td>
    <td>
    	   <a href="provisioning.html">Provisioning Workflow</a> <br/>
    	   <a href="subscription.html">Notifications</a> <br/>
    	   <a href="trustservices.html">Report Creation</a> <br/>
    	   <a href="reportsubmission.html">Data Submission and Response Receiving</a> <br/>
    	</td>
    <td><a href="CapabilityStatement-medmorph-healthdata-exchange-app.html">HDEA Capability Statement Example</a></td>
  </tr>
  <tr>
    <td/>
    <td/>
    <td/>
  </tr>
  <tr>
    <td>Data Source</td>
    <td><a href="subscription.html">Notifications</a></td>
    <td><a href="CapabilityStatement-medmorph-data-receiver.html">Data Source Capability Statement Example</a></td>
  </tr>
  <tr>
    <td/>
    <td/>
    <td/>
  </tr>
  <tr>
    <td>Trust Service Provider</td>
    <td><a href="trustservices.html">Report Creation</a></td>
    <td><a href="CapabilityStatement-medmorph-trust-service-provider.html">Trust Service Provider Capability Statement Example</a></td>
  </tr>
  <tr>
    <td/>
    <td/>
    <td/>
  </tr>
  <tr>
    <td>Trusted Third Party</td>
    <td><a href="reportsubmission.html">Data Submission and Response Receiving</a></td>
    <td><a href="CapabilityStatement-medmorph-trusted-third-party.html">Trusted Third Party Capability Statement Example</a></td>
  </tr>
  <tr>
    <td/>
    <td/>
    <td/>
  </tr>
  <tr>
    <td>Data Receiver - PHA/RO</td>    
    <td>
       <a href="provisioning.html">Provisioning</a> <br/>
       <a href="reportsubmission.html">Data Submission and Response Receiving</a>
    </td>
    <td><a href="CapabilityStatement-medmorph-data-receiver.html">Data Receiver Capability Statement Example</a></td>
  </tr>
  <tr>
    <td/>
    <td/>
    <td/>
  </tr>
</table>





