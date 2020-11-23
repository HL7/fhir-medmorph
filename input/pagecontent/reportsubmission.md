This section of the implementation guide defines the specific requirements for submitting reports that are created by the Backend Service App.

### MedMorph Reports

Reports that are created by the Backend Service App are intended to contain all the resource instances necessary to be sent to the PHA/Research Organizations. The following are specific requirements for report creation and submission.

#### Backend Service App Requirements for Report Submission

* The Backend Service App SHALL create reports to be sent to the PHA/RO in form of a Bundle of type 'message' as indicated by the profile [MedMorph Report Bundle](StructureDefinition-medmorph-bundle.html).

* The Backend Service App SHALL include the MessageHeader resource as the first entry in the Bundle. 

* The Backend Service App SHALL copy the receiver address from the PlanDefintion.ext-receiversAddress extension element into the MessageHeader.destination.endpoint element.

* The Backend Service App SHALL populate the sender information as a reference to the Organization resource representing the healthcare organization.

* The Backend Service App SHALL populate the sender's FHIR endpoint in the MessageHeader.source.endpoint element.

* The Backend Service App SHALL populate the Bundle.identifier element as the persistent business identifier for correlating responses. 

* The Backend Service App SHALL populate the Bundle.timestamp element with the bundle creation time. 

* The Backend Service App SHALL validate the bundle created when required as defined by the PlanDefinition.action requirements.

* The Backend Service App SHALL encrypt the bundle created based on the PlanDefinition.action requirements. All Resource entries within the Bundle except the MessageHeader resource SHALL be encrypted when desired. 

* The Backend Service App SHALL submit the bundle by invoking the [Receiver's Address]/$process-message operation.

* When submitting the bundle via Trusted Third Party, The Backend Service App SHALL submit the bundle by invoking the [Trusted Third Party FHIR Endpoint]/$process-message operation.

#### APIs for Backend Service App Message Submission 

```
POST [Receivers FHIR Endpoint]/$process-message when submitting directly to the receivers FHIR endpoint.

POST [Trusted Third Party FHIR Endpoint]/$process-message when submitting via Trusted Third Party FHIR endpoint.
```

#### Message Forwarding Requirements for Trusted Third Party (TTP)

* The TTP SHALL implement the $process-message operation on the ROOT URL of the FHIR Server to receive reports from the Backend Service App using the POST operation.

* TTP SHALL use the MessageHeader.destination.endpoint to route the message to the final destination. 

* TTP SHALL forward the message to the final destination in a synchronous manner. 

* When a synchronous response is received from the receiver, the TTP SHALL return the response to the sender as part of the synchronous transaction.

* TTP SHALL forward the message to the final destination by invoking the [Receiver's Address]/$process-message operation.

* When a PHA/Research Organization submits an asynchronous response back to the healthcare organization, the Trusted Third Party SHALL forward the message to the healthcare organization using the MessageHeader.destination.endpoint address. This is nothing but the Sender's FHIR Endpoint address.

* To forward the response message to the healthcare organization, the TTP SHALL invoke the [Sender's FHIR Endpoint]/$process-message operation including the response message data sent by the PHA/Research Organization. 

#### APIs for Trusted Third Party Message Forwarding 

```
POST [Receiver's FHIR Endpoint]/$process-message when forwarding to the receivers FHIR endpoint.

POST [Sender's FHIR Endpoint]/$process-message when forwarding to the senders FHIR endpoint.
```

#### Message Receiving Requirements for PHA/Research Organization

* The PHA/Research Organization SHALL implement the $process-message operation on the ROOT URL of the FHIR Server to receive reports from the Backend Service App or TTP using the POST operation.

* Upon receipt of the message, the PHA/Research Organization SHALL decrypt the message when required. 

* Upon receipt of the message, the PHA/Research Organization SHALL validate the message before accepting the message.

* When there are validation failures, the PHA/Research Organization SHALL return a message bundle back with the details of the failures.

* When the PHA/Research Organization sends an asynchronous message back to the healthcare organization as a reply-to an original message, the PHA/Research Organization SHALL 
- populate the MessageHeader.response.identifier with the original message identifier.
- populate the MessageHeader.destination.endpoint with the Sender's FHIR Endpoint.
- populate the MessageHeader.sender with the PHA/Research Organization reference.
- populate the MessageHeader.source.endpoint with the PHA/Research Organization FHIR endpoint.



#### APIs for Backend Service App Message Submission 

```
POST [Sender's FHIR Endpoint]/$process-message when submitting a response to the sender's FHIR endpoint directly.

POST [Trusted Third Party FHIR Endpoint]/$process-message when submitting a response via Trusted Third Party FHIR endpoint.
```
