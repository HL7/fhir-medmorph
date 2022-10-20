This section defines the specific requirements related to Subscriptions and Notifications as specified in this MedMorph Reference Architecture (RA) Implementation Guide (IG). The specification focuses on the creation of SubscriptionTopics in the Data Source (e.g., an EHR), subscriptions to the topics by the Health Data Exchange app (HDEA), MedMorph’s backend services app, and notifications from the Data Source to the HDEA.

### Use of Subscription Topics

The MedMorph RA IG intended to use SubscriptionTopics as a mechanism by which Data Sources (e.g EHRs) would generate notifications to trigger processes within a HDEA. Based on the notifications, the HDEA will initiate the reporting workflows based on PlanDefinition resources. 

### Usage of FHIR Subscriptions Backport IG to implement Subscriptions

The MedMorph RA IG intended to leverage the Subscriptions Backport IG to implement SubscriptionTopics and associated notifications. However currently the Subscriptions Backport IG is based on FHIR Release 4.3.0 which is FHIR Release R4B sequence and is not compatible with the MedMorph RA IG which is based on FHIR Release 4.0.1 which is FHIR Release R4 sequence. MedMorph RA IG cannot be advanced to Release 4.3.0 as it is dependent on US Core and Bulk Data Access IG versions which are referenced in the ONC 21st Century Cures Act regulation.

This topic was discussed at the FMG and it has been noted that a FHIR Subscriptions Backport IG based on version 4.0.1 would potentially be created in due time. Until the time a Subscriptions Backport IG compatible with FHIR 4.0.1 is made available, the MedMorph RA IG **will not reference** a specific version of the Subscriptions Backport IG and not require any specific conformance requirements for the Subscription implementation. 

### Implementing Notifications without using FHIR Subscriptions 

FHIR Subscriptions capability is not widely implemented by Data Source systems currently. This is partly because of the above version compatibility issues, However the MedMorph RA IG needs to define a mechanism for notification until the Subscriptions Backport IG compatible with FHIR Release 4.0.1 is released. 

In order to ensure that the MedMorph RA IG can be implemented, the HDEA should support a RESTful API which can be invoked by a Data Source for notification. The RESTful API interaction which is similar to the ```rest-hook``` in Subscriptions IG is described below:

```
POST [HDEA APP URL]/receive-notification
	patientId
	resourceId
	resourceType
	topicUrl
```

The API is a POST interaction which takes the following parameters as part of the POST body:


* patientId -  the Id of the Patient FHIR resource whose data is changed
* resourceId - the exact resource which was changed (for e.g, Condition resource Id if a new condition was added or an existing condition was changed)
* resourceType - the FHIR ResourceType identifying for the resourceId provided
* topicUrl - the canonical URL for the various Subscription topics, that helps the HDEA to identify the specific type of notification. 

The above API is a recommendation and implementers of HDEA and Data Source actors can agree upon a variation of the above interface for notification, including implementing Subscription notification mechanisms identified in the Subscriptions IG.  

### SubsciptionTopics (Notification Events) and Canonical Urls

This section identifies a list of all the notification events that could be used by the Content IGs for notifications and a proposed list of Canonical URLs for each event. These URLs can be used to create SubscriptionTopics in the future and can be used to identify the type of event for the API outlined in the previous section. The list of the topics are based on the [Named Event Valueset](ValueSet-us-ph-triggerdefinition-namedevent.html) value set.


|Notification/Named Event/Subscription Topic	| Canonical Url 	| ResourceId/ResourceType expected as part of notification	|
| :---										| :---			| :--- 														|
| encounter-start							|http://hl7.org/fhir/us/medmorph/StructureDefinition/encounter-start|Encounter|
| encounter-end								|http://hl7.org/fhir/us/medmorph/StructureDefinition/encounter-end|Encounter|
| encounter-modified							|http://hl7.org/fhir/us/medmorph/StructureDefinition/encounter-modified|Encounter|
| new-diagnosis								|http://hl7.org/fhir/us/medmorph/StructureDefinition/new-diagnosis|Condition|
| modified-diagnosis							|http://hl7.org/fhir/us/medmorph/StructureDefinition/modified-diagnosis|Condition|
| new-medication								|http://hl7.org/fhir/us/medmorph/StructureDefinition/new-medication|MedicationRequest|
| modified-medication								|http://hl7.org/fhir/us/medmorph/StructureDefinition/modified-medication|MedicationRequest|
| new-labresult								|http://hl7.org/fhir/us/medmorph/StructureDefinition/new-labresult|Observation|
| modified-labresult								|http://hl7.org/fhir/us/medmorph/StructureDefinition/modified-labresult|Observation|
| new-order								|http://hl7.org/fhir/us/medmorph/StructureDefinition/new-order|ServiceRequest|
| modified-order								|http://hl7.org/fhir/us/medmorph/StructureDefinition/modified-order|ServiceRequest|
| new-procedure								|http://hl7.org/fhir/us/medmorph/StructureDefinition/new-procedure|Procedure|
| modified-procedure							|http://hl7.org/fhir/us/medmorph/StructureDefinition/modified-procedure|Procedure|
| new-immunization								|http://hl7.org/fhir/us/medmorph/StructureDefinition/new-immunization|Immunization|
| modified-immunization							|http://hl7.org/fhir/us/medmorph/StructureDefinition/modified-immunization|Immunization|
| demographic-change							|http://hl7.org/fhir/us/medmorph/StructureDefinition/demographic-change|Patient|


#### Subscriptions and Notifications Using SubscriptionTopic

This section in the future will identify specific conformance requirements for Data Sources based on Subscriptions Backport IG when the version compatible with FHIR Release 4.0.1 is available.

**Note**: Some Examples of the Subscription Topics are available on the [FHIR Artifacts](artifacts.html) examples section. Content IGs will define specific Subscription Topics required for the use case in the future.
