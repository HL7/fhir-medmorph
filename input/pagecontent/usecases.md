### Business Need

* For Health Care Surveys: Identify and collect hospital (emergency department and inpatient care) and ambulatory care practice data and electronically report them to a system hosted at the federal level. Electronic reporting will increase the response rate of sampled hospitals and ambulatory health care providers to the National Hospital Care Survey (NHCS) and the National Ambulatory Medical Care Survey (NAMCS). This will also increase the volume, quality, completeness, and timeliness of the data submitted to the NHCS and NAMCS. Electronic reporting via automated means (without provider involvement) will reduce the burden associated with survey participation and reduce costs associated with recruiting hospital and ambulatory health care providers.

* For Hepatitis C: Leverage existing FHIR infrastructure including electronic case reporting to improve the availability of data that advance national priorities such as eliminating hepatitis C as a public threat in the US, especially if it becomes a chronic condition when not treated acutely. The goal is to optimize access to hepatitis C data already captured within the EHR.

* For Cancer Registry Reporting: Leverage existing FHIR infrastructure to transmit cancer case information primarily from ambulatory care practices to state cancer registries. The intent is to automate and provide access to data not currently available to public health and research. Automation of cancer case registry reporting will reduce the burden of manual or other non-standardized data collection processes that currently exist. The MedMorph cancer registry reporting use case will not replace hospital cancer registry data collection and reporting methods which are working well. Hospital-based cancer registries use certified tumor registrars (CTRs) to abstract data and maintain information on cancers diagnosed and/or treated in their hospitals. Hospital cancer registries and CTRs follow standardized rules and data collection methods for the purpose of cancer analysis and research. Alternatively, EHR data are primarily used for clinical care and routinely require manipulation and translation to meet the needs of the state cancer registry. Additionally, since 1995, hospital cancer registries have been reporting cancer cases to state cancer registries using a national standard format and process. MedMorph Reference Architecture IG does not replace this existing cancer reporting mechanism which works well. The MedMorph central cancer reporting use case is therefore intended to be used primarily by non-hospital providers (e.g., physician offices, oncology clinics, radiation therapy centers, etc.) and by hospitals without cancer registries. 

* For Research: Onboard a new research data partner to join a research network, contribute data that can be used for research. Current processes used to contribute data involve many non-standardized mechanisms and each data partner contributing data may use different structure (e.g., formats) and different semantics. In order for the data to be usable, the data will have to be transformed to some data model so that it can be queried and analyzed. This process when standardized will expedite on-boarding processes and deliver better quality data at a lower cost. 

By leveraging FHIR-based APIs, research and public health organizations can automate data collection (based on specific events and conditions in clinical workflows) and submission of the collected data, complying with all necessary authorities and policies applicable. A more detailed discussion of the MedMorph Use Cases can be found [here](https://carradora.atlassian.net/wiki/spaces/MedMorph/pages/381780019/Use+Case+Work+Groups#Use-Cases).


### MedMorph Actors and Definitions

This section defines the Actors and Systems that interact within the MedMorph Reference Architecture to realize the capabilities of the use cases listed above.

1. __Electronic Health Record (EHR)__:  A system used in care delivery for patients and that captures and stores data about patients and makes the information available instantly and securely to authorized users. While an EHR does contain the medical and treatment histories of patients, an EHR system is built to go beyond standard clinical data collected in a provider’s provision of care location and can be inclusive of a broader view of a patient’s care. EHRs are a vital part of health IT and can:

	* Contain a patient’s medical history, diagnoses, medications, treatment plans, immunization dates, allergies, radiology images, and laboratory and test results
	* Allow access to evidence-based tools that providers can use to make decisions about a patient’s care
	* Automate and streamline provider workflow

	A **FHIR Enabled EHR** exposes FHIR APIs providing a mechanism for other systems to interact with the EHR and exchange data. FHIR APIs provide well defined mechanisms to read and write data. The 	FHIR APIs are protected by an Authorization Server which authenticates and authorizes users or systems prior to accessing the data.

	**NOTE:** At this time the IG uses the term EHR to indicate Certified EHR Technology (CEHRT) according to the ONC regulations, but in the future MedMorph will expand to include additional types of HIT systems.
 
2. __Provider__:  A person who delivers health care to a patient. In addition to delivering care, the provider also updates the EHR with the patient data captured during the workflow and signs off on the content added, deleted, or modified in the patient record once it is updated. The provider can be the individual doctor, nurse, or a combination of different people that are part of the care team.

3. __Knowledge Artifact Repository__:  Enables the full spectrum of reporting and querying for programs, studies, and initiatives. A Knowledge Artifact Repository represents the system that is used to distribute metadata such as surveys, trigger codes, timing constraints, decision support artifacts, and other knowledge artifacts to:

	* Identify that the criteria to trigger reporting have been met (e.g., triggering a report for a patient case or for bulk data transfer)
	* Identify the timing parameters for the reporting (e.g., immediate, batched at the end of the day/week/month/quarter/etc.)
	* Identify the data that needs to be collected for reporting 
	* Identify the structure of the report
	* Identify decision support rules for reporting

	Note: Knowledge artifacts may contain PHI or PII information

	An example of a Knowledge Artifact Repository is the Association of Public Health Laboratories (APHL) Informatics Messaging Services (AIMS) Platform). The [AIMS Platform](https://www.aphl.org/programs/informatics/Pages/aims_platform.aspx) hosts the eRSD Knowlede Artifact used for electronic case reporting program.

4. __Backend Service App__:  A system that resides within the clinical care setting and performs the reporting functions to public health and/or research registries. The Backend Service App uses the information supplied by the knowledge artifact repository along with applicable authorities and policies to determine when reporting needs to be done, what data needs to be reported, how the data needs to be reported, and where the data should be reported. The term “Backend Service” is used to refer to the fact that the system does not require user intervention to perform reporting. The term “App” is used to indicate that it is similar to a SMART on FHIR App which can be distributed to clinical care via EHR vendor-specified processes. The EHR vendor-specified processes enable the Backend Service App the ability to use the EHR's FHIR APIs to access data. The healthcare organization is responsible for choosing/developing and maintaining the Backend Service App within the organization.

5. __Trust Service Provider__:  Trust Service Provider affords capabilities that can be used to pseudonymize, anonymize, de-identify, hash, or re-link data that is submitted to public health and/or research organizations. These capabilities are called Trust Services. Trust Services are used, when appropriate, by the Backend Service App.  

6. __Trusted Third Party__: A system (e.g., Health Information Exchange (HIE), Reportable Condition Knowledge Management System (RCKMS)/Association of Public Health Laboratories (APHL) Informatics Messaging Services (AIMS) Platform) at an intermediary organization that serves as a conduit to exchange data between healthcare organizations and an endpoint and may additionally perform other intermediary functions (e.g., apply business logic and inform the Reportability Response) using appropriate authorities and policies. For example, In electronic case reporting use case RCKMS on the AIMS platform is playing a critical compliance role to confirm reportability in accordance with state laws.

7. __Data Mart also known as Data Repository__: A system receiving the data from the clinical care system. A Data Mart / Data Repository is used to represent systems such as cancer registries, National Healthcare Survey data stores, surveillance systems, and electronic vital records systems. Data Marts are actively managed and are used to receive data, store data, and perform analysis as appropriate. These data marts could be operated or accessed by Public Health Authority (or their designated organizations such as trusted third parties), clinical are organizations or research organizations, with appropriate authorities and policies.

	A **Local Data Mart** is one that is hosted by the clinical care organization itself and it may be used to support research use cases, reporting use cases and other analytic capabilities.
	
	A **TTP Hosted Data Mart** is one that is hosted by a Trusted Third Party on behalf of Public Health or Research Organization using appropriate authorities and policies.
	Data Marts can also be hostedb by the PHA or Resarch Organization themselves using the appropriate authorities and policies. 

8. __Public Health Authority (PHA)__: A government or a government-designated organization allowed to receive the data from Trusted Third Parties or provider organizations using appropriate authorities and policies. PHA may also analyze the data and initiate responses back to clinical care. For more detailed information on PHA please refer to https://www.hhs.gov/hipaa/for-professionals/special-topics/public-health/index.html. 

9. __Research Organization__: An organization that can access the data from clinical care or data marts for research purposes with appropriate data use agreements, authorities, and policies.


#### Interactions between MedMorph Actors and Systems

This section outlines the high level interactions between the various MedMorph Actors and Systems defined above. These interactions are shown in the Figure 3.0 below along with the descriptions for each step.

 
 
 {% include img.html img="MedMorphActorsAndSystems.png" caption="Figure 3.0 - MedMorph Actors, Systems and their Interactions" %}


<br>
The descriptions for each step in the above diagram are described below:

* Step 1: A PHA or a Research Organization creates a Knowledge Artifact and makes it available via the Knowledge Artifact Repository.

	* Step 1a: Knowledge Artifact Repositories which implement notifications, can optionally notify the subscribers (EHRs, Backend System App, Administrators) of changes in the Knowledge Artifacts.
	
* Step 2: Backend Service App (BSA) queries the Knowledge Artifact Repository to retrieve a Knowledge Artifact.
 
	* Step 2a: BSA receives the Knowledge Artifact as a response to the query in Step 2.
	
* Step 3: BSA processes the Knowledge Artifact and creates subscriptions in the EHR’s FHIR Server so that it can be notified when specific events occur in clinical workflows.
* Step 4: Providers as part of their clinical workflows update the data in the EHR patient chart.
* Step 5: EHR notifies the BSA based on subscriptions that have been created in Step 3.
* Step 6: BSA queries the EHR for patient’s data.

	* Step 6a: BSA receives the response from the EHR with the Patient’s data.
	
* Step 7: After the creation of the report with identifiable data that needs to be submitted, the BSA invokes Data/Trust Services to de-identify, anonymize, pseudonymize the report as needed.

	* Step 7a: BSA receives the de-identified, anonymized or pseudonymized report.
	
* Step 8: The BSA submits the created report in Step 7 to a local Data Mart or the PHA/Research Organization directly without any Trusted Third Parties acting as intermediaries.

	* Step 8a: PHA/Research Organizations can require TTP to act as intermediaries to receive reports from clinical organizations. For these scenarios, the BSA submits the created report to the TTP.
	Step 8b: The TTP receives a submitted report from the BSA and forwards the report to the PHA/Research Organization.
	
* Step 9: The PHA/Research Organization submits a response back to the Healthcare organization based on the submitted report. If the Data Mart is local, an acknowledgement may be provided without an actual response. The Response transaction can be synchronous (immediately w.r.t Step 8) or asynchronous (after a period of time).

	* Step 9a: The PHA/Research Organizations can require TTP to act as intermediaries to submit reports to clinical organizations. For these scenarios, the PHA/Research Organization submits a response to the TTP.
	* Step 9b: The TTP receives the submitted response from the PHA/Research Organization and forwards the response to the BSA which is part of the healthcare organization.
	
* Step 10: The BSA writes back the response from the PHA/Research Organization to the EHR as appropriate. Note: The Response may have to be re-identified in some scenarios using Data/Trust Services before it is written back to the EHR.


 

The next few sections considers subsets of the above the interactions for ease of understanding and documenting detailed interactions using applicable FHIR mechanisms. 


### MedMorph Workflows

The following section outlines each of the MedMorph workflows, actors, and their interactions based on [MedMorph Use Cases](https://carradora.atlassian.net/wiki/spaces/MedMorph/pages/381780019/Use+Case+Work+Groups#Use-Cases). 

#### Provisioning Workflow

The Provisioning Workflow involves the activities whereby a PHA or a Research Organization publishes a Knowledge Artifact(s) specific to a use case. The published Knowledge Artifact(s) is then downloaded by the Backend Service App. The Backend Service App may subscribe to notifications from the EHR based on data present within the Knowledge Artifact. The actors and steps involved in the workflow are shown in Figure 3.1 below.


{% include img.html img="ProvisioningWorkflow.png" caption="Figure 3.1 - Provisioning Workflow" %}


<br>
The descriptions for each step in the above diagram are described below:

* Step P1: PHA/Research Organization or their designated organization creates the necessary knowledge artifacts for different reporting/research scenarios and publishes the artifacts. These may include trigger codes, timing constraints, report definitions, surveys, value sets knowledge artifacts etc.

* Step P2: The Backend Service App queries the Knowledge Artifact Repository for the appropriate artifact.
Note: Step P2 could be a result of a out of band notification (e.g. email/sms) that something has changed or it could be a response to a subscription notification.

* Step P3:  The Backend Service App receives the knowledge artifact in response to the query in step P2.

* Step P4: The Backend Service App subscribes to topics in the EHR based on the metadata received in step P3 for reporting. 

<br>

#### Notification Workflow

The Notification Workflow involves the activities whereby a provider, as part of the care delivery process, updates a patient's medical record within an EHR. The EHR, based on the changes to the medical record and existing subscriptions, will notify the Backend Service App of the changes in the medical record. The Backend Service App will perform actions such as querying the EHR for additional data, scheduling a reporting job, and creating a report for submission based on the notification and the Knowledge Artifact data. The actors and steps involved in the workflow are shown in Figure 3.2 below.

{% include img.html img="NotificationWorkflow.png" caption="Figure 3.2 - Notification Workflow" %}

<br>
The descriptions for each step in the above diagram are described below:

* Step N1: The provider as part of the care delivery process updates the patient record within the EHR. 

* Step N2: The EHR will notify the Backend Service App about the changes in the Patient record based on subscriptions.

* Step N3:  Upon receiving a notification, the Backend Service App queries the patient record from the EHR using FHIR APIs to evaluate if reporting needs to be performed. (The queries executed in this step are only for evaluation purposes and not for creation of the payload for submission).

* Step N4: The Backend Service App receives the data from the EHR based on the queries in Step N3.

* Step N5: Depending on reporting parameters and the data received from the EHR in step N4, the Backend Service App may perform internal processing such as setting up scheduled jobs for reporting at a later time or designating that there is nothing to report.

<br>

#### Report Creation Workflow

The Report Creation Workflow involves the activities whereby the Backend Service App collects the data necessary for creating the report to be submitted to the PHA or Research Organization. The Backend Service App may then perform additional activities such as pseudonymization, anonymization or de-identification based on the specific use case and knowledge artifact. Finally, the Backend Service App assembles the data in the required reporting format for submission. The actors and steps involved in the workflow are shown in Figure 3.3 below.

{% include img.html img="ReportCreationWorkflow.png" caption="Figure 3.3 - Report Creation Workflow" %}

<br>
The descriptions for each step in the above diagram are described below:

* Step D1: The data collection and submission report creation workflow is triggered for a specific patient based on the output of the Notification workflow.

* Step D2: The Backend Service App queries the EHR to retrieve the necessary data for the patient to create the submission report. 

* Step D3: The EHR returns the data for the queries from step D2.

* Step D4: The Backend Service App processes the data returned from the EHR, applying necessary filters and logic as appropriate.

* Step D5: The Backend Service App based on the type of submission will invoke the Data/Trust Services to de-identify or pseudonymize, hash, or anonymize the collected data before generating the report.

* Step D6: The Data/Trust Services return after de-identifying or  pseudonymizing or hashing or anonymizing the data based on the request in Step D5.

* Step D7: The Backend Service App creates the Submission report from the data collected for submission.

<br>

#### Data Submission Workflow

The Data Submission Workflow involves the activities whereby the Backend Service App packages the data and submits the data to the PHA or Research Organization. The data submission may occur through a Trusted Third Party. The actors and steps involved in the workflow are shown in Figure 3.4 below.


{% include img.html img="DataSubmissionWorkflow.png" caption="Figure 3.4 - Data Submission Workflow" %}

<br>
The descriptions for each step in the above diagram are described below:

* Step S1: The data submission workflow starts after the report is generated by the Data Collection and Submission Report Creation workflow and the report is validated.

* Step S2: The Backend Service App uses the FHIR APIs to submit the report to the Trusted Third Party or to the PHA/Research Organization directly based on authorities and policies. 

* Step S3: The Trusted Third Party forwards the data to the PHA/Research Organization based on authorities and policies.

<br>

#### Receiving Response/Acknowledgement Workflow (PUSH Method)

The Receiving Response/Acknowledgement Workflow involves the activities whereby the PHA or Research Organization provides a response back to clinical care. This workflow depicts the PUSH method (i.e., PUSH from PHA/Research Organization to Clinical Care). The actors and steps involved in the workflow are shown in Figure 3.5 below.

{% include img.html img="ResponseWorkflowPush.png" caption="Figure 3.5 - Receiving Response/Acknowledgement Workflow (PUSH Method)" %}

<br>
The descriptions for each step in the above diagram are described below:

* Step R1: The PHA/Research Organization analyzes the data received from the Data Submission workflow and crafts a response to be sent back to submitting Healthcare Organization.

* Step R2: The PHA/Research Organization then submits the response to the Healthcare Organization directly or via Trusted Third Party based on authorities and policies.

* Step R3: The Trusted Third Party routes the response back received from the PHA/Research Organization to the Healthcare Organization.

* Step R4, R5: The Backend Service App receives the Response and re-identifies the data as needed.

* Step R6: The Backend Service App forwards the response back to the EHR.


<br>


### Onboarding a Health Care Organization (Data Partner) to Populate a Data Mart

Figure 3.7 below shows the research abstract model to onboard a data partner who can contribute data from their EHR system to populate a data mart.

{% include img.html img="DataPartnerOnboarding.png" caption="Figure 3.7 - Populating a Data Mart from an EHR" %}

<br>
The descriptions for each step in the above diagram are described below:

* Step S1:  In this Step, the Backend Service App will initiate an extraction process to get data from an EHR for one or more patients. 

* Step S2: The Backend Service App then uses the Data/Trust Services to transform the data if needed from one data model to another data model. The step may also involve de-identification, anonymization or pseudonymization.

* Step S3 : Once the data is transformed in step S2, the data will then be populated into the data mart.

<br>

As shown in Figure 3.7 above, the Backend Service App will extract data from an EHR for one or more patients and use the Data/Trust Services to perform translation, masking and populate the data mart. In the above diagram the data mart exists within the health care organization. The Data Mart and the Backend Service App could reside outside the health care organization as shown in Figure 3.8 below. 

{% include img.html img="OnboardingExternalDataPartner.png" caption="Figure 3.8 - Populating an Externally Hosted Data Mart from an EHR" %}

<br>
The descriptions for each step in the above diagram are described below:
 
 * Step S1:  In this Step, the Backend Service App will initiate an extraction process to get data from an EHR for one or more patients. 

* Step S2: The Backend Service App then uses the Data/Trust Services to transform the data if needed from one data model to another data model. The step may also involve de-identification, anonymization or pseudonymization.

* Step S3 : Once the data is transformed in step S2, the data will then be populated into the data mart.
 
<br>
