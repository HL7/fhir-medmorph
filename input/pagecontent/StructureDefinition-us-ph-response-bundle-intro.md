### Introduction

This profile is used to represent messages that contain public health/research response data. The Bundle is of type 'message' and the first entry is always the MessageHeader resource. The entry following that is either a Content Bundle or a Document Reference resource.

**Reporting Bundle Structure**

The Response Bundle SHALL always contain the first entry as the MessageHeader resource.
The focus of the MessageHeader (MessageHeader.focus) is either the Content Bundle or the DocumentReference resource.

See the [Response Bundle Example](Bundle-response-bundle-example.json.html)

Implementers are advised to use the DocumentReference resource, if they want to attach a PDF or CDA document that needs to be displayed to the Provider. On the other hand, if the response is just a set of resources, then a Content Bundle can be used. If the Data Receivers want to convey validation errors and other important messages, then using a Content Bundle is appropriate with an OperationOutcome resource that has all the errors.


**Implementation Requirements**

Implementers are advised to read [Report Submission Requirements](reportsubmission.html) to implement the Bundle profile.


**API : Bundle Submission:**

The bundles are submitted using the POST API and invoking the $process-message operation. The syntax is as shown below.

```
POST http://<FHIR Server URL>/$process-message

The body of the request will contain the JSON data of the US PH Response Bundle.
```

