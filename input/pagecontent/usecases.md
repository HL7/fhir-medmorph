### Business need

* Identify and collect hospital (emergency department and inpatient care) and ambulatory care data and electronically report them to a system hosted at the federal level. Electronic reporting will increase the response rate of sampled hospitals and ambulatory health care providers to the National Hospital Care Survey (NHCS) and the National Ambulatory Medical Care Survey (NAMCS). This will also increase the volume, quality, completeness and timeliness of the data submitted to the NHCS and NAMCS. Electronic reporting via automated means (without provider involvement) will reduce the burned associated with survey participation, reduce costs associated with recruiting hospital and ambulatory health care providers.

* Leverage existing FHIR infrastructure including electronic case reporting to improve the availability of data that advance national priorities such as eliminating hepatitis C as a public threat in the US. The goal is to optimize access to hepatitis C data that is already capatured within the EHR.

* Leverage existing FHIR infrastructure to transmit cancer case information to state Central Cancer registries. The intent is to automate and provide access to data not currently available to public health and research. This will not replace hospital registry reporting methods that are working well. Automation of cancer case information reporting will reduce the data collection burden that exists in current manual processes.  

* Onboard a new research data partner to join a research network, and subsequently allow researchers to submit specific research questions and receive responses.

Leveraging FHIR based APIs, Public Health and Research organizations intend to automate the data collection based on specific events and conditions in clinical workflows and submit the collected data complying with the necessary authorities and policies applicable for the jurisdictions. 


### MedMorph Actors and Definitions

This section defines the Actors and Systems that would be interacting to realize the capabilities of the above use cases.

1. __Electronic Health Record (EHR)__:  A system used in care delivery for patients and captures and stores data about patients and makes the information available instantly and securely to authorized users. While an EHR does contain the medical and treatment histories of patients, an EHR system is built to go beyond standard clinical data collected in a provider’s provision of care location and can be inclusive of a broader view of a patient’s care. EHRs are a vital part of health IT and can:

	* Contain a patient’s medical history, diagnoses, medications, treatment plans, immunization dates, allergies, radiology images, and laboratory and test results
	* Allow access to evidence-based tools that providers can use to make decisions about a patient’s care
	* Automate and streamline provider workflow

	A **FHIR Enabled EHR** exposes FHIR APIs for other systems to interact with the EHR and exchange data. FHIR APIs provide well defined mechanisms to read and write data. The 	FHIR APIs are protected by an Authorization Server which authenticates and authorizes users or systems prior to accessing the data.

2. __Provider__:  A person who is participating in the care delivery process for a patient. In addition to delivering care, the Provider also updates the EHR with the patient data captured during the workflow and signs off on the content of the patient record once it is updated. The Provider can be the individual doctor, nurse, admin staff or a combination of different people.

3. __Knowledge Artifact Repository__:  Enables the full spectrum of reporting and querying for programs, studies, and initiatives. Knowledge Artifact Repository represents the system that is used to distribute metadata such as surveys, trigger codes, timing constraints, decision support artifacts, knowledge artifacts to:

	* Identify if reporting needs to be performed for a patient
	* Identify the timing parameters for the reporting
	* Identify the data that needs to be collected for reporting 
	* Identify the structure of the report
	* Identify decision support rules for reporting

	Note: Knowledge artifacts do not contain any PHI or PII information.

4. __Backend Service App__:  A system that resides within the clinical care setting and performs the reporting functions to public health and/or research registries. The system uses the information supplied by the knowledge artifact repository to determine when reporting needs to be done, what data needs to be reported, how the data needs to be reported, and to whom the data should be reported. The term “Backend Service” is used to refer to the fact that the system does not require user intervention to perform reporting. The term “App” is used to indicate that it is similar to SMART on FHIR App which can be distributed to clinical care via EHR vendor specified processes. The EHR vendor specified processes are followed to enable the Backend Service App to use the EHR's FHIR APIs to access data. The healthcare organization is the one who is responsible for choosing and maintaining the Backend Service App within the organization.

5. __Trust Service Provider__:  Trust Service Provider provides capabilities that can be used to pseudonymize, anonymize, de-identify, hash, or re-link data that is submitted to public health and/or research organization. These capabilities are called as Trust Services. Trust Services are used when required appropriate by the Backend Service App.  

6. __Trusted Third Party__: A system (e.g., HIE, RCKMS/AIMS Platform) at an intermediary organization that serves as a conduit to exchange data between healthcare organizations and PHAs. Trusted Third Parties perform the intermediary functions (e.g., apply business logic and informs the Reportability Response) using appropriate authorities and policies.

7. __Data Repository__: A system that is receiving the data from the clinical care. Data Repository is used to represent systems such as a cancer registry, National Healthcare Survey data store, surveillance systems, and electronic vital records systems. Data Repositories are actively managed and are used to receive data, store data, and perform analysis as appropriate. These data repositories could be operated or accessed by PHA (or their designated organizations), research organizations with appropriate authorities and policies.

8. __Public Health Authority (PHA)__: A government or a government designated organization that may receive the data from Trusted Third Parties or provider organizations using appropriate authorities and policies. PHA may also analyze the data and initiate responses back to clinical care. For more detailed information on PHA please refer to https://www.hhs.gov/hipaa/for-professionals/special-topics/public-health/index.html. 

9. __Research Organization__: An organization that can access the data from clinical care or data repositories for research purposes with appropriate data use agreements, authorities, and policies.


### MedMorph Workflows

The following section outlines each of the MedMorph workflows, actors and their interactions in details. 

#### Provisioning Workflow

The provisioning workflow involves the activities where by a PHA or a Research Organization published a Knowledge Artifact specific to a use case. The published Knowledge Artifact is then downloaded by the Backend Service App. The Backend Service App may subscribe to notifications from the EHR based on data present within the Knowledge Artifact. The actors and steps involved in the workflow are as shown below.

{::options parse_block_html="false" /}

<figure>
  <img height="600px" src="ProvisioningWorkflow.png" alt="Figure 2: Provisioning Workflow " style="vertical-align:middle"/>
</figure>

{::options parse_block_html="true" /}


#### Notification Workflow

The notification workflow involves the activities where by a Provider as part of care delivery process updates a Patient's medical record within an EHR. The EHR based on the changes to the medical record and existing subscriptions will notify the Backend Service App of the changes in the medical record. The Backend Service App will perform actions such as querying the EHR for additional data, scheduling a reporting job, creating a report for submission based on the notification and the Knowledge Artifact data. The actors and steps involved in the workflow are as shown below.

 {::options parse_block_html="false" /}

<figure>
  <img height="600px" src="NotificationWorkflow.png" alt="Figure 3: Notification Workflow" style="vertical-align:middle"/>
</figure>

{::options parse_block_html="true" /}

#### Report Creation Workflow

The Report Creation workflow involves the activities where by the Backend Service App collects the data necessary for creating the report to be submitted to the PHA or Research Organization. The Backend Service App may then perform additional activities such as Psuedonymization, Anonymization or de-identification based on specific use case and knowledge artifact. Finally the Backend Service App assembles the data in the required reporting format for submission. The actors and steps involved in the workflow are as shown below.

 {::options parse_block_html="false" /}

<figure>
  <img height="600px" src="ReportCreationWorkflow.png" alt="Figure 4: Report Creation Workflow" style="vertical-align:middle"/>
</figure>

{::options parse_block_html="true" /}


#### Data Submission Workflow

The Data Submission Workflow involves the activities where by the Backend Service App packages the data and submits the data to the PHA or Research Organization. The data submission may occur through a Trusted Third Party as shown in the figure below. The actors and steps involved in the workflow are as shown below.

 {::options parse_block_html="false" /}

<figure>
  <img height="600px" src="DataSubmissionWorkflow.png" alt="Figure 5: Data Submission Workflow" style="vertical-align:middle"/>
</figure>

{::options parse_block_html="true" /}


#### Receiving Response/Acknowledgement Workflow (PUSH METHOD)

The Receiving Response/Acknowledgement Workflow involves the activities where by the PHA or Research Organization provides a response back to the clinical care. In this section the PUSH method (PUSH from PHA/Research Organization to Clinical Care) is being shown. The actors and steps involved in the workflow are as shown below.

 {::options parse_block_html="false" /}

<figure>
  <img height="600px" src="ResponseWorkflowPush.png" alt="Figure 6: Receiving Responses/Acknowledgements PUSH Workflow" style="vertical-align:middle"/>
</figure>

{::options parse_block_html="true" /}


#### Receiving Response/Acknowledgement Workflow (PULL METHOD)

The Receiving Response/Acknowledgement Workflow involves the activities where by the PHA or Research Organization provides a response back to the clinical care. In this section the PULL method (Clinical Care PULLS the response from PHA/Research Organization) is being shown. The actors and steps involved in the workflow are as shown below.

 {::options parse_block_html="false" /}

<figure>
  <img height="600px" src="ResponseWorkflowPull.png" alt="Figure 7: Receiving Responses/Acknowledgements PULL Workflow" style="vertical-align:middle"/>
</figure>

{::options parse_block_html="true" /}
 