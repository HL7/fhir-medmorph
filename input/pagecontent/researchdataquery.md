This section defines the specific conformance requirements for querying a data mart as specified in this MedMorph Reference Architecture IG.

### Requirements for Researcher Portal

* The Researcher Portal SHALL provide an interface to create a Knowledge Artifact containing the researcher query and required value sets for filtering data.

* The Researcher Portal SHALL support both FHIR Path and CQL based queries. 

* The Researcher Portal SHALL maintain Endpoint information for each Data Mart to submit the query. This Endpoint refers to the Backend Service App responsible to interact with the Data Mart. 

* The Researcher Portal SHALL create a Task instance for each submission of the query. This is designated as the parent Task for the query.


### Requirements for Query Translator and Submitter

* The Query Translator and Submitter will translate the FHIR Path or CQL Query to a format desired by the Data Mart based on the Data Mart Endpoint information.

* The Query Translator and Submitter will create a Task for each Data Mart to which the query is submitted. The parent Task for each of the created Tasks is set to the one created by the Researcher Portal.  This will ensure tracking of a single query across all Data Marts and allows for monitoring and management of the Tasks.

* The Query Translator and Submitter will allow the retrieval of Tasks specific to each Data Mart using search mechanisms.

* The Query Translator and Submitter will allow the update of the Task Status by the Backend Service App as the Task gets executed in the data mart.

```
Feedback Requested: In the current CDMH architecture the Query Translator and Submitter is a common service that can be used to translate queries. Should the query translation services be subsumed into the Backend Service App which can leverage common Data/Trust Services for conversion as needed rather than keeping it as a different actor as currently shown in the abstract model. 
```

### Requirements for Backend Service App 

* The Backend Service App SHALL retrieve the specific Tasks designated to be executed on it's connected Data Marts.

* The Backend Service App SHALL execute the query specified by the Task on the Data Mart and collect the results.

* The Backend Service App SHALL update the Task status based on the execution.

* If required and specified in the Knowledge Artifact, the Backend Service App SHALL allow display and approval of query results before submitting them to the researcher.

* The Backend Service App SHALL submit the results to the Query Result Translator and Aggregator after query execution, results review and approval.


### Requirements for Data Mart

* The Data Mart SHALL support execution of queries in one of the following formats
	* ANSI SQL
	* SAS
	* FHIR Path
	* CQL 
	
```
Feedback Requested: Should MedMorph prescribe the query formats or leave it to implementation ? The current approach used by CDMH to translate FHIRPath or CQL queries to different formats in the Query Translator in a common service to all data marts. This works only when the target format is known, if the target format is not known the translations will not help.
```

### Requirements for Result Translator and Aggregator

* The Result Translator and Aggregator SHALL be capable of receiving results from the Backend Service App using a FHIR Bundle.

* The Result Translator and Aggregator SHALL be capable of translating results from local research model formats to FHIR formats leveraging the CDMH developed mappings.

* The Result Translator and Aggregator SHALL be capable of aggregating results from different data marts for the same query before displaying it to the researcher.

```
Feedback Requested: In the current CDMH architecture the Result Translator and Aggregator is a common service that can be used to translate results and aggregate results. Should the result translation services be subsumed into the Backend Service App which can leverage common Data/Trust Services for conversion as needed rather than keeping it as a different actor as currently shown in the abstract model. 
```
