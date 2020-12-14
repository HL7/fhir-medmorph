### Business Need
 
* Onboard a new research data partner to join a research network, contribute data that can be used for research. Current processes used to contribute data involve many non-standardized mechanisms and each data partner contributing data may use different structure (e.g., formats) and different semantics. In order for the data to be usable, the data will have to be transformed to some data model so that it can be queried and analyzed. This process when standardized will expedite onboarding processes and deliver better quality data at a lower cost. 

* Once a data partner joins a research network and contributes data, a researcher should be able to perform queries to filter specific data sets and analyze the data to improve treatments and outcomes. Currently researchers are able to query data marts on a limited basis because of the variations in the data models (e.g., PCORnet CDM, i2b2, OMOP, FHIR Resources) and the technologies (e.g., Microsoft SQL, Postgres SQL, SAS, etc.)  used to host the data mart. Standardizing these data access mechanisms will help overcome the hurdles faced by researchers in accessing data from EHRs.

### Research Abstract Model for Onboarding a Data Partner to Populate a Data Mart

Figure 4.1 below shows the research abstract model to onboard a data partner who can contribute data from their EHR system to populate a data mart.

**Figure 4.1 - Populating a Data Mart from an EHR**

{::options parse_block_html="false" /}

<figure>
  <img height="600px" src="DataPartnerOnboarding.png" alt="Figure 4.1 - Populating a Data Mart from an EHR " style="vertical-align:middle"/>
</figure>

{::options parse_block_html="true" /}

<br>

As shown in Figure 4.1 above, the Backend Service App will extract data from an EHR for one or more patients and use the Data/Trust Services to perform translation, masking and populate the data mart. In the above diagram the data mart exists within the health care organization. The Data Mart and the Backend Service App could reside outside the health care organization as shown in Figure 4.2 below. 

**Figure 4.2 - Populating an Externally Hosted Data Mart from an EHR**

{::options parse_block_html="false" /}

<figure>
  <img height="600px" src="OnboardingExternalDataPartner.png" alt="Figure 4.2 - Populating an Externally Hosted Data Mart from an EHR " style="vertical-align:middle"/>
</figure>

{::options parse_block_html="true" /}

<br>

### Research Abstract Model for Querying a Data Mart

Figure 4.3 shows the steps for a researcher to query a data mart and access data.

**Figure 4.3 - Researcher Querying a Data Mart**

{::options parse_block_html="false" /}

<figure>
  <img height="600px" src="ResearchDataQuery.png" alt="Figure 4.3 - Researcher Querying a Data Mart" style="vertical-align:middle"/>
</figure>

{::options parse_block_html="true" /}

<br>

As shown in Figure 4.3 above, the researcher submits a query to a researcher portal which then sends the query to be submitted to the various data marts after translating the query as needed. The query is then received by the Backend Service App and then executed on the data marts, the results are collected and then sent back to the researcher after translation. This workflow is currently being standardized by the Common Data Model Harmonization (CDMH) project led jointly by FDA and NIH. We will be leveraging many of capabilities being built by the CDMH project to execute this workflow and add additional APIs and standards as needed.

The detailed description of the steps are as described below:

* Step Q1 :  In this Step, the researcher composes a query in FHIR and creates a Knowledge Artifact (PlanDefinition, ValueSets, Data Marts to be queried etc) that contains the query. This is submitted to the Researcher Portal.

* Step Q2:  The FHIR query is then submitted for translation so that it is converted into multiple formats ( OMOP query, FHIR query, i2b2 query, PCORNet CDM query) based on the data models that need to be queried.

* Step Q3: In this step, the query is distributed to each of the data marts to be queried. The query language, syntax and expressions will be specific to the data model supported by each data mart.

* Step Q4: The Backend Service App receives the query via the Knowledge Artifact and runs submits the query for execution to the Data Mart.

* Step Q5: The Data Mart completes the execution and returns the results back to the Backend Service App.

* Step Q6: The results are forwarded to Results Translator and Aggregator to translate back to FHIR and aggregate it as needed.

* Step Q7: The results are then submitted back to the Researcher Portal.

* Step Q8: The results are made available to the researcher in a FHIR format.



### MedMorph Research Actors and Definitions

This section defines the Actors and Systems that interact within the MedMorph Reference Architecture to realize the capabilities of the use cases listed above.

1. __Electronic Health Record (EHR)__:  A system used in care delivery for patients and that captures and stores data about patients and makes the information available instantly and securely to authorized users. While an EHR does contain the medical and treatment histories of patients, an EHR system is built to go beyond standard clinical data collected in a provider’s provision of care location and can be inclusive of a broader view of a patient’s care. EHRs are a vital part of health IT and can:

	* Contain a patient’s medical history, diagnoses, medications, treatment plans, immunization dates, allergies, radiology images, and laboratory and test results
	* Allow access to evidence-based tools that providers can use to make decisions about a patient’s care
	* Automate and streamline provider workflow

	A **FHIR Enabled EHR** exposes FHIR APIs for other systems to interact with the EHR and exchange data. FHIR APIs provide well defined mechanisms to read and write data. The 	FHIR APIs are protected by an Authorization Server which authenticates and authorizes users or systems prior to accessing the data.

2. __Provider__:  A person who delivers health care to a patient. In addition to delivering care, the provider also updates the EHR with the patient data captured during the workflow and signs off on the content added, deleted, or modified in the patient record once it is updated. The provider can be the individual doctor, nurse, or a combination of different people that are part of the care team.

3. __Data Mart__: Data Mart represents a system that will hold data that is to be accessed by researchers. Typically researchers do not access the EHR or operational data stores directly as they are used for clinical operations. In order to facilitate access to researchers healthcare organizations populate data stores that can be accessed by researchers. These data stores are called Data Marts in our abstract model. Typically these data stores are present within the healthcare organization, but could also be hosted externally to the healthcare organization. The data models that are used to store the data may depend on the types of research studies. The following are the commonly used data models for research.

	* PCORnet CDM
	* i2b2
	* OMOP
	* Sentinel
	* FHIR is one of the data models that could also be used for research in the future as it gets adopted in the research settings. 

4. __Backend Service App__:  For research use cases, the Backend Service App represents a system that resides within the clinical care setting and interacts with the EHRs, Data Marts and the systems used by the researchers. The system will use specific Knowledge Artifact resources to identify when data has to be extracted, when data has to be queried and how results have to be returned. The term “Backend Service” is used to refer to the fact that the system does not require user intervention to perform reporting. The term “App” is used to indicate that it is similar to SMART on FHIR App which can be distributed to clinical care via EHR vendor-specified processes. The EHR vendor-specified processes enable the Backend Service App to use the EHR's FHIR APIs to access data. The healthcare organization is responsible for choosing/developing and maintaining the Backend Service App within the organization.

5. __Trust Service Provider__:  Trust Service Provider affords capabilities that can be used to translate data between different models, map terminologies between different data models, pseudonymize, anonymize, de-identify, hash, or re-link data that is submitted to public health and/or research organizations. These capabilities are called Trust Services. Trust Services are used, when appropriate, by the Backend Service App.  

6. __Research Organization__: An organization that can access the data from clinical care or data repositories for research purposes with appropriate data use agreements, authorities, and policies.

7. __Researcher Portal__: A system that is used by the researcher to explore which data marts exist, compose queries, submit the queries, receive results for analysis.

8. __Query Translator and Submitter__: A system that interacts with the Researcher Portal to receive queries, translate queries and then submit the queries to the Backend Service App.

9. __Result Translator and Aggregator__: A system that interacts with the Backend Service App to receive results back from healthcare organizations and then translates the results as needed and aggregates the data if needed before forwarding the results to the Researcher Portal.

