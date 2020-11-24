This section of the implementation guide defines the specific requirements for submitting reports that are created by the Backend Service App.

### MedMorph Reports

Reports that are created by the Backend Service App are intended to contain all the resource instances necessary to be send to the PHA/Research Organizations. The following are specific requirements for report creation and submission.

#### Backend Service App Requirements for Messaging

##### Message Submission

* The Backend Service App SHALL create reports to be sent to the PHA/RO in form of a Bundle of type 'message' as indicated by the profile [MedMorph Report Bundle](StructureDefinition-medmorph-bundle.html).

* The Backend Service App SHALL include the MessageHeader resource as the first entry in the Bundle. 

* The Backend Service App SHALL copy the receiver address from the PlanDefintion.ext-receiversAddress extension element into the MessageHeader.destination.endpoint element.

* The Backend Service App SHALL populate the sender information as a reference to the Organization resource representing the healthcare organization.

* The Backend Service App SHALL populate the sender's FHIR endpoint in the MessageHeader.source.endpoint element.

* The Backend Service App SHALL populate the Bundle.identifier element as the persistent business identifier for coorelating responses. 

* The Backend Service App SHALL populate the Bundle.timestamp element with the bundle creation time. 

* The Backend Service App SHALL validate the bundle created when required as defined by the PlanDefinition.action requirements.

* The Backend Service App SHALL encrypt the bundle created based on the PlanDefinition.action requirements. All Resource entries within the Bundle except the MessageHeader resource SHALL be encrypted when specified as part of the actions required in the PlanDefinition. 

* The Backend Service App SHALL submit the bundle by invoking the [Receiver's Address]/$process-message operation.

* When submitting the bundle via Trusted Third Party, The Backend Service App SHALL submit the bundle by invoking the [Trusted Third Party FHIR Endpoint]/$process-message operation.

* When submitting the bundle directly to the PHA or Research Organization, The Backend Service App SHALL submit the bundle by invoking the [PHA or Research Organization FHIR Endpoint]/$process-message operation.

##### Message Receiving

* When responses are delivered synchronously by the TTP/PHA or Research Organization, the Backend Service App SHALL process the response message. 

* In order to receive asynchronous responses from TTP or PHA or Research Organization, the Backend Service App SHALL implement the $process-message operation on the ROOT URL of the FHIR Server.

* Upon receipt of either synchronous or asynchronous response messages, the Backend Service App SHALL decrpypt the message when required. 

* Upon receipt of either synchronous or asynchronous response messages, the Backend Servie App SHALL validate the message before accepting the message.

* The Backend Service App SHALL track message submissions with the message responses.

* For each message submission, The Backend Service App SHALL provide the ability for an administrator or the provider to view the message submissions and message responses received. 

##### APIs Used by Backend Service App to submit messages to TTP or PHA/Research Organization

```
For Message Submission to PHA/Research Organization directly:
POST [PHA or Research Organization FHIR Endpoint]/$process-message when submitting directly to the PHA or Research Organization.
Synchronous responses are received as part of the POST response.

For Message submission via TTP:
POST [Trusted Third Party FHIR Endpoint]/$process-message when submitting via Trusted Third Party FHIR endpoint.
Synchronous responses are received as part of the POST response.
```

##### APIs Supported by Backend Service App to receive asynchronous response messages to TTP or PHA/Research Organization

```
POST [Backend Service App Fhir Endpoint]/$process-message for receiving asynchronous message responses. 
```

##### APIs Supported by Backend Service App to view responses for messages submitted

```
```

#### Message Forwarding Requirements for Trusted Third Party (TTP)

* The TTP SHALL implement the $process-message operation on the ROOT URL of the FHIR Server to receive reports from the Backend Service App using the POST operation.

* TTP SHALL use the MessageHeader.destination.endpoint to route the message to the final destination. 

* When forwarding a message from heathcare organization to PHA or Research Organization, TTP SHALL forward the message to the final destination by invoking the [PHA or Research Organization FHIR Endpoint]/$process-message operation.

##### Synchronous Message Forwarding Requirements

* TTP SHALL forward the message to the final destination in a synchronous manner. 

* When a synchronous response is received from the receiver, the TTP SHALL return the response to the sender as part of the synchronous transaction.

##### Asynchronous Message Forwarding Requirements 

* When a PHA/Research Organization submits an asynchronous response back to the healthcare organization, the Trusted Third Party SHALL forward the message to the healthcare organization using the MessageHeader.destination.endpoint address. This is nothing but the healthcare organization (Sender's) FHIR Endpoint.

* In order to forward the response message to the healthcare organization, the TTP SHALL invoke the [Backend Service App FHIR Endpoint]/$process-message operation including the response message data sent by the PHA/Research Organization. 

##### APIs used by Trusted Third Party for Message Forwarding  

```
POST [PHA or Research Organization FHIR Endpoint]/$process-message when forwarding a message from Backend Service App to the PHA or Research Organization. 
Synchronous responses get forwarded back to the healthcare organization as part of the POST response.
 
For Asychronous responses, the TTP has to make a seperate API call as identified below.
POST [Backend Service App FHIR Endpoint]/$process-message when forwarding a message from PHA or Research Organization to the Backend Service App.
```

#### PHA/Research Organization Requirements for Receiving Messages from TTP or Backend Service App

* The PHA/Research Organization SHALL implement the $process-message operation on the ROOT URL of the FHIR Server to receive reports from the Backend Service App or TTP using the POST operation.

* Upon receipt of the message, the PHA/Research Organization SHALL decrpypt the message when required. 

* Upon receipt of the message, the PHA/Research Organization SHALL validate the message before accepting the message.

##### Synchronous Responses from the PHA/Research Organization

* When there are validation failures, the PHA/Research Organization SHALL return a message bundle back with the details of the failures as part of the POST response.

##### Asynchronous Responses from the PHA/Research Organization

* When the PHA/Research Organization sends an asynchronous message back to the healthcare organization as a reply-to an orginal message, the PHA/Research Organization SHALL 
	- populate the MessageHeader.response.identifier with the original message identifier 
	- populate the MessageHeader.destination.endpoint with the Sender's FHIR Endpoint.
	- populate the MessageHeader.sender with the PHA/Research Organization reference.
	- populate the MessageHeader.source.endpoint with the PHA/Research Organization FHIR endpoint.


##### APIs supported by PHA/Research Organization to receive a message from Backend Service App or Trusted Third Party 

```
POST [PHA or Research Organization FHIR Endpoint]/$process-message when receiving a message from Backend Service App or TTP. 
For synchronous responses, the data is sent back as part of the POST response.
```

##### APIs used by PHA/Resarch Organization to submit a message to Backend Service App or Trusted Third Party 

```
These APIs are used for sending asynchronous responses back to the healthcare organization directly or via the TTP.

POST [Backend Service App FHIR Endpoint]/$process-message  for submitting a response directly to the Backend Service App (Healthcare Organization)

POST [Trusted Third Party FHIR Endpoint]/$process-message  for submitting a response via Trusted Third Party FHIR Endpoint.
```