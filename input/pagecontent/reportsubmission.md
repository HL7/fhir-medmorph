This section defines the specific requirements for submitting reports that are created by the Health Data Exchange App (HDEA), MedMorph’s backend services app, as specified in this MedMorph Reference Architecture (RA) Implementation Guide (IG).

### MedMorph Reports

Reports created by the HDEA contain all the resource instances (data) necessary for sending to the Data Receiver (e.g., Public Health Authority (PHA)/Research Organization (RO)). The specific resources (data) to be contained in the report will be defined by the content IG specific to the use case. The following are specific framework requirements for report submission across all use cases.

#### Backend Service App Requirements for Messaging

##### Message Submission

* The HDEA SHALL create reports to be sent to the Data Receiver in form of a Bundle of type 'message' as indicated by the profile [MedMorph Reporting Bundle](StructureDefinition-us-ph-reporting-bundle.html).

* The HDEA SHALL include the MessageHeader resource as the first entry in the Bundle. 

* The HDEA SHALL copy the receiver address from the Knowledge Artifact's PlanDefintion.ext-receiversAddress extension element into the MessageHeader.destination.endpoint element.

* The HDEA SHALL populate the sender information as a reference to the Organization resource representing the Healthcare Organization.

* The HDEA SHALL populate the sender's FHIR endpoint in the MessageHeader.source.endpoint element.

* The HDEA SHALL populate the Bundle.identifier element as the persistent business identifier for correlating responses. 

* The HDEA SHALL populate the Bundle.timestamp element with the bundle creation time. 

* The HDEA SHALL encrypt the bundle created based on the PlanDefinition.action requirements. All Resource entries within the Bundle except the MessageHeader resource SHALL be encrypted when specified as part of the actions required in the PlanDefinition. 

* The HDEA SHALL submit the bundle by invoking the [Data Receiver's FHIR Endpoint]/$process-message operation.

* When submitting the bundle via Trusted Third Party(TTP), The HDEA SHALL submit the bundle by invoking the [Trusted Third Party FHIR Endpoint]/$process-message operation.


##### Message Receiving

* When responses are delivered synchronously by the TTP/Data Receiver, the HDEA SHALL process the response message. 

* In order to receive asynchronous responses from TTP or Data Receiver, the HDEA SHALL implement the $process-message operation on the ROOT URL of the FHIR Server.

* Upon receipt of either synchronous or asynchronous response messages, the HDEA SHALL decrypt the message when required. 

* Upon receipt of either synchronous or asynchronous response messages, the HDEA SHALL validate the message before accepting the message.

* The HDEA SHALL track message submissions with the message responses.

* For each message submission, The HDEA SHALL provide the ability for an administrator or the provider to view the message submissions and message responses received. 


##### APIs Used by HDEA to Submit Messages to TTP or Data Receiver

```
For Message Submission to Data Receiver directly:
POST [Data Receiver FHIR Endpoint]/$process-message when submitting directly to the Data Receiver.
Synchronous responses are received as part of the POST response.

For Message submission via TTP:
POST [TTP FHIR Endpoint]/$process-message when submitting via TTP FHIR endpoint.
Synchronous responses are received as part of the POST response.
```

##### APIs Supported by HDEA to Receive Asynchronous Response Messages 

```
POST [HDEA FHIR Endpoint]/$process-message for receiving asynchronous message responses. 
```

#### Message Forwarding Requirements for TTP

* The TTP SHALL implement the $process-message operation on the ROOT URL of the FHIR Server to receive reports from the HDEA using the POST operation.

* TTP SHALL use the MessageHeader.destination.endpoint to route the message to the final destination. 

* When forwarding a message from health care organization to Data Receiver, the TTP SHALL forward the message to the final destination by invoking the [Data Receiver FHIR Endpoint]/$process-message operation.

##### Synchronous Message Forwarding Requirements

* TTP SHALL forward the message to the final destination in a synchronous manner. 

* When a synchronous response is received from the receiver, the TTP SHALL return the response to the sender as part of the synchronous transaction.

##### Asynchronous Message Forwarding Requirements 

* When a Data Receiver submits an asynchronous response back to the health care organization, the TTP SHALL forward the message to the health care organization using the MessageHeader.destination.endpoint address. This is nothing but the health care organization (Sender's) FHIR Endpoint.


##### APIs used by Trusted Third Party for Message Forwarding  

```
POST [Data Receiver FHIR Endpoint]/$process-message when forwarding a message from HDEA to the Data Receiver. 
Synchronous responses get forwarded back to the healthcare organization as part of the POST response.
 
For Asynchronous responses, the TTP has to make a separate API call as identified below.
POST [HDEA FHIR Endpoint]/$process-message when forwarding a message from Data Receiver to the HDEA.
```

#### Data Receiver Requirements for Receiving Messages from TTP or HDEA

* The Data Receiver SHALL implement the $process-message operation on the ROOT URL of the FHIR Server to receive reports from the HDEA or TTP using the POST operation.

* Upon receipt of the message, the Data Receiver SHALL decrypt the message when required. 

* Upon receipt of the message, the Data Receiver SHALL validate the message before accepting the message.

##### Synchronous Responses from the Data Receiver

* When there are validation failures, the Data Receiver SHALL return a message bundle back with the details of the failures as part of the POST response.

##### Asynchronous Responses from the Data Receiver

* When the Data Receiver sends an asynchronous message back to the Healthcare Organization as a reply-to an original message, the Data Receiver SHALL 
	- populate the MessageHeader.response.identifier with the original message identifier 
	- populate the MessageHeader.destination.endpoint with the Sender's FHIR Endpoint.
	- populate the MessageHeader.sender with the Data Receiver reference.
	- populate the MessageHeader.source.endpoint with the Data Receiver FHIR endpoint.


##### APIs Supported by Data Receiver to Receive a Message from HDEA or TTP 

```
POST [Data Receiver FHIR Endpoint]/$process-message when receiving a message from HDEA or TTP. 
For synchronous responses, the data is sent back as part of the POST response.
```

##### APIs Used by Data Receiver to Submit a Message to HDEA or TTP

These APIs are used for sending asynchronous responses back to the Healthcare Organization directly or via the TTP.

```

POST [HDEA FHIR Endpoint]/$process-message for submitting a response directly to the HDEA (Healthcare Organization)

POST [TTP FHIR Endpoint]/$process-message for submitting a response via TTP FHIR Endpoint.
```
