This section of the implementation guide defines the specific conformance requirements for systems wishing to conform to actors specifed in this MedMorph reference architecture implementation guide.  The specification focuses on the creation of  the Knowledge Artifacts and their usage by the Backend Service App.  It also describes the use of [SMART on FHIR Backend Services Authorization](http://hl7.org/fhir/uv/bulkdata/authorization/index.html) and provides guidance on privacy, security and other implementation requirements.


### Context

#### Pre-reading
Before reading this formal specification, implementers should first familiarize themselves with two other key portions of the specification:

* The [Use Cases & Overview](usecases.html) page provides context for what this formal specification is trying to accomplish and will give a sense of both the business context and general process flow enabled by the formal specification below.

* The [Technical Background](background.html) page provides information about the underlying specifications and indicates what portions of them should be read and understood to have necessary foundation to understand the constraints and usage guidance described here.


#### Conventions
This implementation guide uses specific terminology to flag statements that have relevance for the evaluation of conformance with the guide:

* **SHALL** indicates requirements that must be met to be conformant with the specification.

* **SHOULD** indicates behaviors that are strongly recommended (and which may result in interoperability issues or sub-optimal behavior if not adhered to), but which do not, for this version of the specification, affect the determination of specification conformance.

* **MAY** describes optional behaviors that are free to consider but where the is no recommendation for or against adoption.


#### Systems and their Capability Statements

This implementation guide sets expectations for the following systems using capability statements as shown below:


<table>
  <thead>
    <tr>
      <th>System Name</th>
      <th>Capability Statement Link</th>
    </tr>
  </thead>
  <tr>
    <td>Knowledge Artifact Repository</td>
    <td><a href="{{site.data.fhir.path}}plandefinition.html">Knowledge Artifact Repository Capability Statement</a></td>
  </tr>
  <tr>
    <td>Backend Service App</td>
    <td><a href="{{site.data.fhir.path}}plandefinition.html">Backend Service App Capability Statement</a></td>
  </tr>
  <tr>
    <td>EHR</td>
    <td><a href="{{site.data.fhir.path}}plandefinition.html">EHR Capability Statement</a></td>
  </tr>
  <tr>
    <td>Trust Service Provider</td>
    <td><a href="{{site.data.fhir.path}}plandefinition.html">Trust Service Provider Capability Statement</a></td>
  </tr>
  <tr>
    <td>Trusted Third Party</td>
    <td><a href="{{site.data.fhir.path}}plandefinition.html">Trusted Third Party Capability Statement</a></td>
  </tr>
    <tr>
    <td>Public Health Authority</td>
    <td><a href="{{site.data.fhir.path}}plandefinition.html">Public Health Authority Capability Statement</a></td>
  </tr>
    <tr>
    <td>Research Organization</td>
    <td><a href="{{site.data.fhir.path}}plandefinition.html">Research Organization Capability Statement</a></td>
  </tr>
</table>


#### Claiming Conformance 

Actors and Systems asserting conformance to this implementation guide have to implement the requirements outlined in the corresponding capability statements. The following definition of MUST SUPPORT is to be used in the implementation of the requirements.

##### MUST SUPPORT Definition

* Systems SHALL be capable of populating data elements as specified by the profiles and are returned using the specified APIs in the capability statement.
* Systems SHALL be capable of processing resource instances containing the MUST SUPPPORT data elements without generating an error or causing the application to fail. In other words Systems SHOULD be capable of displaying the data elements for human use or storing it for other purposes.
* In situations where information on a particular data element is not present and the reason for absence is unknown, Systems SHALL NOT include the data elements in the resource instance returned from executing the API requests.
* When accessing MedMorph data, Systems SHALL interpret missing data elements within resource instances returned from API requests as data not present.


#### Profiles
This specification makes significant use of [FHIR profiles]({{site.data.fhir.path}}profiling.html), search parameter definitions and terminology artifacts to describe the content to be shared as part of MedMorph workflows. The implementation guide is based on FHIR [R4]({{site.data.fhir.path}}) and profiles are listed for each interaction.

The full set of profiles defined in this implementation guide can be found by following the links on the [FHIR Artifacts](artifacts.html) page.


#### US Core
This implementation guide also leverages the [US Core](http://hl7.org/fhir/us/core) set of profiles defined by HL7 for sharing non-veterinary EMR individual health data in the U.S.  Where US Core profiles exist, this Guide either leverages them directly or uses them as a base for any additional constraints needed to support the member attribution list use cases.  If no constraints are needed, this IG doesn't define any profiles.

Where US Core profiles do not yet exist (e.g. for PlanDefinition, TriggerDefinition), profiles have been created.


#### Bulk Data IG 
This section outlines how the Bulk Data IG will be leveraged by this implementation guide. 




#### SMART on FHIR Backend Services Authorization
This section outlines how the SMART on FHIR Backend Services Authorization guide will be used by this implementation guide. 


### MedMorph Knowledge Artifact requirements

The section outlines specific requirements that need to be followed in creating MedMorph Knowledge Artifacts.

#### Representing workflow events



#### Representing Terminologies



#### Representing Expression Logic 


#### Representing Research Queries 


#### Representing Security Requirements  


#### Security and Privacy considerations


### MedMorph Workflows and APIs

#### Provisioning Workflow - Creation of a Knowledge Artifact


**Precondition:**


**API: **


**Expected Result:**



