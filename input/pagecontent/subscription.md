This section defines the specific requirements related to Subscriptions and Notifications as specified in this MedMorph Reference Architecture (RA) Implementation Guide (IG). The specification focuses on the creation of SubscriptionTopics in the Data Source (e.g., an EHR), subscriptions to the topics by the Health Data Exchange app (HDEA), MedMorph’s backend services app, and notifications from the Data Source to the HDEA.

### Use of Subscription Topics

The MedMorph RA IG intended to use SubscriptionTopics as a mechanism by which Data Sources (e.g EHRs) would generate notifications to trigger processes within a HDEA. Based on the notifications, the HDEA will initiate the reporting workflows based on PlanDefinition resources. 

### Usage of FHIR Subscriptions Backport IG to implement Subscriptions

The MedMorph RA IG intended to leverage the Subscriptions Backport IG to implement Subscriptions and associated notifications. The following are the Subscription Topic URLs that need to be used in the creation of the Subscription. 

#### SubsciptionTopics (Notification Events) and Canonical Urls

This section identifies a list of all the named events that could be used by the Content IGs for notifications and a proposed list of Canonical URLs for each event. These URLs can be used to create Subscriptions in the future and can be used to identify the type of event. The list of the topics are based on the [Named Event Valueset](ValueSet-us-ph-triggerdefinition-namedevent.html) value set.


|Notification/Named Event/Subscription Topic	| Canonical Url 	| ResourceId/ResourceType expected as part of notification	|
| :---										| :---			| :--- 														|
| encounter-start							|http://hl7.org/fhir/us/medmorph/SubscriptionTopic/encounter-start|Encounter|
| encounter-end								|http://hl7.org/fhir/us/medmorph/SubscriptionTopic/encounter-end|Encounter|
| encounter-modified							|http://hl7.org/fhir/us/medmorph/SubscriptionTopic/encounter-modified|Encounter|
| new-diagnosis								|http://hl7.org/fhir/us/medmorph/SubscriptionTopic/new-diagnosis|Condition|
| modified-diagnosis							|http://hl7.org/fhir/us/medmorph/SubscriptionTopic/modified-diagnosis|Condition|
| new-medication								|http://hl7.org/fhir/us/medmorph/SubscriptionTopic/new-medication|MedicationRequest|
| modified-medication								|http://hl7.org/fhir/us/medmorph/SubscriptionTopic/modified-medication|MedicationRequest|
| new-labresult								|http://hl7.org/fhir/us/medmorph/SubscriptionTopic/new-labresult|Observation|
| modified-labresult								|http://hl7.org/fhir/us/medmorph/SubscriptionTopic/modified-labresult|Observation|
| new-order								|http://hl7.org/fhir/us/medmorph/SubscriptionTopic/new-order|ServiceRequest|
| modified-order								|http://hl7.org/fhir/us/medmorph/SubscriptionTopic/modified-order|ServiceRequest|
| new-procedure								|http://hl7.org/fhir/us/medmorph/SubscriptionTopic/new-procedure|Procedure|
| modified-procedure							|http://hl7.org/fhir/us/medmorph/SubscriptionTopic/modified-procedure|Procedure|
| new-immunization								|http://hl7.org/fhir/us/medmorph/SubscriptionTopic/new-immunization|Immunization|
| modified-immunization							|http://hl7.org/fhir/us/medmorph/SubscriptionTopic/modified-immunization|Immunization|
| demographic-change							|http://hl7.org/fhir/us/medmorph/SubscriptionTopic/demographic-change|Patient|
   
The following are examples of Subscriptions for encounter-start and encounter-end named events.

* [encounter-start Subscription](Subscription-encounter-start-subscription-example.html)

* [encounter-end Subscription](Subscription-encounter-end-subscription-example.html)


#### HDEA Requirements for Subscription Creation

* When interacting with Data Sources that support Subscriptions, the HDEA SHOULD create a Subscription resource for each named event identified in the Knowledge Artifact being processed. 

* The HDEA SHALL represent the Canonical URLs from the above table as part of the Subscription.criteria element.

* HDEA SHALL implement a rest-hook channel to receive notifications from Data Sources.

* HDEA SHALL start the reporting workflow processing using Knowledge Artifact instances which contain the named event identified by the Subscription.criteria URL.     

* HDEA SHALL update the Subscriptions and turn them off, when the Knowledge Artifact is not being processed. 

#### Data Source Requirements for Subscriptions

* Data Sources SHOULD support Subscriptions for encounter-start and encounter-end named events.

* Data Sources MAY support Subscriptions for each of the named event identified above. 

* Each MedMorph content IG SHALL identify the specific named-event required to be supported by the Data Source for the use case.

* Data Sources SHOULD support the channel of type rest-hook to notify the subscribers of changes.

* Data Sources SHOULD support a notification payload type of id-only where the id of the changed resource is included.

* Data Sources MAY include in the notification the resources changed along with context information. 

* Data Sources SHOULD consider including context information such as the patient and the encounter resources as part of the notification.


### Implementing Notifications without using FHIR Subscriptions 

There are situations where the Data Sources have not implemented the Subscriptions capability. To enable implementation of MedMorph use cases in these situations, the MedMorph RA prescribes a RESTful API to enable the necessary notifications. 
The HDEA should support a RESTful API which can be invoked by a Data Source for notification. The RESTful API interaction which is similar to the ```rest-hook``` in Subscriptions IG is described below:

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

