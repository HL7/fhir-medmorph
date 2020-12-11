This section defines the specific requirements related to subscriptions and notifications as specified in this MedMorph Reference Architecture IG.  The specification focuses on the creation of SubscriptionTopics in the EHR, Subscriptions to the topics by the Backend Service App, and notifications from the EHR to the Backend Service App.

### MedMorph Subscription Topics

The MedMorph Reference Architecture IG specifies that EHRs or other systems need to generate notifications based on subscriptions to trigger processes within a Backend Service App. Based on the notifications, the Backend Service App will initiate the reporting workflows based on PlanDefinition resources.

**Note:** 
FHIR Subscriptions capability is undergoing significant changes in FHIR R5 including the introduction of the SubscriptionTopic resource. However, the MedMorph Reference Architecture IG is based on FHIR R4 and will continue to be based on FHIR R4. There is an effort in HL7 currently to backport the FHIR R5 Subscription model in FHIR R4. Once the backport is finished the MedMorph Reference Architecture IG will be updated to use the latest subscription capabilities.

In FHIR R4, the SubscriptionTopic resource does not exist, however Subscriptions exist. The next few sections will outline specific requirements for the EHR and the Backend Service App for subscriptions and notifications.

#### Subscriptions and Notifications Using SubscriptionTopic in FHIR R5

As the FHIR subscription capabilities get backported to FHIR R4, SubscriptionTopic resources will be supported in the specification. The following requirements are based on SubscriptionTopics.

* EHRs SHALL support the SubscriptionTopics identified by each of the named-events in the [Named Event Valueset](	http://hl7.org/fhir/us/fhir-medmorph/ValueSet/us-ph-triggerdefinition-namedevent.html).

* EHRs SHALL notify the subscribers of changes based on the named-events using a channel of type ```rest-hook```. 

* EHRs SHALL include in the notification the resources that were changed along with context information. The context information SHALL always include the patient whose data was modified. The context information SHOULD include the encounter during which the change was recorded when applicable.

* Backend Service App SHALL subscribe to the EHR SubscriptionTopics identified by each of the named-events in the [Named Event Valueset](	http://hl7.org/fhir/us/fhir-medmorph/ValueSet/us-ph-triggerdefinition-namedevent.html).

* Backend Service App SHALL implement a rest-hook channel to receive notifications from EHR Subscriptions.

* Backend Service App SHALL start the reporting workflow processing using appropriate PlanDefinition instances based on the notification received from the EHR.

#### Subscriptions and Notifications Using Subscription Resource in FHIR R4 

The current FHIR R4 specification only supports the Subscription Resource and hence until the SubscriptionTopic is backported to FHIR R4 the following requirements need to be followed to subscribe and receive notifications.

* EHRs SHALL support Subscription instance creation for resources identified by each of the named-events in the [Named Event Valueset](	http://hl7.org/fhir/us/fhir-medmorph/ValueSet/us-ph-triggerdefinition-namedevent.html).

* EHRs SHALL support a criteria of "[ResourceType]?all" as part of the Subscription instance. The criteria "[ResourceType]?all" indicates all creates and updates on the particular type of resource for which Subscription is created. 

* EHRs SHALL notify the subscribers of changes based on the named-events using a channel of type ```rest-hook```. 

* EHRs SHALL support the POST API to create the Subscription instance.

* EHRs SHALL support the DELETE API to delete the Subscription instance.

* EHRs SHALL include in the notification the resource that were changed along with context information. The context information SHALL always include the patient whose data was modified. The context information SHOULD include the encounter during which the change was recorded when applicable.

* Backend Service App SHALL create EHR Subscription instances identified by each of the named-events in the [Named Event Valueset](	http://hl7.org/fhir/us/fhir-medmorph/ValueSet/us-ph-triggerdefinition-namedevent.html).

* Backend Service App SHALL implement a rest-hook channel to receive notifications from EHR Subscriptions.

* Backend Service App SHALL start the reporting workflow processing using appropriate PlanDefinition instances based on the notification received from the EHR.

The following is a Subscription example for Condition resource:

```
{
  "resourceType": "Subscription",
  "id": "example",
  "status": "requested",
  "end": "2021-01-01T00:00:00Z",
  "reason": "Monitor new or updated Conditions instances",
  "criteria": "Condition?all",
  "channel": {
    "type": "rest-hook",
    "endpoint": "https://[FHIR Server URL]/backend-service-app/notify",
    "payload": "application/fhir+json"
  }
}
```
