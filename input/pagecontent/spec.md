This section defines the specific requirements for systems wishing to conform to actors specified in this MedMorph Reference Architecture IG.  The specification focuses on the creation of the Knowledge Artifacts and their usage by the Backend Service App. It also describes the use of [SMART on FHIR Backend Services Authorization]({{site.data.fhir.smartapplaunch}}/backend-services.html) and provides guidance on privacy, security, and other implementation requirements.


### Context

#### Pre-reading
Before reading this formal specification, implementers should first be familiar with two other key portions of the specification:

* The [Use Cases](usecases.html) page provides the business context and general process flow enabled by the formal specification.

* The [Background](background.html) page provides information about the underlying specifications and indicates what portions of each should be reviewed in order to have the necessary foundation to understand the constraints and usage guidance described in this detailed specification.


#### Conventions
This implementation guide uses specific terminology to flag statements that have relevance for the evaluation of conformance with the guide:

* **SHALL** indicates requirements that must be met to be conformant with the specification.

* **SHOULD** indicates behaviors that are strongly recommended (and which may result in interoperability issues or sub-optimal behavior if not adhered to), but which do not, for this version of the specification, affect the determination of specification conformance.

* **MAY** describes optional behaviors that are free to consider but where the is no recommendation for or against adoption.


#### Claiming Conformance 

Actors and Systems asserting conformance to this implementation guide have to implement the requirements outlined in the corresponding capability statements. The following definition of MUST SUPPORT is to be used in the implementation of the requirements.

##### MUST SUPPORT Definition

* Systems SHALL be capable of populating data elements as specified by the profiles and data elements are returned using the specified APIs in the capability statement.
* Systems SHALL be capable of processing resource instances containing the MUST SUPPORT data elements without generating an error or causing the application to fail. 
* Systems SHOULD be capable of displaying the MUST SUPPORT data elements for human use or storing it for other purposes.
* In situations where information on a particular data element is not present and the reason for absence is unknown, Systems SHALL NOT include the data elements in the resource instance returned from executing the API requests.
* When accessing MedMorph data, Systems SHALL interpret missing data elements within resource instances returned from API requests as data not present.


#### Profiles
This specification makes significant use of [FHIR profiles]({{site.data.fhir.path}}profiling.html), search parameter definitions, and terminology artifacts to describe the content to be shared as part of MedMorph workflows. The implementation guide is based on [FHIR R4]({{site.data.fhir.path}}) and profiles are listed for each interaction.

The full set of profiles defined in this implementation guide can be found by following the links on the MedMorph [FHIR Artifacts](artifacts.html) page.


#### US Core
This IG also leverages the [US Core]({{site.data.fhir.uscoreR4}}/index.html) set of profiles defined by HL7 for sharing non-veterinary EMR individual health data in the U.S.  Where US Core profiles exist, this IG either leverages them directly or uses them as a base for any additional constraints needed to support the member attribution list use cases.  If no constraints are needed, this IG does not define any profiles.

Where US Core profiles do not yet exist (e.g., for PlanDefinition, Bundle), MedMorph profiles have been created.


#### SMART on FHIR Backend Services Authorization
This section outlines how the SMART on FHIR Backend Services Authorization will be used by the MedMorph Reference Architecture implementation guide. 

* The following actors comprise the System Actors: EHRs, Knowledge Artifact Repository, Backend Service App, Trust Service Provider, Trusted Third Party, PHA, and Research Organizations.

* System Actors (EHRs, Knowledge Artifact Repository, Backend Service App, Trust Service Provider, Trusted Third Party, PHA, and Research Organization) SHOULD advertise conformance to SMART Backend Services Authorization by hosting a Well-Known Uniform Resource Identifiers (URIs) as defined in the [SMART on FHIR IG Backend Services]({{site.data.fhir.smartapplaunch}}/backend-services.html) specification.

* When conforming to the SMART Backend Services Authorization, System Actors SHALL include token_endpoint, scopes_supported, token_endpoint_auth_methods_supported and token_endpoint_auth_signing_alg_values_supported as defined in the [SMART on FHIR IG Backend Services]({{site.data.fhir.smartapplaunch}}/backend-services.html) specification.

* When System Actors act as clients, they SHOULD share their JSON Web Key Set (JWKS) with the server System Actors using Uniform Resource Locators (URLs) as defined in the [SMART on FHIR IG Backend Services]({{site.data.fhir.smartapplaunch}}/backend-services.html) specification.

* Client System Actors SHALL obtain the access token as defined in the [SMART on FHIR Backend Services]({{site.data.fhir.smartapplaunch}}/backend-services.html) specification.

* Content Implementation Guides SHALL each specify the scopes of each respective use case and any additional constraints on the authentication and authorization flows using SMART on FHIR Backend Services.


#### System Actors, Requirements and Capability Statements

This implementation guide sets expectations for the following systems using workflow specific requirements and capability statements as shown below. Implementers are advised to review the workflow requirements and then review the capability statements before implementing the corresponding actor.


<table>
  <thead>
    <tr>
      <th>System Actor Name</th>
      <th> Requirements</th>
      <th>Capability Statement Link</th>
    </tr>
  </thead>
  <tr>
    <td>Knowledge Artifact Repository</td>
    <td><a href="provisioning.html">Provisioning Workflow Requirements</a></td>
    <td><a href="CapabilityStatement-medmorph-knowledge-artifact-repository.html">Knowledge Artifact Repository Capability Statement</a></td>
  </tr>
  <tr>
    <td/>
    <td/>
    <td/>
  </tr>
  <tr>
    <td>Backend Service App</td>
    <td>
    	   <a href="provisioning.html">Provisioning Workflow Requirements</a> <br/>
    	   <a href="subscription.html">Subscriptions and Notifications Requirements</a> <br/>
    	   <a href="trustservices.html">Trust Services Requirements</a> <br/>
    	   <a href="reportsubmission.html">Report Submission Requirements</a> <br/>
    	</td>
    <td><a href="CapabilityStatement-medmorph-backend-service-app.html">Backend Service App Capability Statement</a></td>
  </tr>
  <tr>
    <td/>
    <td/>
    <td/>
  </tr>
  <tr>
    <td>EHR</td>
    <td><a href="subscription.html">Subscriptions and Notifications Requirements</a></td>
    <td><a href="CapabilityStatement-medmorph-ehr.html">EHR Capability Statement</a></td>
  </tr>
  <tr>
    <td/>
    <td/>
    <td/>
  </tr>
  <tr>
    <td>Trust Service Provider</td>
    <td><a href="trustservices.html">Trust Services Requirements</a></td>
    <td><a href="CapabilityStatement-medmorph-trust-service-provider.html">Trust Service Provider Capability Statement</a></td>
  </tr>
  <tr>
    <td/>
    <td/>
    <td/>
  </tr>
  <tr>
    <td>Trusted Third Party</td>
    <td><a href="reportsubmission.html">Report Forwarding Requirements</a></td>
    <td><a href="CapabilityStatement-medmorph-trusted-third-party.html">Trusted Third Party Capability Statement</a></td>
  </tr>
  <tr>
    <td/>
    <td/>
    <td/>
  </tr>
  <tr>
    <td>Public Health Authority</td>    
    <td>
       <a href="provisioning.html">Provisioning Workflow Requirements</a> <br/>
       <a href="reportsubmission.html">Report Receiving Requirements</a>
    </td>
    <td><a href="CapabilityStatement-medmorph-public-health-authority.html">Public Health Authority Capability Statement</a></td>
  </tr>
  <tr>
    <td/>
    <td/>
    <td/>
  </tr>
  <tr>
    <td>Research Organization</td>
    <td>
       <a href="provisioning.html">Provisioning Workflow Requirements</a> <br/>
       <a href="reportsubmission.html">Report Receiving Requirements</a>
    </td>
    <td><a href="CapabilityStatement-medmorph-public-health-authority.html">Research Organization Capability Statement</a></td>
  </tr>
</table>





