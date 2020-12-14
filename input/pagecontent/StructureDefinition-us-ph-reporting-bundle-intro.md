### Introduction

This profile is used to represent messages that contain public health/research reporting data. The Bundle is of type 'message' and the first entry is always the MessageHeader resource. The entry following that is a nested content bundle that will contain all the resources that need to be submitted. When data is to be encrypted, the Content Bundle is encrypted; however, the outer Bundle and the MessageHeader are not encrypted.

**Reporting Bundle Structure**

The Reporting Bundle SHALL always contain the first entry as the MessageHeader resource.
The focus of the MessageHeader (MessageHeader.focus) is the Content Bundle.
The Content Bundle contains all the resources related to the Patient.

See the [Reporting Bundle Example](Bundle-reporting-bundle-example.json.html)


**Implementation Requirements**

Implementers are advised to read [Report Submission Requirements](reportsubmission.html) to implement the Bundle profile.


**API : Bundle Submission:**

The bundles are submitted using the POST API and invoking the $process-message operation. The syntax is as shown below.

```
POST http://<FHIR Server URL>/$process-message

The body of the request will contain the JSON data of the US PH Reporting Bundle.
```
