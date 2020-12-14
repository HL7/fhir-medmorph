### Introduction

This profile is used to represent messages that contain public health/research response data. The Bundle is of type 'message' and the first entry is always the MessageHeader resource. The entry following that is a Communication resource that will contain the information relevant to the response.

**Reporting Bundle Structure**

The Response Bundle SHALL always contain the first entry as the MessageHeader resource.
The focus of the MessageHeader (MessageHeader.focus) is the Communication resource.
The Communication resource links the request and the response together.

See the [Response Bundle Example](Bundle-response-bundle-example.json.html)


**Implementation Requirements**

Implementers are advised to read [Report Submission Requirements](reportsubmission.html) to implement the Bundle profile.


**API : Bundle Submission:**

The bundles are submitted using the POST API and invoking the $process-message operation. The syntax is as shown below.

```
POST http://<FHIR Server URL>/$process-message

The body of the request will contain the JSON data of the US PH Reporting Bundle.
```

