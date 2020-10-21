### Business need

Providers need to access Member Attribution Lists for the following business needs

* Use the list to close care gaps for members that 'count' in financial reconciliation

* Use the list to prospectively manage patients and meet quality metrics and manage costs by closing care gaps.

* Use the list to track care costs of attributed patients, manage utilization, monitor referrals/specialist care.

* Use the list to perform end of year reconciliation to get 'credit' for the right patients based on plans/contracts. 

* Use the list for monthly membership tracking to see trends and confirm accountable care PMPM payments

* Use the list to determine project membership for upcoming year based on prospective lists at end/start of year.

* Use the list to keep historical data of patients for continuity of care as members fall off or get added to the list

* Use the list for performance reporting on specific targets and measures.

Using FHIR based APIs, providers and payers can exchange Member Attribution Lists which can enable existing business processes and systems to meet the above business needs. The creation of a Member Attribution List typically starts with a need to identify the patients for a specific purpose such as Risk Based Contracts or Quality Reporting. Once the patients are identified other FHIR APIs and Da Vinci specifications can be used to retrieve clinical, financial or other relevant information as needed.

### Example Member Attribution List Exchange Scenarios (Success path)

#### Scenario 1
Provider Organization A enters a risk-based contract with Payer B. As part of establishing the contract targets, specific measures and financial incentives are documented. Payer B uses historical claims and other information present about members to create a Member Attribution List for Provider Organization A. The Member Attribution List identifies the list of patients that Provider Organization A is responsible for as part of the contract. Payer B needs to exchange this list with Provider Organization A periodically to ensure that Provider Organization A is aware of the list of patients that it is responsible for as per the contract. Payer B could publish the list in standard way and Provider Organization A retrieves the list for use. Alternatively Provider Organization A may request for the list and Payer B provides the list once it is ready.

#### Scenario 2
Provider Organization A enters a risk-based contract with Payer B. As part of establishing the contract targets, specific measures, financial incentives are documented. Payer B uses historical claims and other information present about members to create a Member Attribution List for Provider Organization A. The list is published by Payer B and Provider Organization A retrieves the list. Once Provider Organization A reconciles the list, it identifies a list of changes that need to be done to the list and notifies the Payer B about the changes. Payer B either accepts or rejects the changes and may modify the existing Member Attribution List. If the list is modified, Payer B notifies Provider Organization A of the changes. Provider Organization A retrieves the list and starts using the list for various business needs. 

#### Use of Member Attribution List as part of the Data Exchange for Quality Measures and Care Gaps
Member Attribution Lists are fundamental to closing care gaps and reporting on quality measures. Providers have to report on specific patients to payers on specific quality measures and close any care gap requirements that may exist. Providers and Payers agree upon the list of patients for whom reporting has to be performed on a regular basis. Similarly care gaps associated with these patients have to be closed to receive applicable financial incentives. In all these cases the agreement on what is the "list of patients" is critical. This function is served by the Member Attribution List. The Member Attribution List can be agreed upon between the Payers and Providers and exchanged on a regular cadence. Based on the patients attributed in the Member Attribution List Payers can request Providers for specific quality measure data, care gaps data. Similarly Providers can close care gaps for patients attributed to them via the Member Attribution List and can report quality data using the Data Exchange for Quality Measures Da Vinci implementation guide.

** Note: For the initial version of the Implementation Guide, the notification of changes and requesting changes are not in scope. These aspects will be examined in future releases.   


### Member Attribution List workflows and definitions

The following definitions are used for the Member Attribution List implementation guide.

1. __MemberId:__
A unique identifier for a member within a payers plan.

2. __Payer Identifier:__ 
A unique identifier for an organization that provides insurance plans and enters into contracts with health care providers.

3. __Contract Identifier:__
A unique identifier assigned by the Payer to the agreement with a specific healthcare provider.

4. __Plan Identifier:__
A unique identifier for an insurance plan that a payer provides. Members belong to specific plans.

**NOTE:** The combination of Member Identifier, Payer Identifier, Contract Identifier and Plan Identifier is always unique.

5. __Attribution:__ 
Results of Algorithmic or manual process that assigns patients to providers or payers. Alternatively a patient could declare to be part of a Group by providing or selecting their PCP information or ACO information.

6. __Member Attribution List:__
Enumeration of patients who are attributed to payers, providers, medical homes or groups. Patients may belong to multiple plans. 
The Attribution list contains the patient information along with information such as Attributed Provider, Health Plan information, Validity Period for the list, Risk Information etc. A member may be present multiple times within a member attribution list, however the combination of Member Identifier, Payer Identifier, Contract Identifier and Plan Identifier is always unique and can be used to identify the member.

7. __Attributed Provider:__
Provider responsible for managing the quality and costs of the patient’s health care per the contract and will receive the payments and credits based on performance.

8. __Attribution Period:__
The period over which the member is attributed to a specific provider. The period has a start and an end date.

8. __Producer:__
An Entity creating the attribution list.
Producer Owns the master copy of the list. 
Producers allow for changes to be made to the list. 
A Producer may receive an initial list from consumer and owns the list after that point in time and publishes the list for consumptionCons.

9. __Consumer:__ 
An Entity consuming the attribution list. 
Consumers may contribute to the creation of the list owned by the Producer.
Consumers may request changes to be made to the list owned by the Producer.
Consumers may receive an initial list from producer and may request changes to the list before reaching final agreement on the list.

10. __Attribution List Data:__
Data contained within the attribution list. Data includes
* Patient Demographics Data 
* Attributed Provider Data (First Name, Last Name, Id, NPI, TIN, Address)
* Health Plan Data (Subscriber Id, Member Id, Medicare Id, Medicaid Id, Plan Name, Plan Type, Enrollment Start and End Dates)
* Attribution Data (Effective Start and End Date for Attribution, Attribution Method, Risk score)
* Miscellaneous Data (ACO Information, Conditions)

11. __Attribution List Changes:__ 
Addition of patients to the attribution list. 
Deletion of patients to the attribution list. 
Changes in attribution list data. 



{::options parse_block_html="false" /}

<object type="image/svg+xml" data="workflow.svg" alt="Member Attribution List Exchange Workflow">
    <!-- Your fall back here -->
    <img src="workflow.svg" />
</object>

{::options parse_block_html="true" /}


The following is a brief description of the workflow steps with a Payer representing the Producer and a Provider Organization representing the Consumer. 

**1. Payer (Producer) and Provider (Consumer) enter into a contract **<br/>
Provider and a Payer enter into a contract with specific terms and conditions and decide on the need for a Member Attribution List. Payer internally creates the Member Attribution List using internal processes and existing data about the patients. 

**2. Payer (Producer) Informs Provider (Consumer) and makes the List available to the Provider **<br/>
In this step the Payer informs the Provider about the list and makes it available to the Provider. The specific mechanism of how this exchange happens varies based on Payers and Providers. This implementation guide will specify standards for this interaction.

**3a. Provider (Consumer) informs Payer (Payer) about changes **<br/>
Once the Provider receives the list in Step 2, Provider reconciles the list with internal lists and data and in case changes are needed, notifies the Payer about specific changes. These changes could be to add additional patients, remove patients from the list etc. The specific mechanism of how this exchange happens varies based on Payers and Providers. Future versions of this implementation guide will specify standards for requesting and notifying of changes.
If the Consumer has no changes, then Step 3a would not be executed and the Consumer has accepted the list for usage. 

Note: Steps 2 and 3a may be repeated as many times as needed until the Producer and Consumer agree upon the member attribution list.

**3b. Provider (Consumer) informs Payer (Producer) about no changes **<br/>
Once the Provider receives the list in Step 2, Provider reconciles the list with internal lists and data and in case no changes are needed, notifies the Payer about finalizing the list. The specific mechanism of how this exchange happens varies based on Payers and Providers. Future versions of this implementation guide will specify standards for notifying of changes and no changes.

**4. Payer (Producer) makes list available to Provider (Consumer) at regular intervals **<br/>
Once the list is finalized, the Payer and Provider agree to exchange the list periodically as required. The specific mechanism of how this exchange happens varies based on Payers and Providers. This implementation guide will specify standards for this interaction.

**NOTE:** The above workflow is the normal scenario. In addition to the above workflow the Producer may change the list periodically based on additional data. As this happens the Producer may decide to redo Steps 2 through 4. In this version of the IG, there are no notification mechanisms created for a Producer to notify a consumer when the list changes. However there could be an agreed upon cadence between the Producer and the Consumer on how often the list is exchanged. 
Every change that is made to the list may be available to the Consumer based on the cadence. In other words if it is a monthly list, then as changes get made the updated list will be exchanged on a monthly basis. The change reconciliation process is out of scope for the current version of the IG as outlined in the above steps.

#### Considerations

* The scenario above uses the term 'Producer'.  Typically, that would be a Payer organization, but in some cases, it could be a Provider Organization. This is true in [Data at Point of Care use cases](https://dpc.cms.gov/).

### Member Attribution List Exchange Patterns

The section takes the workflow described above and identifies the different types of exchange patterns that are currently used. Although each of the patterns identified below are used in the real world, the initial version of the Implementation Guide will focus on the exchange mechanisms identified in pattern #2.

{::options parse_block_html="false" /}

<object type="image/svg+xml" data="exchange.svg" alt="Member Attribution List Exchange Patterns">
    <!-- Your fall back here -->
    <img src="exchange.svg" />
</object>
{::options parse_block_html="true" /}

<br/>

{::options parse_block_html="false" /}

<object type="image/svg+xml" data="exchange-2.svg" alt="Member Attribution List Exchange Patterns">
    <!-- Your fall back here -->
    <img src="exchange-2.svg" />
</object>
{::options parse_block_html="true" /}


The following are brief descriptions of the Member Attribution List Exchange patterns.

**Pattern 1:**
In pattern 1, the Producer is pushing the list to the consumer using a "One-way push" exchange pattern.

**Pattern 2:**
In pattern 2, the Consumer request the Member Attribution List from the producer. The Consumer then receives the 
list once it is available. 

Note: The requests and responses are not real-time as the Producer may have to perform work to prepare and make the list available. The initial version of the implementation guide will focus on this specific exchange pattern.

**Pattern 3:**
In pattern 3, the Producer notifies the Consumer of changes in the Member Attribution List. The Consumer requests for the changes and eventually receives incremental changes.

Note: The requests and responses are not real-time as the Producer may have to perform work to prepare and make the list available. Future versions of the implementation guide will focus on this specific exchange pattern.

**Pattern 4:**
In pattern 4, the Consumer requests changes to the Member Attribution List and then the rest of the interactions follow exchange pattern #3. 

Note: Future versions of the implementation guide will focus on this specific exchange pattern.

**Pattern 5:**
In pattern 5, the Consumer provides an initial attribution list to the Producer. This could be based on the patients being treated by the provider. This is true in [Data at Point of Care use cases](https://dpc.cms.gov/). The rest of the interactions follow exchange pattern #3. 

Note: Future versions of the implementation guide may focus on this specific exchange pattern depending on the extent it applies to the commercial payer and provider ecosystem.

**Pattern 6:**
In pattern 6, the interactions are essentially combination of patterns #5, #4, #3 and #2. 




