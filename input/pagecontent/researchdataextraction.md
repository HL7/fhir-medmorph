This section defines the specific conformance requirements for onboarding a data partner to populate a Data Mart as specified in this MedMorph Reference Architecture (RA) Implementation Guide (IG).


### Data Source Requirements

* The Data Source (e.g., EHR) system SHALL support the [base]/Group/[id]/$export operation for exporting data for one or more patients.

* The Data Source system SHALL support search of Group resources using identifier and name.

* The Data Source system SHALL support all resource types available in the Data Source related to Patient compartment for the [base]/Group/[id]/$export?_type parameter.

* The Data Source system SHALL support the Bulk Data Request Flow as defined in the [Bulk Data Access IG specification.]({{site.data.fhir.ver.bulkig}}/index.html)

* The Data Source system MAY support the Bulk Data Delete Request as defined in the Bulk Data Access IG specification.

* The Data Source system SHALL support the Bulk Data Status Request as defined in the Bulk Data Access IG specification.

* The Data Source system SHALL set the requireAccessToken to true within the Bulk Data Status Request response body as defined in the Bulk Data Access IG specification.

* The Data Source system SHALL require the Health Data Exchange App (HDEA) to provide valid access token to export the data.

* When the HDEA does not have appropriate authorization to the data requested, the Data Source system SHALL return OperationOutcome with appropriate error message.

* The Data Source system SHALL support the [US Core profiles]({{site.data.fhir.ver.uscoreR4}}/index.html) to export the content for each patient.

* The Data Source system SHALL support ```system/*.read`` scope to access data for multiple patients.

#### Creation of Group Resource 

* The Data Source system SHOULD support the creation of the Group Resource using the POST API. The Group will contain a list of patients who have consented to contribute their data for research.

Note: The Group Resource creation process depends on the health care organizations, the workflows used, and the approvals required. This varies widely and will be left to each health care organization.


#### Consent Management  

Health care organizations are responsible for collecting patient consent before sharing the patient data for research. Each health care organization follows applicable laws and their own policies and processes to obtain consent. Consent obtained may be represented using electronic and structured data or could be a signed PDF document. Consent also may be stored as part of a Data Source or an external system. As part of the MedMorph RA IG development, Consent Management is not in-scope due to the above variations and the current lack of standardization.


#### Validating Consent Before Disclosing Data

Disclosing identifiable data for research requires explicit patient consent. When the HDEA requests the Data Source to export all the data for one or more patients, the Data Sources FHIR Authorization Server must validate the consent before sharing data for research. The process of enforcement of consent varies by health care organization. MedMorph does not prescribe any standard mechanism to enforce consent but assumes that the Data Source Authorization Server will allow the [base]/Group/[id]/$export to continue if all the patients in the Group have consented to share their data.


#### Data Source APIs and Profiles to be Supported

The following APIs have to be supported by the Data Source

```
[FHIR base URL]/Group/[id]/$export with all available types in a Patient Compartment.
```

All US Core profiles must be supported by the Data Source and data exported have to be represented using the US Core profiles. 


### HDEA Requirements

* The HDEA SHALL register with the Data Sources Authorization Server to obtain client id, client secret to invoke Data Source FHIR APIs for Bulk Data Access. 

* The HDEA SHALL process a Knowledge Artifact PlanDefinition resource to determine the following 
	* How frequently the data has to be exported?

* The HDEA SHALL get authorized and obtain necessary access tokens to invoke the operations necessary to export data from the Data Source.

* The HDEA SHALL invoke [FHIR Base URL]/Group/[id]/$export and include all types from the Patient compartment in the export request.

* Once the data is exported, The HDEA SHALL download/fetch the data from the Data Source per the Bulk Data Access IG specification.

* The HDEA SHALL apply translations to the data as needed using the Trust Service Provider when specified by the Knowledge Artifact. 

* The HDEA SHOULD leverage FHIR to research data model mappings in the [Common Data Models Harmonization (CDMH) IG](http://hl7.org/fhir/us/cdmh/index.html) to identify the FHIR resources and APIs required to populate the Data Marts.


### Data Mart Requirements

Data Marts store the extracted data from the Data Source using different data models and technologies. Once the data is extracted from the Data Sources the HDEA could automatically populate the Data Mart if it supports FHIR APIs. Currently most Data Marts do not support FHIR. Standardizing the ability to populate a Data Mart using FHIR APIs is desirable to enable efficient data partner onboarding and data mart population. 

* Data Marts SHOULD support POST FHIR APIs for each of the US Core resource profiles so that the HDEA can populate the Data Mart.

* The Data Mart SHOULD leverage FHIR to research data model mappings in the CDMH IG to identify the FHIR resources and APIs to be supported.

* The Data Mart SHOULD accept the data for each resource in FHIR format and translate to internal data models leveraging the CDMH IG mappings where applicable. 

#### APIs and Profiles to be Supported by Data Mart

```
POST [FHIR Base URL]/[Resource]/ for each resource and profile specified in the US Core IG and CDMH IG.
```

### Trust Service Provider Requirements

The Trust Service Provider supports services required for the various research use cases. 

* The Trust Service Provider SHALL support the operations as specified in the Trust Service Provider Capability Statement. 


### Sufficiency of US Core Data Elements

US Core profiles contain data elements identified in US regulations with a few additions agreed upon by the Data Source vendors. These additions are identified as the most important data elements for payment, treatment, and operations. However, the research data models are richer in nature and contain significantly more data elements, many of which are not present in the US Core profiles. 
Acknowledging this fact, the MedMorph RA IG is prescribing US Core as starting point. Content IG developers SHOULD leverage CDMH IG to populate the data elements required for common research data models.

 
