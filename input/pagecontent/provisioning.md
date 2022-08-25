This section defines the conformance requirements for systems wishing to implement the Provisioning Workflow. More specifically, the creation of the Knowledge Artifacts, publishing them in a Knowledge Artifact Repository, and their access by the Health Data Exchange App (HDEA) and/or Data Sources. 


### MedMorph Knowledge Artifacts

The next few sections describe and outline specific requirements that need to be followed in creating MedMorph Knowledge Artifacts.

#### Knowledge Artifact Overview

MedMorph Knowledge Artifacts follow the general Event, Condition, Action (ECA) rule in computer programming. An occurrence of an event triggers specific actions which are only executed when certain conditions are met. In MedMorph these ECA rules are used to implement different workflows to collect, package and report data from clinical care without increasing the burden on providers. The figure below gives an overview of ECA rule and examples of how it could be applied for the Central Cancer Registry Reporting and Health Care Survey use cases.

{% include img.html img="ECAExample.svg" caption="Figure 6.1 - ECA Rule and MedMorph Examples" %}

<br>
  
The MedMorph RA IG provides the basic constructs required to enable public health and research reporting using ECA rules embedded in Knowledge Artifacts which are machine processable and can be executed without burdening the provider. In addition to the ECA rules there are references to ValueSets, Library Resources, Security Certificates, and Endpoint information. These different artifacts are shown in Figure 6.2 below.

{% include img.html img="KnowledgeArtifactComponents.png" caption="Figure 6.2 - Knowledge Artifact Components" %}

<br>

#### Machine Processable Events, Conditions, and Actions

MedMorph Knowledge Artifacts which are built using ECA rules have to be machine processable (structurally and semantically) to reduce provider burden. In addition, the Knowledge Artifacts have to be generalized to accommodate multiple use cases specified in Content IGs.

MedMorph has defined a list of named events that typically occur in clinical workflows based on which notifications can be performed. MedMorph Content IGs will select events from the documented [Named Events](ValueSet-us-ph-triggerdefinition-namedevent.html) to start any MedMorph reporting workflow. Typically named events occur due to updates to patient data within a Data Source. The named events are eventually intended to be FHIR Subscription Topics once FHIR subscriptions get implemented in the Data Source. The HDEA subscribes to these [Subscription Topics](ValueSet-us-ph-triggerdefinition-namedevent.html) and will get notified when the corresponding events occur in the workflow. 

Once the HDEA receives notifications for a specific Named Event via FHIR Subscriptions, the HDEA will evaluate the data according to the conditions specified in the Knowledge Artifact. If conditions are met then the actions will be executed as per the ECA rules in the Knowledge Artifact. If conditions are not met actions will not be executed. To describe machine processable conditions, FHIR Expressions are used. MedMorph will use [FHIR Path](https://www.hl7.org/fhir/fhirpath.html) expressions or [CQL](https://cql.hl7.org/) expressions. 

The Actions that can be executed are based on the different use cases considered for the MedMorph RA. MedMorph Content IGs will select actions from the documented [List of PlanDefinition Actions](ValueSet-us-ph-plandefinition-action.html) during the creation of the Knowledge Artifact. The execution sequence of the various actions can be visualized as shown in Figure 6.3 below.

{% include img.html img="ActionSequence.svg" caption="Figure 6.3 - Action Execution Steps" %}

<br>

As shown in Figure 6.3 above, actions will start in Step 1 when a notification from a subscribed named event is delivered from the Data Source to the HDEA. After the notification, the reporting workflow starts in Step 2 which executes a series of sub-steps using predefined actions. Not all actions represented within the sub-steps in the figure are required as part of a workflow. We will consider an example to highlight the actions that will be executed. 

In the case of Central Cancer Registry Reporting, once a named event occurs and the execution of the reporting workflow starts, the following actions will be executed in a sequence.

* Determine if the patient has a reportable cancer condition, by executing the check-trigger-codes action in Step 2a. In Step 2a, the check-participant-registration action is not required for this scenario and hence it is skipped.

* If the patient has a reportable condition, the Cancer Report will be created by accessing appropriate data from the Data Source in Step 2b. The accessed data and executed queries will be defined in the Content IG. The create-report action will assemble the data into a bundle per the MedMorph RA IG and Central Cancer Registry Reporting Content IG.

* Step 2c will be skipped since there is no specific requirement to anonymize, pseudonymize, or de-identify the data. 

* The report is validated in Step 2d to conform to the various profiles specified by the Content IG by executing the validate-report action. 

* Once the report is validated and ready for transmission, the report is submitted by executing the submit-report action in Step 2e.

These actions described above are machine processable and are extensible. As new use cases are instantiated using the MedMorph RA IG, new actions may be added. For example, if the report needs to be signed, an action called add-digital-signature action can be defined and added to Step 2e. Similarly, new steps could also be introduced as needed. 

The next section outlines specific requirements for the creation and consumption of the Knowledge Artifacts by MedMorph RA actors.

#### Creating Knowledge Artifacts

Data Receivers such as PHAs or ROs who receive the data are the actors who create or produce Knowledge Artifacts based on their use cases and program needs. 

* Knowledge Artifact producers SHALL represent the Knowledge Artifact using a US PH Specification Bundle.

* Knowledge Artifact producers SHALL represent ECA rules using PlanDefinition Resource.

* MedMorph Knowledge Artifacts SHALL be of type workflow-definition and is represented in PlanDefinition.type attribute.

* MedMorph workflows are triggered by named-events such as Start of an encounter, close of an encounter, new diagnosis etc. Producers of Knowledge Artifacts SHALL select specific named-events from the [Named Event Valueset](ValueSet-us-ph-triggerdefinition-namedevent.html). In case the required named-event is not available in the value set, the value set MAY be extended in the Content IGs.

* Producers of Knowledge Artifacts SHALL identify the specific named-events to be subscribed to by the HDEA in the PlanDefintion.action.trigger element.

* MedMorph Knowledge Artifacts SHALL use only named-events for triggering which is indicated by setting the PlanDefinition.action.trigger.type to named-event for actions specifying triggers.

* MedMorph workflow actions are essentially activities that get executed to accomplish a specific task. Knowledge Artifact producers SHALL  select specific actions from the [Plan Definition Action Valueset](ValueSet-us-ph-plandefinition-action.html). In case the required action is not available in the value set, the value set MAY be extended in the Content IGs.

* Producers of Knowledge Artifacts SHALL use Expressions of type text/fhirpath.

* Producers of Knowledge Artifacts MAY use alternative expression of text/cql or text/cql.identifier for defining Knowledge Artifacts.  

* MedMorph Conditions SHALL always be of kind "applicability" and are specified in PlanDefinition.Condition.kind element.

* Producers of Knowledge Artifacts SHALL use a Library resource to include the ValueSet references required for matching trigger codes. This Library Resource can be referenced in the PlanDefinition.relatedArtifact element.

* Producers of Knowledge Artifacts using Expressions of type text/cql SHALL include the CQL library reference in the PlanDefinition.library element.

* Producers of Knowledge Artifacts SHALL represent the Data Receiver's endpoint where data has to be submitted in the PlanDefinition.ext-receiverEndpoint extension.

* Producers of Knowledge Artifacts SHALL indicate the input data requirements for a specific action in PlanDefinition.action.input element.

* Producers of Knowledge Artifacts SHOULD define the queries to be used to extract the data from the Data Source using the PlanDefinition.action.input.defaultQuery extension element. 

* Producers of Knowledge Artifacts SHOULD use the PlanDefinition.action.input.relatedDataId extension to reuse data across actions and avoid the same query being executed on the Data Source multiple times.

* Producers of Knowledge Artifacts SHALL indicate the output data requirements for a specific action in PlanDefinition.action.output element.

* Producers of Knowledge Artifacts SHALL include the sub-steps for an action in PlanDefinition.action.action element.

* For executing actions which are delayed by a time duration, Knowledge Artifact producers SHALL specify the duration in the PlanDefinition.action.timing element.

* For executing actions which are dependent on other actions, Knowledge Artifact producers SHALL specify the related action in the PlanDefinition.action.relatedAction element. 

* The PlanDefinition.action.relatedAction.relationship to be used for MedMorph related actions SHALL be before-start. This implies that the related action cannot start until the parent action is completed.
* To specify related actions which are delayed by a time duration, the PlanDefinition.action.relatedAction.offsetDuration element SHALL be used.

* Knowledge Artifact producers SHALL populate all applicable "MUST SUPPORT" elements in the PlanDefinition including inherited "MUST SUPPORT" elements from Shareable PlanDefinition profile. 

* Knowledge Artifact producers SHALL include PlanDefinition and its dependencies specified using FHIR Resources such as Library, ValueSet etc in the US PH Specification Bundle.

#### APIs

The APIs that can be used to create, update, retrieve, and search for Knowledge Artifacts is specified [Specification Bundle profile](StructureDefinition-us-ph-specification-bundle.html) page.

#### Notifications of Changes in Knowledge Artifacts

Knowledge Artifacts get changed from time to time and health care organizations implementing Knowledge Artifacts have to regularly update the PlanDefinition instances based on changes. Currently the MedMorph RA IG does not prescribe any specific notification mechanism.

* The Knowledge Artifact Repository SHALL support retrieval of Knowledge Artifacts using a GET request.

* The Knowledge Artifact Repository SHOULD allow consumers to subscribe to notifications ensuring they get notified of changes via a REST hook or Email as specified in the [Subscriptions R5 Backport IG]({{site.data.fhir.subscriptionsig}}/index.html).

* The consumers of Knowledge Artifacts MAY choose to either subscribe to notifications of changes or poll the Knowledge Artifact Repository and retrieve the latest information and determine if changes have been made.

#### Operationalization of Knowledge Artifacts

Knowledge Artifacts have to be operationalized by healthcare organizations. Before operationalizing the Knowledge Artifact and processing by the HDEA/ Backend Services App or by a Data Source, healthcare organizations may test and validate the processing of the Knowledge Artifact before reporting to Data Receivers.

#### Knowledge Artifact Repository Specific Requirements 

The Knowledge Artifact Repository actor SHALL support the PlanDefinition creation, update, retrieval and search mechanisms as identified in the [Knowledge Artifact Repository Capability Statement](CapabilityStatement-medmorph-knowledge-artifact-repository.html).

#### HDEA Specific Requirements 

* The HDEA actor SHALL support the Knowledge Artifact retrieval and search mechanisms as identified in the [HDEA Capability Statement Example](CapabilityStatement-medmorph-healthdata-exchange-app.html).

* The HDEA SHALL be capable of processing the PlanDefinition instances specified in the Knowledge Artifact and creating Subscriptions in the Data Source based on the named-events specified in the PlanDefinition.action.trigger elements.

* The HDEA SHALL be capable of processing FHIR Path Expressions to evaluate Conditions which are specified as part of the PlanDefinition.action.condition elements

* The HDEA SHALL implement the ability to process action dependencies specified via the PlanDefinition.action.relatedAction elements. Typical implementations will build a graph or a tree structure to identify dependencies between actions.

* The HDEA SHALL be capable of scheduling jobs based on timing offsets specified in the PlanDefinition.action.timing and PlanDefinition.action.relationAction.offsetDuration elements.

* The HDEA SHALL be capable of querying the Data Sources using FHIR APIs to retrieve patient specific content as specified in the PlanDefinition.action.input element.

