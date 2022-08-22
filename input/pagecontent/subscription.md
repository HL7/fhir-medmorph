This section defines the specific requirements related to Subscriptions and Notifications as specified in this MedMorph Reference Architecture (RA) Implementation Guide (IG). The specification focuses on the creation of SubscriptionTopics in the Data Source (e.g., an EHR), Subscriptions to the topics by the Health Data Exchange app (HDEA),MedMorph’s backend services app, and notifications from the EHR to the HDEA.

### MedMorph Subscription Topics

The MedMorph RA IG specifies that EHRs or other systems need to generate notifications based on subscriptions to trigger processes within a HDEA. Based on the notifications, the HDEA will initiate the reporting workflows based on PlanDefinition resources.

#### Subscriptions and Notifications Using SubscriptionTopic

The following requirements are based on SubscriptionTopics defined as per the [Subscriptions R5 Backport IG]({{site.data.fhir.subscriptionsig}}/index.html).

* Data Sources SHOULD support the SubscriptionTopics identified by each of the named-events in the [Named Event Valueset](ValueSet-us-ph-triggerdefinition-namedevent.html). Each MedMorph Content IG should identify the specific named-event required to be supported by the Data Source for the use case.   

* Data Sources SHOULD support the channel of type ```rest-hook``` to notify the subscribers of changes.

* Data Sources SHOULD support a notification payload type of ```id-only``` where the id of the changed resource is included.   

* Data Sources MAY include in the notification the resources that were changed along with context information. Data Sources SHOULD consider including context information such as the patient and the encounter resources. The MedMorph Content IGs will specify the detailed notifications required as per the use case.

* HDEA SHALL subscribe to the SubscriptionTopics identified by each of the named-events in the [Named Event Valueset](	ValueSet-us-ph-triggerdefinition-namedevent.html) in the Knowledge Artifact.

* HDEA SHALL implement a rest-hook channel to receive notifications from Data Sources.

* HDEA SHALL start the reporting workflow processing using appropriate Knowledge Artifact instances based on the notification received from the Data Source.

**Note**: Examples of the Subscription Topics are available on the [FHIR Artifacts](artifacts.html) examples section. Content IGs will define specific Subscription Topics required for the use case.
