This section provides guidance to content implementation guide authors on how to leverage the medmorph reference architecture and create a Content Implementation Guide (IG). 

***NOTE: This section is non-normative and is only included as guidance to inform content ig authors.***

### MedMorph Content IG overview

MedMorph Content Implementation Guides (IGs) are created to address specific reporting requirements for specific use cases and/or reporting programs. Each Content IG will focus on a specific use case such as Cancer reporting, Healthcare Survey reporting, Hepatitis C reporting. Each Content IG is expected to leverage the MedMorph reference architecture during the development and experiment with the reference implementation before finalizing the content implementation guide (ig) for publishing. 

For a deeper understanding of the Content IG and its relationships with other implementation guides, refer to the section [Implementation Guide Types and Relationships](background.html#implementation-guides-types-and-their-relationships).

The next few paragraphs outline the various sections that are expected to be in a Content IG and the creation process for the sections.

### Content IG Sections 

The following sections are expected to be created in a Content IG. In addition to the sections outlined below other sections can be added as needed. 

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
	<td>This section is to provide introduction and background on the specific use case for which the content ig is being created.</td>
  </tr>
  <tr>
    <td>Use Cases and Workflows</td>
	<td>This section is to document the use cases, user stories and workflows associated with the use case and/or the reporting program. This should include the identification of the MedMorph Reference Architecture Actors and Systems taht will be used in the workflow. If any new actors or systems are to be used for the use case, they need to be defined in this section along with their roles and interactions with the MedMorph Reference Architecture actors and systems.</td>
  </tr>
  <tr>
    <td>EHR Requirements</td>
	<td>This section outlines the specific data elements that are required to be supported by the EHRs. These include the subscription events, notifications and the specific FHIR Resources and data elements required for the use case. The section should also define the specific FHIR APIs that need to be supported by the EHR for the specific use case. Lastly the security requirements for interaction with the EHR must be specified in this section.</td>
  </tr>
  <tr>
    <td>PHA Requirements</td>
	<td>This section outlines the specific requirements for submission to the PHAs. This section should outline the APIs, processing of the data, content to be submitted, support for synchronous vs asynchronous responses. The section should also outline the responses to be expected from the PHA and how these responses should be consumed by the healthcare organization.Lastly the security requirements for interaction with the PHA must be specified in this section.</td>
  </tr>
  <tr>
    <td>Knowledge Artifact Requirements</td>
	<td>This section outlines the specific requirements for the use case specific Knowledge Artifact. This will define the trigger events, actions that need to be executed, value sets and code systems that need to be used as part of the processing.</td>
  </tr>
  <tr>
    <td>CodeSystems and ValueSets</td>
	<td>This section outlines the CodeSystems and ValueSets to be used by the Content IG.</td>
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

In this section, the authors have to specifically identify the actors and systems that are going to be reused from the [MedMorph Reference Architecture](usecases.html). If the Content IG requires a new actor or a system that needs to be part of the use case and workflow, then the new actor should be added and defined in this section. The section should then outline the interactions between the various actors and systems if they are different than the MedMorph Reference Architecture (RA) . The section should contain the following sub-sections

* Business Objectives being met by the Content IG
* User Story examples where the content ig is going to be used.
* Identification and/or definition of the Actors and Systems based on the user stories.
* Documentation of the Main Flow and significant Exception Flows of the use case using the actors and systems using a BPM type of diagram.
* Identification of any interactions between actors and systems that are not part of the MedMorph RA.

### Capturing EHR Requirements in a Content IG

* In this section, the data requirements for the use case should be identified in detail. The requirements can be captured in a spreadsheet and linked into the section. The data requirements should be tagged as being applicable to the EHR where appropriate.

For e.g, In a cancer reporting content ig, the cancer reporting use case may need to capture Condition data including the Condition using ICD10/SNOMED codes, the status of the condition (active/resolved), onset data of the condition, encounter where the diagnosis was performed etc. 

* The named event requirements for the use cases have to be selected from the [MedMorph RA Named Event Value Set](ValueSet-us-ph-triggerdefinition-namedevent.html). If the events defined are not sufficient for the use case, then the authors should contact the PHWG for guidance. 

* The Subscription Topics that will be used based on the named events should be defined per the Subscription Backport IG.

* The specific APIs and Operations that need to be supported by the EHRs should be identified and documented in the Capability Statement and linked from this section.

* The security mechanisms to be used to interact with the EHR (for e.g SMART on FHIR App Launch, SMART on FHIR Backend Services Authorization or other mechanisms) should be identified in this section.


### Capturing PHA Requirements in a Content IG

* The data requirements that are relevant to the PHA should be documented in this section. This is normally a subset of all the data requirements identified in the EHR Data Requirements section.

* The submission requirements have to be documented in this section. This should include any timing constraints on when the data has to be submitted, any periodic data submission requirements. The content of the Report that is generated and sent to the PHA.

* The PHA responses to the submission have to be outlined. This include dealing with the main flow and success paths and the exception paths and the data that will be included for either scenario.

* The section also should identify if the interactions are going to synchronous and/or asynchronous. Any routing and validation requirements should be documented in this sections.

* The specific APIs and Operations that need to be supported by the PHAs should be identified and documented in the Capability Statement and linked from this section.

* The security mechanisms to be used to interact with the PHA (for e.g SMART on FHIR App Launch, SMART on FHIR Backend Services Authorization or other mechanisms) should be identified in this section.

### Capturing the Knowledge Artifact Requirements in a Content IG

* Refer to [provisioning](provisioning.html) requirements on how to create a Knowledge Artifact. 

### Dealing with CodeSystems and ValueSets

CodeSystems and ValueSets will be used extensively to identify the relevant clinical content and submitting the data to the PHA. These CodeSystems and ValueSets can be 

* Defined already by the HL7 Terminology WG
* Defined by Other IGs such as US Core, US Public Health Profiles, eCR FHIR IG etc.
* Defined by previous initiatives and may be hosted in PHINVADs or VSAC.
* Defined newly by the Content IG itself.

For each CodeSystem and ValueSet that is used in the IG, the source has to be identified to be one of the above. Any decisions on whether the value set or code system is hosted by an extenal agency such PHINVADS or VSAC is left to the Content IG author. 
Any ValueSets and CodeSystems defined in the IG that are commonly applicable to multiple IGs should be pushed to HL7 Terminology WG for consideration. 

### Creation of Profiles

Ideally, creating a Content IG that can leverage existing profiles from other IGs would reduce implementation burden on both EHRs and PHAs. In order to achieve this goal the following recommendations are made 

* From the data requirements, identify if there is a specific US Core profile that can be used.
* If there is not a specific US Core profile to be used, then refer to the US PH Library to determine if there is a profile that can be used.
* If none are found, refer to other PH initiatives such as eCR, Birth and Death Reporting to identify if there is a profile that can be leveraged.
* If none are found or if there are profiles that can be used partially, then create a profile in the Content IG that fits the need. The created profile could be a new profile or a profile of an existing profile. 

### Content IG Examples

The section should have an example for each data element that is part of the IG.
These examples should have been validated for compliance to their specific profile using the FHIR Validator.
Ideally examples should be generated from the Connectathon testing activities.

 