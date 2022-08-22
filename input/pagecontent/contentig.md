This section provides guidance to content implementation guide authors on how to leverage the MedMorph Reference Architecture (RA) and create a Content Implementation Guide (IG).

***NOTE: This section is only included in this IG as guidance to inform content IG authors.***

### MedMorph Content IG overview

MedMorph content IGs are created to address reporting requirements for specific use cases and/or reporting programs. Each content IG will focus on a specific use case such as Central Cancer Registry Reporting, Health Care Surveys, or Research Data Exchange. Each content IG is expected to leverage the MedMorph RA IG during development. The content IG should be tested and piloted before being finalized for publication.

For a more detailed understanding of the content IG and relationships with other IGs, refer to the section
 [Implementation Guide Types and Relationships](background.html#ig-types-and-their-relationships).

The next few paragraphs outline the various sections and the creation process to follow when authoring a content IG.

### Example Content IG Sections 

The table below identifies the necessary sections of a content IG. In addition to the sections outlined below, other sections can be added as needed.

<table>
  <thead>
    <tr>
      <th>Section Name</th>
      <th>Purpose</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>Introduction</td>
	<td>This section provide an introduction and background on the specific use case for which the Content IG is being created.</td>
  </tr>
  <tr>
    <td>Use Cases and Workflows</td>
	<td>This section  describes the use cases, user stories, and workflows associated with the use case and/or the reporting program. This section should include identifying the MedMorph Reference Architecture  actors and systems  needed for the workflow. If any new actors or systems are identified for the use case, they must be defined in this section along with their roles and interactions with the MedMorph Reference Architecture actors and systems. Any requirements that depend on previous state, current state, and cached state should be captured as part of the use case to inform the processing and capabilities required for each actor.</td>
  </tr>
  <tr>
    <td>Data Source (e.g EHR) Requirements</td>
	<td>This section outlines the specific data elements necessary for the use case that must also be supported by Data Sources. These data elements include subscription events, notifications, and mapping to FHIR Resources. This section should also define the necessary FHIR APIs that the Data Sources must support and the security requirements for interaction with the Data Source if different from the MedMorph RA IG.</td>
  </tr>
  <tr>
    <td>Data Receiver Requirements</td>
	<td>This section outlines the specific requirements for submission to the data receiver (e.g., PHAs, ROs, TTPs). This section should delineate the APIs, processing of the data, content necessary for submission, and support for synchronous vs. asynchronous responses. Additionally, this section should include the expected responses from the data receiver and how these responses should be consumed by the healthcare organization.  Lastly, the security requirements for interaction with the data receiver must be specified in this section.</td>
  </tr>
  <tr>
    <td>Knowledge Artifact Requirements</td>
	<td>This section details the requirements for the use case-specific Knowledge Artifacts. This will define the trigger events, actions that must be executed, value sets, and code systems necessary for processing.</td>
  </tr>
  <tr>
    <td>CodeSystems and ValueSets</td>
	<td>This section outlines the CodeSystems and ValueSets required for the use case.</td>
  </tr>
  <tr>
    <td>FHIR Artifacts</td>
	<td>This section provides the following FHIR Artifacts for the Content IG. 
	<ul>
	<li>Profiles</li>
	<li>Capability Statements</li>
	<li>Code Systems</li>
	<li>Value Sets</li>
	<li>Examples</li>
	</ul>
	</td>
  </tr>
  </tbody>
</table>

### Capturing the Use Case and Workflows in a Content IG

In this section, the authors must reference the actors and systems specified by the [MedMorph RA IG](usecases.html) that are required for the use case. If the use case requires a new actor or system, then the new actor or system should be added and defined in this section of the content IG.  If a new actor/system is needed, the interactions between the various actors and systems should be documented as well as the relationship of those actor(s)/system(s) to the MedMorph actors/systems.
 
The section should contain the following sub-sections:

* **Business Objectives** being met by the Content IG
* **User Story(ies)** the Content IG is addressing.
* Identification and/or definition of the Actors and Systems based on the user story(ies)
* Documentation of the Main Flow and significant Exception Flows of the use case using the identified actors and systems. (e.g., a business process model [BPM] type of diagram).
* Identification of any interactions between actors and systems that are not part of the MedMorph RA IG.


### Capturing Data Source (e.g EHR) Requirements in a Content IG

* •	In this section, the data requirements for the use case should be identified in detail. The requirements can be captured in a spreadsheet and linked from this section.

For example, the level of detail that should be documented in this section of the Central Cancer Registry Reporting content IG includes requirements the cancer reporting use case needs to capture Condition data, including but not limited to the relevant ICD10/SNOMED codes, the status of the condition (e.g., active, resolved, readmission), onset data of the condition, encounter where the diagnosis was performed, etc.  Additionally, the following topics are necessary to fully specify this section of the content IG:

* The named event requirements for the use cases have to be selected from the [Named Event Value Set](ValueSet-us-ph-triggerdefinition-namedevent.html). If the events defined are not sufficient for the use case, then the authors should contact the HL7 PH WG for guidance. 

* The Subscription Topics that will be used based on the named events should be defined per the Subscription Backport IG.

* The specific APIs and Operations that need to be supported by the Data Sources should be identified and documented in the Capability Statement and linked from this section.

* The security mechanisms to be used to interact with the Data Source (e.g., SMART on FHIR Backend Services Authorization or other mechanisms) should be identified in this section.


### Capturing Data Receiver Requirements in a Content IG

The data requirements relevant to the data receiver (e.g., PHA) should be documented in this section. Typically, this is a subset of all the data requirements identified in the Data Source Data Requirements section
The data submission requirements must be documented in this section. This includes any timing constraints on when the data must be submitted any periodic data submission requirements, and the content of the Report that is generated and sent to the data receiver.

* The Data Receiver responses to the submission must be outlined. This includes documenting the  main workflow, success paths, exception paths, and the data that will be included.

* Additionally, this section should identify if the interactions are synchronous and/or asynchronous and document any routing and validation requirements.

* The specific APIs and Operations requiring support from the Data Receiver should be identified and documented in the Capability Statement and linked from this section.

* The security mechanisms needed to interact with the Data Receiver (e.g., SMART on FHIR Backend Services Authorization, or other mechanisms) should be documented in this section.


### Capturing the Knowledge Artifact Requirements in a Content IG

* Refer to [Provisioning workflow specifications](provisioning.html) on how to create a Knowledge Artifact. 

### Dealing with CodeSystems and ValueSets

CodeSystems and ValueSets are used extensively to identify the relevant clinical content to submit to the Data Receiver (e.g., PHA). 

These CodeSystems and ValueSets can be:

* Defined by the HL7 Terminology WG

* Defined by other IGs such as US Core, US Public Health Profiles, eCR FHIR IG, etc.

* Defined by previous initiatives and hosted in the Public Health Information Network Vocabulary Access and Distribution System (PHINVADs) or the Value Set Authority Center (VSAC).

* Defined as new content in the content IG if no reusable content exists. 
 
For each CodeSystem and ValueSet used in the IG, the source must be identified. Any decisions on whether the value set or code system is hosted by an external agency such PHINVADS or VSAC is left to the content IG author. Any ValueSets and CodeSystems defined in the IG that are commonly applicable to multiple IGs should be pushed to HL7 Terminology WG for consideration.

### Creation of Profiles

Ideally, creating a content IG that can leverage existing profiles from other IGs would reduce implementation burden on both Data Sources (e.g., EHRs) and Data Receivers. To achieve this goal, the following  is recommended:

* From the data requirements, identify if there is a specific US Core profile that can be used.

* If there is not a specific US Core profile, then refer to the US Public Health (PH) Library to determine if there is a profile that can be used.

* If no profiles are found in either US Core or the US PH Library, refer to other PH initiatives (e.g., eCR, Vital Records Birth and Fetal Death Reporting) to identify if there is a profile that can be leveraged.

* If no existing profiles completely satisfy the use case need, including if an existing profile partially supports the need, a new profile can be created and documented in the content IG. The created profile could be a completely new profile, a modified profile, or a profile of an existing profile.

### Content IG Examples

The content IG Examples section should have an example for each data element that is part of the IG. These examples should be validated for compliance to their specific profile using the FHIR Validator. Ideally, examples should be generated from the Connectathon testing activities.

 