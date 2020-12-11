### Introduction

This profile is used to represent Knowledge Artifacts in the MedMorph Reference Architecture IG. For a detailed overview of Knowledge Artifacts and how they are used, please refer to [Provisioning Requirements](provisioning.html).


**Implementation Requirements**

Implementers are advised to read [Provisioning Requirements](provisioning.html) to implement the PlanDefinition profile.


**API : Creation of PlanDefinition Resource Instance:**

The PlanDefinition instance is created using the regular POST API. The syntax is as shown below.

```
POST http://<FHIR Server URL>/PlanDefinition

The body of the request will contain the JSON data of PlanDefinition to be used in creation.
```

**API : Updation of PlanDefinition Resource Instance:**

The PlanDefinition instance is updated using the regular PUT API. The syntax is as shown below.

```
POST http://<FHIR Server URL>/PlanDefinition/[Instance Id]

The body of the request will contain the JSON data of PlanDefinition to be used for updating the instance. The id of the PlanDefinition body should match the [Instance Id] in the request.
```

**API : Retrieval of PlanDefinition Resource Instance:**

The PlanDefinition instance is retrieved using the regular GET API. The syntax is as shown below.

```
GET http://<FHIR Server URL>/PlanDefinition/[Instance Id]

The instance corresponding to the [Instance Id] is returned.
```

**API : Search of PlanDefinition Resource Instance:**

The PlanDefinition instance can be searched using the regular GET API. The syntax for various searches are as shown below.

```
GET http://<FHIR Server URL>/PlanDefinition/?publisher=[publisher name]

GET http://<FHIR Server URL>/PlanDefinition/?name=[name of plan definition instance]

GET http://<FHIR Server URL>/PlanDefinition/?title=[title of plan definition instance ]

GET http://<FHIR Server URL>/PlanDefinition/?name=[name of plan definition instance ]&version=[version number]

The search results are returned back in a bundle.
```
