This section of the implementation guide defines the specific conformance requirements for onboarding a data partner to populate a data mart.


### Requirements for EHR

* EHR system SHALL support the [base]/Group/[id]/$export operation for exporting data of one or more patients.

* The EHR system SHALL support search of Group resources using identifier, name.

* EHR system SHALL support all resource types available in the EHR related to Patient compartment for the [base]/Group/[id]/$export?_type parameter.

* EHR system SHALL support the Bulk Data Request Flow as defined in the Bulk Data Access IG specification.

* EHR system MAY support the Bulk Data Delete Request as defined in the Bulk Data Access IG specification.

* EHR system SHALL support the Bulk Data Status Request as defined in the Bulk Data Access IG specification.

* EHR system SHALL set the requireAccessToken to true within the Bulk Data Status Request response body as defined in the Bulk Data Access IG specification.

* EHR system SHALL require Backend Service App to provide valid access token to export the data.

* When the Backend Service App does not have appropriate authorization to the data requested, the EHR system SHALL return OperationOutcome with appropriate error message.

* The EHR system SHALL support the US Core profiles as identified in the US Core STU 3 implementation guide to export the content for each patient.

* The EHR system SHALL support ```system/*.read and patient/*.read`` scopes to access data for multiple patients.

#### Creation of Group Resource 

* The EHR system SHALL support the creation of the Group Resource using the POST API. The Group will contain a list of patients who have consented to contribute their data for research.

Note: The Group resource creation process will depend on the healthcare organizations and the workflows used and the approvals required. This varies widely and hence this will be left to the healthcare organizations. 

```
Feedback Requested: Since there may be many research activities supported by a healthcare organization, there could be multiple instances of Groups each with overlapping or no-overlapping set of patients. We need to determine what is the best way to search for different groups. 
Currently, we are prescribing searching for Group by identifier where the identifier and name. However we need some feedback on if this is a good mechanism to search or should a Group resource instance be provisioned for each research activity approved by the IRB ?
```

#### Consent Management  

Healthcare organizations are responsible for collecting patient Consent before sharing the patient data for research. Each healthcare organization follows their own policies and processes to obtain consent. Consent obtained may be represented using electronic and structured data or could just be a signed PDF document. Consent also may be stored as part of an EHR or an external system. Because of these variations we are requesting the following feedback.

```
Feedback Requested: 
1. Should MedMorph prescribe representation of Consent using HL7 FHIR Consent resource ? 
2. Where should this consent be stored ? In the EHR or External Systems ?
Alternatively: 
1. Should MedMorph prescribe the use of ResearchStudy resource for each Study and enroll each participant as a ResearchParticipant and assume that Consent collection and validation is performed as the patient is enrolled in the study ? 
```

#### Validating Consent before disclosing data

Disclosing data for research requires explicit patient consent. When the Backend Service App requests the EHR to export all the data for one or more patients, the EHRs FHIR Authorization Server has to validate the consent before sharing data for research. Again the process of enforcement of Consent varies by healthcare organization. MedMorph does not prescribe any standard mechanism to enforce Consent but assumes that the EHR Authorization Server will allow the [base]/Group/[id]/$export to continue if all the patients in the Group have consented for their data to be shared.

#### EHR APIs and Profiles to be Supported

The following APIs have to be supported by the EHR

```
[FHIR base URL]/Group/[id]/$export with all available types in a Patient Compartment.
```

All US Core profiles have to be supported by the EHR and data exported have to be represented using the US Core profiles. 


### Requirements for a Backend Service App

* The Backend Service App SHALL register with the EHRs Authorization Server to obtain client id, client secret to invoke EHR FHIR APIs for Bulk Data Access. 

* The Backend Service App SHALL process a Knowledge Artifact PlanDefinition resource to determine the following 
	* Which Group to use to export the data for patients ?
	* How frequently the data has to be exported ?
	* What translations are required for the data post extraction ? 

* The Backend Service App SHALL get authorized and obtain necessary access tokens to invoke the operations necessary to export data from the EHR.

* The Backend Service App SHALL invoke [FHIR Base URL]/Group/[id]/$export and include all types from the Patient compartment in the export request.

* Once the data is exported, The Backend Service App SHALL download/fetch the data from the EHR per the Bulk Data Access IG specification.

* The Backend Service App SHALL apply translations to the data as needed using the Data/Trust Services. 

* For Data Marts supporting FHIR APIs for data ingestion, the Backend Service App SHALL invoke the POST APIs for each resource to create the resource in the Data Mart.

* The Backend Service App SHALL leverage FHIR to research data model mappings developed by CDMH to identify the FHIR resources and APIs required to populate the Data Marts.


### Requirements for a Data Mart

Data Marts store the extracted data from the EHR using different data models and technologies. Once the data is extracted from the EHRs the Backend Service App could automatically populate the data mart if it supports FHIR APIs. Currently most Data Marts do not support FHIR. Standardizing the ability to populate a data mart using FHIR APIs will be desirable to enable efficient data partner onboarding and data mart population. 

* Data Marts SHALL support POST FHIR APIs for each of the US Core resource profiles so that the Backend Service App can populate the Data Mart.

* The Data Mart SHALL leverage FHIR to research data model mappings developed by CDMH to identify the FHIR resources and APIs to be supported.

* The Data Mart SHALL accept the data for each resource in FHIR format and translate to internal data models leveraging the CDMH developed mappings where applicable. 

#### APIs and Profiles to be Supported by Data Mart

```
POST [FHIR Base URL]/[Resource]/ for each US Core resource and profile specified in the US Core implementation guide.
```

### Requirements for a Trust Service Provider

The Trust Service Provider supports many of the Data/Trust Services required for the various research use cases. 

* The Trust Service Provider SHALL support the Trust Service Operations as specified in the Trust Service Provider Capability Statement. 

```
Feedback Requested: Should MedMorph prescribe specific behavior/operation to translate from one data model to another ? Since this is an internal implementation detail of the organization, MedMorph's intention is to leverage the CDMH mappings from FHIR to research data models but not prescribe any behavioral/operational constructs for the actual conversion.
```


### Sufficiency of US Core data elements

US Core profiles contain data elements identified in the US regulations with a few additions that have been agreed upon by the EHR vendors as the most important data elements for payment, treatment and operations. However the research data models are richer in nature and contain significantly more data elements not present in the US Core profiles. 
Acknowledging this fact, the MedMorph Architecture IG is prescribing US Core as starting point to build the framework. As the CDMH project completes, a Content IG could be developed for populating Data Marts leveraging CDMH work to create profiles to populate all the data elements required for each data model.

 
