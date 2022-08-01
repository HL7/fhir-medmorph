This section defines the specific requirements related to subscriptions and notifications as specified in this MedMorph Reference Architecture IG.  The specification focuses on the creation of SubscriptionTopics in the EHR, Subscriptions to the topics by the Backend Service App, and notifications from the EHR to the Backend Service App.

### MedMorph Subscription Topics

The MedMorph Reference Architecture IG specifies that EHRs or other systems need to generate notifications based on subscriptions to trigger processes within a Backend Service App. Based on the notifications, the Backend Service App will initiate the reporting workflows based on PlanDefinition resources.

#### Subscriptions and Notifications Using SubscriptionTopic

The following requirements are based on SubscriptionTopics defined as per the Subscriptions R5 Backport IG.

* EHRs SHOULD support the SubscriptionTopics identified by each of the named-events in the [Named Event Valueset](ValueSet-us-ph-triggerdefinition-namedevent.html). Each MedMorph Content IG should identify the specific named-event required to be supported by the EHR for the use case.   

* EHRs SHOULD support the channel of type ```rest-hook``` to notify the subscribers of changes.

* EHRs SHOULD support a notification payload type of ```id-only``` where the id of the changed resource is included.   

* EHRs MAY include in the notification the resources that were changed along with context information. EHRs SHOULD consider including context information such as the patient and the encounter resources. The MedMorph Content IGs will specify the detailed notifications required as per the use case.

* Backend Service App SHALL subscribe to the EHR SubscriptionTopics identified by each of the named-events in the [Named Event Valueset](	ValueSet-us-ph-triggerdefinition-namedevent.html).

* Backend Service App SHALL implement a rest-hook channel to receive notifications from EHR Subscriptions.

* Backend Service App SHALL start the reporting workflow processing using appropriate PlanDefinition instances based on the notification received from the EHR.

Note: Examples of the Subscription Topics are available on the [FHIR Artifacts](artifacts.html) examples section. Content IGs will define specific Subscription Topics required for the use case.
