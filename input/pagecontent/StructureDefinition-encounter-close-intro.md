### Introduction

This profile is used to represent the canonical url of the Subscription Topic for the [encounter-close named event](CodeSystem-us-ph-triggerdefinition-namedevents.html).

EHRs supporting the subscription topic of encounter-close has to ensure that the

	* Encounter.period.end has a valid time populated which indicates when the encounter ended.
	* Encounter.status is set to "finished" to indicate that the encounter is complete. 
	
**Feedback Requested** 

1. Are EHRs ensuring closing of encounters ? 
2. How do EHRs handle long running encounters (Meaning encounters running for months) maybe for long term care etc ?

 