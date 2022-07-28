### Business Need

* For Health Care Surveys: Identify and collect hospital (emergency department and inpatient care) and ambulatory care practice data and electronically report them to a system hosted at the federal level. Electronic reporting will increase the response rate of sampled hospitals and ambulatory health care providers to the National Hospital Care Survey (NHCS) and the National Ambulatory Medical Care Survey (NAMCS). This will also increase the volume, quality, completeness, and timeliness of the data submitted to the NHCS and NAMCS. Electronic reporting via automated means (without provider involvement) will reduce the burden associated with survey participation and reduce costs associated with recruiting hospital and ambulatory health care providers.

* For Hepatitis C: Leverage existing FHIR infrastructure including electronic case reporting to improve the availability of data that advance national priorities such as eliminating hepatitis C as a public threat in the US, especially if it becomes a chronic condition when not treated acutely. The goal is to optimize access to hepatitis C data already captured within the EHR.

* For Cancer Registry Reporting: Leverage existing FHIR infrastructure to transmit cancer case information primarily from ambulatory care practices to state cancer registries. The intent is to automate and provide access to data not currently available to public health and research. Automation of cancer case registry reporting will reduce the burden of manual or other non-standardized data collection processes that currently exist. The MedMorph cancer registry reporting use case will not replace hospital cancer registry data collection and reporting methods which are working well. Hospital-based cancer registries use certified tumor registrars (CTRs) to abstract data and maintain information on cancers diagnosed and/or treated in their hospitals. Hospital cancer registries and CTRs follow standardized rules and data collection methods for the purpose of cancer analysis and research. Alternatively, EHR data are primarily used for clinical care and routinely require manipulation and translation to meet the needs of the state cancer registry. Additionally, since 1995, hospital cancer registries have been reporting cancer cases to state cancer registries using a national standard format and process. MedMorph Reference Architecture IG does not replace this existing cancer reporting mechanism which works well. The MedMorph central cancer reporting use case is therefore intended to be used primarily by non-hospital providers (e.g., physician offices, oncology clinics, radiation therapy centers, etc.) and by hospitals without cancer registries. 

* For Research: Onboard a new research data partner to join a research network, and subsequently allow those researchers within the network the ability to submit specific research questions and receive responses.

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

	Note: Knowledge artifacts do not contain any PHI or PII information

4. __Backend Service App__:  A system that resides within the clinical care setting and performs the reporting functions to public health and/or research registries. The Backend Service App uses the information supplied by the knowledge artifact repository along with applicable authorities and policies to determine when reporting needs to be done, what data needs to be reported, how the data needs to be reported, and where the data should be reported. The term “Backend Service” is used to refer to the fact that the system does not require user intervention to perform reporting. The term “App” is used to indicate that it is similar to a SMART on FHIR App which can be distributed to clinical care via EHR vendor-specified processes. The EHR vendor-specified processes enable the Backend Service App the ability to use the EHR's FHIR APIs to access data. The healthcare organization is responsible for choosing/developing and maintaining the Backend Service App within the organization.

5. __Trust Service Provider__:  Trust Service Provider affords capabilities that can be used to pseudonymize, anonymize, de-identify, hash, or re-link data that is submitted to public health and/or research organizations. These capabilities are called Trust Services. Trust Services are used, when appropriate, by the Backend Service App.  

6. __Trusted Third Party__: A system (e.g., Health Information Exchange (HIE), Reportable Condition Knowledge Management System (RCKMS)/Association of Public Health Laboratories (APHL) Informatics Messaging Services (AIMS) Platform) at an intermediary organization that serves as a conduit to exchange data between healthcare organizations and an endpoint. Trusted Third Parties perform the intermediary functions (e.g., apply business logic and inform the Reportability Response) using appropriate authorities and policies.

7. __Data Repository__: A system receiving the data from the clinical care system. A Data Repository is used to represent systems such as cancer registries, National Healthcare Survey data stores, surveillance systems, and electronic vital records systems. Data Repositories are actively managed and are used to receive data, store data, and perform analysis as appropriate. These data repositories could be operated or accessed by Public Health Authority (or their designated organizations) or research organizations, with appropriate authorities and policies.

8. __Public Health Authority (PHA)__: A government or a government-designated organization allowed to receive the data from Trusted Third Parties or provider organizations using appropriate authorities and policies. PHA may also analyze the data and initiate responses back to clinical care. For more detailed information on PHA please refer to https://www.hhs.gov/hipaa/for-professionals/special-topics/public-health/index.html. 

9. __Research Organization__: An organization that can access the data from clinical care or data repositories for research purposes with appropriate data use agreements, authorities, and policies.


#### Interactions between MedMorph Actors and Systems

This section outlines the high level interactions between the various MedMorph Actors and Systems defined above. These interactions are shown in the Figure 3.0 below along with the descriptions for each step.

 
 
 {% include img.html img="MedMorphActorsAndSystems.png" caption="Figure 3.0 - MedMorph Actors, Systems and their Interactions" %}


<br>
The descriptions for each step in the above diagram are described below:

* Step 1: A Public Health Agency (PHA) or a Research Organization (RO) creates a Knowledge Artifact (KA) and makes it available via the Knowledge Artifact Repository (KAR).
* Step 1a: KARs, which implement notifications, can optionally notify the subscribers (EHRs, Backend System App, Administrators) of changes in the KAs.
* Step 2: Backend Service App (BSA) queries the KAR to retrieve a KA.
* Step 2a: BSA receives the KA as a response to the query in Step 2.
* Step 3: BSA processes the KA and creates subscriptions in the EHR’s FHIR Server so that it can be notified when specific events occur in clinical workflows.
* Step 4: Providers, as part of their clinical workflows, update the data in the EHR patient chart.
* Step 5: EHR notifies the BSA based on subscriptions that have been created in Step 3.
* Step 6: BSA queries the EHR for patient’s data.
* Step 6a: BSA receives the response from the EHR with the patient’s data.
* Step 7: After the creation of the report with identifiable data that needs to be submitted, the BSA invokes Data/Trust Services (DTS) to de-identify, anonymize, pseudonymize the report as needed (NOTE: DTS are not invoked if the data must remain identifiable, as with most public health use cases).
* Step 7a: BSA receives the de-identified, anonymized or pseudonymized report (only if DTS have been invoked).
* Step 8: The BSA submits the created report in Step 7 to the PHA/RO directly without any Trusted Third Parties (TTPs) acting as intermediaries.
* Step 8a: PHAs/ROs can require TTPs to act as intermediaries to receive reports from clinical organizations. For these scenarios, the BSA submits the created report to the TTP.
* Step 8b: The TTP receives a submitted report from the BSA and forwards the report to the PHA/RO.
* Step 9: The PHA/RO submits a response back to the healthcare organization based on the submitted report. The response transaction can be synchronous (immediately w.r.t. Step 8) or asynchronous (after a period of time).
* Step 9a: The PHA/RO can require TTP to act as intermediaries to submit reports to clinical organizations. For these scenarios, the PHA/RO submits a response to the TTP.
* Step 9b: The TTP receives the submitted response from the PHA/RO and forwards the response to the BSA which is part of the healthcare organization.
* Step 10: The BSA writes back the response from the PHA/Research Organization to the EHR as appropriate. (NOTE: The response may have to be re-identified in some scenarios using Data/Trust Services before it is written back to the EHR.)
 

The next few sections considers subsets of the above the interactions for ease of understanding and documenting detailed interactions using applicable FHIR mechanisms. 


### MedMorph Workflows

The following section outlines each of the MedMorph workflows, actors, and their interactions based on [MedMorph Use Cases](https://carradora.atlassian.net/wiki/spaces/MedMorph/pages/381780019/Use+Case+Work+Groups#Use-Cases). 

#### Provisioning Workflow

The Provisioning Workflow involves the activities whereby a PHA or a Research Organization publishes a Knowledge Artifact(s) specific to a use case. The published Knowledge Artifact(s) is then downloaded by the Backend Service App. The Backend Service App may subscribe to notifications from the EHR based on data present within the Knowledge Artifact. The actors and steps involved in the workflow are shown in Figure 3.1 below.


{% include img.html img="ProvisioningWorkflow.svg" caption="Figure 3.1 - Provisioning Workflow" %}


<br>

#### Notification Workflow

The Notification Workflow involves the activities whereby a provider, as part of the care delivery process, updates a patient's medical record within an EHR. The EHR, based on the changes to the medical record and existing subscriptions, will notify the Backend Service App of the changes in the medical record. The Backend Service App will perform actions such as querying the EHR for additional data, scheduling a reporting job, and creating a report for submission based on the notification and the Knowledge Artifact data. The actors and steps involved in the workflow are shown in Figure 3.2 below.

{% include img.html img="NotificationWorkflow.svg" caption="Figure 3.2 - Notification Workflow" %}

<br>

#### Report Creation Workflow

The Report Creation Workflow involves the activities whereby the Backend Service App collects the data necessary for creating the report to be submitted to the PHA or Research Organization. The Backend Service App may then perform additional activities such as pseudonymization, anonymization or de-identification based on the specific use case and knowledge artifact. Finally, the Backend Service App assembles the data in the required reporting format for submission. The actors and steps involved in the workflow are shown in Figure 3.3 below.

{% include img.html img="ReportCreationWorkflow.svg" caption="Figure 3.3 - Report Creation Workflow" %}

<br>

#### Data Submission Workflow

The Data Submission Workflow involves the activities whereby the Backend Service App packages the data and submits the data to the PHA or Research Organization. The data submission may occur through a Trusted Third Party. The actors and steps involved in the workflow are shown in Figure 3.4 below.


{% include img.html img="DataSubmissionWorkflow.svg" caption="Figure 3.4 - Data Submission Workflow" %}


<br>

#### Receiving Response/Acknowledgement Workflow (PUSH Method)

The Receiving Response/Acknowledgement Workflow involves the activities whereby the PHA or Research Organization provides a response back to clinical care. This workflow depicts the PUSH method (i.e., PUSH from PHA/Research Organization to Clinical Care). The actors and steps involved in the workflow are shown in Figure 3.5 below.

{% include img.html img="ResponseWorkflowPush.svg" caption="Figure 3.5 - Receiving Response/Acknowledgement Workflow (PUSH Method)" %}

<br>

#### Receiving Response/Acknowledgement Workflow (PULL Method)

The Receiving Response/Acknowledgement Workflow involves the activities whereby the PHA or Research Organization provides a response back to clinical care. This workflow depicts the PULL method (i.e., Clinical Care PULLS the response from PHA/Research Organization). The actors and steps involved in the workflow are shown in Figure 3.6 below.


{% include img.html img="ResponseWorkflowPull.svg" caption="Figure 3.6 - Receiving Response/Acknowledgement Workflow (PULL Method)" %}
 
<br>
