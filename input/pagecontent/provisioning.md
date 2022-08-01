This section defines the conformance requirements for systems wishing to conform to Knowledge Artifact Repository actor specified in this MedMorph Reference Architecture IG.  The specification focuses on the creation of the Knowledge Artifacts and their access by the Backend Service App and/or EHRs. 


### MedMorph Knowledge Artifacts

The next few sections describe and outline specific requirements that need to be followed in creating MedMorph Knowledge Artifacts.

#### Knowledge Artifact Overview

MedMorph Knowledge Artifacts follow the general Event, Condition, Action (ECA) rule in computer programming. An occurrence of an event triggers specific actions which are only executed when certain conditions are met. In MedMorph these ECA rules are used to enable public health reporting from clinical care without increasing the burden on providers. The figure below gives an overview of ECA rule and examples of how it could be applied for the Cancer Reporting and Healthcare Survey use cases.

{% include img.html img="ECAExample.svg" caption="Figure 6.1 - ECA Rule and MedMorph Examples" %}

<br>
  
The MedMorph Reference Architecture IG is providing the basic constructs required to enable public health and research reporting using  ECA rules embedded in Knowledge Artifacts which are machine processable and can be executed without burdening the provider. In addition to the ECA rules there are references to ValueSets, Library resources, security certificates and Endpoint information. These different artifacts are shown in Figure 6.2 below.

{% include img.html img="KnowledgeArtifactComponents.png" caption="Figure 6.2 - Knowledge Artifact Components" %}

<br>

#### Machine Processable Events, Conditions, and Actions

Events, Conditions, and Actions which are the basis of knowledge artifacts have to be machine processable and have to be defined (structurally and semantically) to accommodate multiple use cases.

MedMorph has defined a list of named events that typically occur in clinical workflows based on which notifications can be performed. MedMorph will use the documented [Named Events List](ValueSet-us-ph-triggerdefinition-namedevent.html) as the starting point to kickoff knowledge artifact processing. Typically named events occur due to updates to patient data within an EHR. The named events are eventually intended to be FHIR Subscription Topics as FHIR subscriptions get implemented in the EHRs. The Backend Service App subscribes to these [Subscription Topics](ValueSet-us-ph-triggerdefinition-namedevent.html) and will get notified when these events occur along with context information. 

Once the Backend Service App receives notifications of the Named Events via Subscriptions, the Backend Service App will have to evaluate the data based on the Conditions specified in the Knowledge Artifact. If conditions are met then the actions will be executed. If conditions are not met actions will not be invoked. To describe machine processable conditions, FHIR Expressions are used. MedMorph will use [FHIR Path](https://www.hl7.org/fhir/fhirpath.html) expressions or [CQL](https://cql.hl7.org/) expressions. 

The Actions that need to be performed are based on the different use cases considered for MedMorph architecture. MedMorph will use the documented [Action List](ValueSet-us-ph-plandefinition-action.html). The execution sequence of the various actions can be visualized as shown in Figure 6.3 below.

{% include img.html img="ActionSequence.svg" caption="Figure 6.3 - Action Execution Steps" %}

<br>

As shown in Figure 6.3 above, actions will start in Step 1 when a notification from a subscribed named event happens. After the notification, the reporting workflow starts in Step 2 which has a series of sub-steps. Not all sub-steps or all actions represented within the sub-steps in the figure are required as part of a workflow. We will consider an example to highlight the actions that will be executed. 

In the case of Cancer Reporting, once a named event occurs and the execution of the reporting workflow starts, the following actions will be executed in a sequence.

* Determine if the patient has a reportable Cancer condition, by executing the check-trigger-codes action in Step 2a. In Step 2a, the check-participant-registration action is not required for this scenario and hence it is skipped.
* If the patient has a reportable condition, the Cancer report will be created by accessing appropriate data from the EHR in Step 2b. The data to be accessed and the queries to be executed will be defined in the Content IGs. The create-report action will assemble the data into a bundle per the MedMorph Reference Architecture IG.
* Step 2c will be skipped since there is no specific requirement to anonymize, pseudonymize or de-identify the data. 
* The report is validated in Step 2d to conform to the various profiles by executing the validate-report action. 
* Once the report is validated and ready for transmission, the report may be encrypted and then submitted for transmission in Step 2e by executing the encrypt-report and submit-report actions respectively. 

The actions are machine processable and are extensible. As new use cases are instantiated using the MedMorph Reference Architecture new actions may be added. For example, if the report needs to be signed, an action called add-digital-signature action can be defined and added to Step 2e. Similarly, new steps could also be introduced as needed. 

The next section outlines specific requirements for the creation and consumption of the MedMorph Knowledge Artifacts.

#### Creating Knowledge Artifacts
PHAs or Research Organizations are the actors who create or produce Knowledge Artifacts. 

* Knowledge Artifact producers SHALL represent Knowledge Artifacts using PlanDefinition Resource.

* MedMorph Knowledge Artifacts SHALL be of type eca-rule or workflow-definition and is represented in PlanDefinition.type attribute.

* MedMorph Workflows are triggered by named-events such as Start of an encounter, close of an encounter, new diagnosis etc. Producers SHALL select specific named-events from the [Named Event Valueset](ValueSet-us-ph-triggerdefinition-namedevent.html). In case the required named-event is not available in the value set, the value set MAY be extended.

* Producers of Knowledge Artifacts SHALL identify the specific named-events to be subscribed to by the Backend Service App in the PlanDefintion.action.trigger.ext-namedEventType extension element.

* MedMorph Knowledge Artifacts SHALL use only named-events for triggering and is indicated by setting the PlanDefinition.action.trigger.type to named-event for actions specifying triggers.

* MedMorph Workflow actions are essentially activities that get executed to accomplish a specific task. Producers SHALL  select specific actions from the [Plan Definition Action Valueset](ValueSet-us-ph-plandefinition-action.html). In case the required action is not available in the value set, the value set MAY be extended.

* Producers of Knowledge Artifacts SHALL use Expressions of type text/fhirpath for defining Knowledge Artifacts. Producers MAY also create the same Knowledge Artifact using Expressions of type text/cql. 

* MedMorph Conditions SHALL always be of kind "applicability" and are specified in PlanDefinition.Condition.kind element.

* Producers of Knowledge Artifacts SHALL include the ValueSet reference required for matching trigger codes in the PlanDefintion.relatedArtifact element.

* Producers of Knowledge Artifacts using Expressions of type text/cql SHALL include the CQL library reference in the PlanDefinition.library element.

* Producers of Knowledge Artifacts SHALL represent the receiver's address in the PlanDefinition.ext-receiverAddress extension.

* Producers of Knowledge Artifacts SHALL indicate the input data requirements for a specific action in PlanDefinition.action.input element.

* Producers of Knowledge Artifacts SHALL indicate the output data requirements for a specific action in PlanDefinition.action.output element.

* For executing actions which are delayed by a time duration, Knowledge Artifact producers SHALL specify the duration in the PlanDefinition.action.timing element.

* For executing actions which are dependent on other actions, Knowledge Artifact producers SHALL specify the related action in the PlanDefinition.action.relatedAction element. 

* The PlanDefinition.action.relatedAction.relationship to be used for MedMorph related actions SHALL be before-start. This implies that the related action cannot start until the parent action is executed. 

* To specify related actions which are delayed by a time duration, the PlanDefinition.action.relatedAction.offsetDuration element SHALL be used.

* MedMorph Knowledge Artifact producers SHALL demonstrate the ability to populate all applicable "MUST SUPPORT" elements in the PlanDefinition including inherited "MUST SUPPORT" elements from Shareable PlanDefinition profile. 

#### APIs

The example APIs that can be used to create, update, retrieve, and search for PlanDefinition is specified [PlanDefinition profile](StructureDefinition-us-ph-plandefinition.html) page.

#### Notifications of Changes in Knowledge Artifacts

Knowledge Artifacts get changed from time to time and healthcare organizations implementing Knowledge Artifacts have to regularly update the PlanDefinition instances based on changes. Currently the MedMorph Architecture IG does not prescribe any specific notification mechanism.

* The Knowledge Artifact Repository SHALL support retrieval of Knowledge Artifacts using a GET request.

* The Knowledge Artifact Repository SHOULD allow consumers to subscribe for notifications so that they get notified of changes via a REST hook or Email as specified in the [Subscriptions R5 Backport IG]({{site.data.fhir.subscriptionsig}}/index.html).

* The consumers of Knowledge Artifacts MAY choose to either subscribe for notifications of changes or poll the Knowledge Artifact Repository and retrieve the latest information and determine if changes have been made.

#### Operationalization of Knowledge Artifacts

Knowledge Artifacts have to be operationalized by healthcare organizations. Before operationalizing the Knowledge Artifact and its processing by a Backend Service App or by an EHR, healthcare organizations may test and validate the processing of the knowledge artifact before operationalizing in production to enable reporting to Public Health / Research Organizations.

#### Knowledge Artifact Repository Specific Requirements 

Knowledge Artifact Repository actor SHALL support the PlanDefinition creation, update, retrieval and search mechanisms as identified in the [Knowledge Artifact Repository Capability Statement](CapabilityStatement-medmorph-knowledge-artifact-repository.html).

#### Backend Service App Specific Requirements 

The Backend Service App actor SHALL support the PlanDefinition retrieval and search mechanisms as identified in the [Backend Service App Capability Statement](CapabilityStatement-medmorph-backend-service-app.html).

The Backend Service App SHALL be capable of processing the PlanDefinition instances and creating Subscriptions in the EHR based on the named-events specified in the PlanDefinition.action.trigger elements.

The Backend Service App SHALL be capable of processing FHIR Path Expressions to evaluate Conditions which are specified as part of the PlanDefinition.action.condition elements

The Backend Service App SHALL implement the ability to process action dependencies specified via the PlanDefinition.action.relatedAction elements. Typical implementations will build a graph or a tree structure that can be used to identify dependencies between actions.

The Backend Service App SHALL be capable of scheduling jobs based on timing offsets specified in the PlanDefinition.action.timing and PlanDefinition.action.relationAction.offsetDuration elements.

The Backend Service App SHALL be capable of querying the EHRs using US Core APIs to retrieve patient specific content.

