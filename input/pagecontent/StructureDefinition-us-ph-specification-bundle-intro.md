### Introduction

This profile is used to represent the Knowledge Artifact. Each Bundle will contain the relevant resources to define the workflows, trigger codes etc. The bundle is of type collection.

**Specification Bundle Structure**

The Specification Bundle SHALL always contain a PlanDefinition entry.
The Specification Bundle SHOULD contain all the relevant ValueSet resources.

See the [Specification Bundle Example](Bundle-specification-bundle-example.json.html)


**Implementation Requirements**

Implementers are advised to read [Provisioning Requirements](provisioning.html) to implement the Knowledge Artifacts. 
Also refer to the [Plan Definition profile](StructureDefinition-us-ph-plandefinition.html) for accessing Specification Bundles via PlanDefinition APIs.


**API : Knowledge Artifact Retrieval:**

The bundles are retrieved using the GET API on the PlanDefinition resource using $get-kar operation. The syntax is as shown below.

```
GET http://<FHIR Server URL>/PlanDefinition/[id]/$get-kar

The response of the request will be the Knowlege Artifact returned via a Specification Bundle.
```

