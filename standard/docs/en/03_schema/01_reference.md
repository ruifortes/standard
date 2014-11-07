[TOC]

# Schema Reference

<span class="lead">The [Release Schema](../release) provides a detailed specification of the fields and data structures to use when publishing data, with supplementary schemas showing how to combine release and records. This reference section works step-by-step through additional supporting information to assist publishers and users of the data.</span>

(Note: If any conflicts are found between this text, and the text within the schema, the schema takes precedence)

## Release structure

The majority of OCDS data is held within a structured [release](../release). Releases must be published within a release package. Releases are made up of a number of blocks of data, including:

* tender
* award
* contract
* performance

A release can only contain one set of tender information (as we define a unique [contracting process](../../definitions#contracting-process) by the initiation stage), but may contain multiple instances of awards, contracts and performance information.

Releases are given a [tag](#release-tag) to indicate what stage of a contracting process they represent, but there are no formal restrictions on when information about a stage of the contracting process may be provided. For example, a publisher announcing the signing of a contract with a 'contract' tagged release, may also include information in the award and tender blocks in order to provide a comprehensive picture of the contracting process to date which led to that contract being signed. 

### Package Metadata

A release package, modelled on the [Data Package](http://dataprotocols.org/data-packages/) protocol, provides meta-data about release(s) it contains. 

<div class="include-csv" data-src="standard/docs/field_definitions/release-package.csv" data-table-class="table table-striped schema-table"></div>

#### Notes

* The uri should unique identify this release package. Publishers should provide a [dereferenceable HTTP URI](http://en.wikipedia.org/wiki/Dereferenceable_Uniform_Resource_Identifier) wherever possible and should host the data package at this URI, enabling users to look-up and verify the contents of a release package from its original source. 

* The [publishedDate](#date) on which this package was published. If a package is automatically generated and re-published on a regular basis, this date should reflect the date of the last change to the contents of the package. 

* The publisher should be identified using an Organisation block. See the [Organization/Entity](#entity) guidance for details.

* See the [licensing guidance](../../implementation/publication_patterns#licensing) for more details on selecting and publishing license information. 

* See the [publication policy](../../implementation/publication_patterns#publication-policy) guidance for more details.

### Top level

A release consists of the following fields and objects:

<div class="include-csv" data-src="standard/docs/field_definitions/release-toplevel.csv" data-table-class="table table-striped schema-table"></div>

#### Notes

* ```ocid``` - Providing each [contracting process](../../definitions#contracting-process) with a unique identifier is essential to enable data about contracts to be linked up across different releases. Open Contracting IDs are composed of a prefix assigned to each publisher, and a local identifier drawn from their internal systems that can be used to tie together tenders, awards, contracts and other key data points from a specific contracting process. See the [Open Contracting Identifier guidance](../../identifiers#ocid) for details of how to construct an OCID. 

* ```releaseTag``` - The release tag is used to identify the nature of the release being made. This can be used by consuming applications to filter releases, or may in future be used for advanced validation. A release which updates or amends previous data must always use the appropriate update or amendment release tag. Values must be drawn from the [releaseTag codelist](../codelists#release-tag).

* ```date``` - The release [date](#date) should reflect the point in time at which the information in this release was disclosed. A release package may contain release with different release dates.

* ```language``` - see the section on [multi-language support](#multi-language-support) for information on language handling.

* ```buyer``` - The buyer details are published using an [organization](#entity) block.

Further details on each of the blocks contained within release are below. 

### Planning

The planning section can be used to describe the background to a contracting process. This may include details of the budget from which funds are drawn, or related projects for this contracting process. Background documents such as a needs assessment, feasibility study and project plan can also be included in this section.

#### Fields
<div class="include-csv" data-src="standard/docs/field_definitions/release-planning.csv" data-table-class="table table-striped schema-table"></div>

Apart from documents, the majority of information is held within the budget block. This is designed to allow both machine-readable linkable data about budgets, cross-referencing to data held in other standards such as the [Budget Data Package](https://github.com/openspending/budget-data-package) or [International Aid Transparency Initiative Standard](http://www.iatistandard.org), and human readable description of the related budgets and projects, supporting users to understand the relationship of the contracting process to existing projects and budgets even where linked data is not available.
    
<div class="include-csv" data-src="standard/docs/field_definitions/release-budget.csv" data-table-class="table table-striped schema-table"></div>


### Tender

The tender section includes details of the announcement that a organization intends to source some particular goods or services, and to establish one or more contract(s) for these.

It may contain details of a forthcoming process to receive and evaluate proposals to supply these goods and services, and may also be used to record details of how a completed tender process, including details of bids received. 

#### Fields

<div class="include-csv" data-src="standard/docs/field_definitions/release-tender.csv" data-table-class="table table-striped schema-table"></div>

#### Notes 

* ```tender.id``` - see the [identifiers guidance](../../key_concepts/identifiers#tender_award_and_contract) for further information on the tender identifier. In most cases this can be the same as the ocid.

* ```procuringEntity``` - in many cases the organization managing the procurement process will be different from the organization whose budget is being used for the procurement (the 'buyer' in OCDS terminology). If this is the case, then the details of this procuring organization should be provided here. 

* ```title``` and ```description``` - tender title and description are optional. The details of items to be procured should always be provided in ```items```. Descriptions should not be used in place of providing structured data on items, dates and other details. Instead, title and description should be used to provide a brief overview of the tender. Publishers should consider adopting a 'tweet length' title, and should avoid ALL UPPERCASE titles, or titles containing code words or other artefacts from internal databases. The goal of these fields is to give users a clear idea of the nature of a tender. 

* ```items``` - publishers should provide details of each of the items to be procured under this tender. 

* ```milestones``` - publishers should list any relevant milestones associated with the delivery of the goods and services covered by this tender. These are the milestones against which the whole contracting process will be evaluated. Publishers may include information about key milestones during the tender process itself, but should not use this in place of ```tenderPeriod```, ```enquiryPeriod``` or ```awardPeriod```.

* ```value``` and ```minValue``` - the total upper estimated value of a procurement should be given in ```value```. For publishers who also specify a estimate minimum value, this can be placed in ```minValue```.

*  ```method``` and ```methodRationale``` - tendering processes can use a variety of methods. Publishers should map their methods to one of the approved codes according to the [GPA definitions](http://www.wto.org/english/docs_e/legal_e/rev-gpr-94_01_e.htm) of open, selective or limited. A free text explanation of why a given method was appropriate to this tender can be provided in ```methodRationale```. 

* ```awardCriteria``` and ```awardCriteriaDetails``` - The [award criteria code list](../codelists#award-criteria) describes the basis on which contract awards will be made. This is an open codelist, and so may be extended with new codes. Free text describing the basis on which bids will be judged, and made, can be provided in the ```awardCriteriaDetail``` field. Publishers wishing to provide more structured information about selection, shortlisting and award criteria should propose [extensions](../../key_concepts/conformance#extensions) for this. 

* ```documents``` - supporting documentation should be attached to the tender. This may include official legal notices of tender, as well as technical specifications, evaluation criteria, and, as a tender process progresses, clarifications, replies to queries and copies of bids submitted or listings of shortlisted firms. See the [attachments](#attachments) section for more details of how to include documents, and consult the [documentType codelist](../codelists#document-type) for suggested documents to include for basic, intermediate or advanced publication.

Information on bidders against a contract will be handled by an extension during the period of the standard release candidate. Publishers wishing to provide detailed information on bidders should [contact support](../../standard/support).


### Award

The award section is used to announce any awards issued for this tender. There may be multiple awards made. Releases can contain all, or a subset, of these awards. A related award block is required for every contract, as it contains information on the suppliers. 

<div class="include-csv" data-src="standard/docs/field_definitions/release-award.csv" data-table-class="table table-striped schema-table"></div>

#### Notes


### Contract

The contract section is used to provide details of contracts that have been entered into.  

<div class="include-csv" data-src="standard/docs/field_definitions/release-contract.csv" data-table-class="table table-striped schema-table"></div>

#### Notes

Every contract needs to have a related award, linked via the ```awardID``` property. This is because supplier information is contained within the 'award'. The framework contract details below help illustrate the reasons for this. 

#### Framework contracts

A framework is an agreement with suppliers to establish terms governing contracts that may be awarded during the life of the agreement[*](http://www.constructingexcellence.org.uk/tools/frameworkingtoolkit/whatis.jsp). 

Framework **agreements** can be represented by [awards](#award), which would each detail a supplier participating in the framework, the line items that the agreement covers with this supplier, and any maximum value for the agreement over time.  

Each call-off purchase against a framework agreement would result in a [contract](#contract), related to the framework agreement [award](#award) via ```awardID```. 

As a result, [award](#award) and [contract](#contract) each contain an [items](#items) block, allowing the award to describe the possible goods and services that can be supplied, whilst the contract describes those that are to be supplied in any particular instance. 

### Implementation

Contract blocks can contain nested implementation information. 

ToDo:

## Record structure

Whereas there can be multiple releases concerning a given contracting process, there is a single **contracting record** for each OCID, providing a summary of all the available data about this particular contracting process.

Releases MUST contain:

* An [OCID](../../key_concepts/identifiers#ocid)
* An array of **releases** related to this contracting process - either by providing a URL for where these releases can be found, or embedding a full copy of the release

Release SHOULD contain:

* **compiledRelease** - the latest version of all open contracting process fields, represented using the release schema. For example, if a contractSignature release has been issued with with a contractValue of $100, and then a contractAmendment release has been issued with a contractValue updated to $200, the compiledRelease would have contract/contractValue of $200.

and

* **versionedRelease** - containing the history of the data in the compiledRecord, with all known past values of any field and the release that information came from.

Records should be embedded within a record package.  

### Package meta-data

<div class="include-csv" data-src="standard/docs/field_definitions/record-package.csv" data-table-class="table table-striped schema-table"></div>

See the guidance on [package meta-data](#package-metadata) above. In addition, a record package includes:

* ```packages``` - which should provide links to all the release packages used to compile this record. 
* ```records``` - see below.

### Records

Each record package contains an array of one or more records, consisting of the following sections:

* Releases (required)
* Compiled Release (optional)
* Versioning Release (optional)

#### Releases

The releases that go to make up a contracting process can be provided in two ways:

* URLs for each release
* Embedded copies of the release

If providing and array of URLs, it should be possible for a consuming application to look up each URL, retrieve a release package, and locate the release inside it. 

In order to locate the specific release inside a release package the releaseID of the release should be appended to the package URL using a fragment identifier.

For example:

* http://ocds.open-contracting.org/demos/releases/12345.json#ocds-a2ef3d01-1594121/1 to refer to the release with a release.id of ocds-a2ef3d01-1594121/1 

ToDo: Construct these as real examples.

#### Compiled Release

The compiled release is latest version of all the data about this contracting process, and has the same schema as a release.

The process for creating a compiled release is described in the guidance on [merging](../../implementation/merging). 

A compiled release provides a snapshot of the current state of a contracting process. 

#### Versioned Release

A versioned release contains the history of all the data in the compiled release, including which fields have changed, when they were changed, and the release that updated them.

Providing this versioned information is valuable for many use cases relating to contract monitoring. 

Publishers may chose to provide a copy of the record with, and without, this information, as for large contracting processes it can add substantially to file sizes.

Third parties should also be able to use the information in the releases list to fetch source data, compile and verify their own version history for a contract.


## Multi-language support (ToVerify)

Many publishers need to be able to share key data in multiple languages. All free-text title and description fields in the Open Contracting Data Standard can be given in one or more languages.

Language variations are included by a copy of multi-lingual fields, suffixed with a language code.

E.g. ```title``` and ```title_es```

In order to allow users to identify the language used in non-suffixed fields, OCDS release and records should declare the default language in the ```language``` field. 

In order to allow users to identify all the other languages in a file, the language codes for any other included languages should be given in the ```languagesIncluded``` array. (ToDo: CHECK PROPERTY NAME)

Languages should be identified using language tags taken from [BCP47](http://tools.ietf.org/html/bcp47). The specification allows BCP47 values in order to accommodate variations in dialect where this is important. However, publishers **should** use the two letter [ISO-639-1 two-digit language tags](http://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) in the vast majority of circumstances, and should not assume that the users are able to distinguish between sub-tag variations (for example, OCDS publishers should strongly prefer 'en' over 'en_US' or 'en_GB'). 

To include a language variation of a field, the field name should be suffixed with _ and the appropriate language tag. For example: ```title_es``` for Spanish.

### Example

A contract is for ‘Software consultancy services’ may be published in a release with the default language sent to ‘en’ (the ISO-639-1 code for English). The following examples give the description of an item as English, French and Spanish.

<div class="tabbable">
<ul class="nav nav-tabs">
  <li class="active"><a href="#json" data-toggle="tab">json</a></li>
  <li><a href="#csv" data-toggle="tab">csv</a></li>
</ul>

<div class="tab-content">
<div class="tab-pane active" id="json">
<div class="include-json" data-src="standard/example/language.json"></div>
</div>
<div class="tab-pane" id="csv">
<div class="include-csv" data-src="standard/example/language.csv" data-table-class="table table-striped"></div>
</div>
</div>
</div>

The JSON Schema makes use of the JSON Scheme 0.4 [‘Pattern Properties](http://spacetelescope.github.io/understanding-json-schema/reference/object.html#pattern-properties)’ definition to allow validation of multi-language fields. 


## Field reference


### Classification

ToDo:


### Organization

ToDo: Update once organisation contact details confirmed.

### Attachment

### Date

OCDS makes use of [ISO8601](http://en.wikipedia.org/wiki/ISO_8601) date-times, following [RFC3339 §5.6](http://tools.ietf.org/html/rfc3339#section-5.6).

A time and timezone/offset MUST always be provided in a date-time.

The following are valid date-times:

* '2014-10-21T09:30:00Z' - 9:30 am on the 21st October 2014, UTC
* '2014-11-18T18:00:00-06:00' - 6pm on 18th November 2014 CST (Central Standard Time)

The following are not valid:

* '2014-10-21' - Missing time portion
* '2014-10-21T18:00' - missing seconds in time portion.
* '2014-11-18T18:00:00' - Missing timezone/offset portion
* '11/18/2014 18:00' - Not following the pattern at all!

Accurately including the time and timezone offsets is particular important for tender deadlines and other dates which may have legal significance, and where users of the data may be from different timezones. The character Z on the end of a date-time indicates the [UTC](http://en.wikipedia.org/wiki/Coordinated_Universal_Time) (or Zero offset) timezone, whereas other timezones are indicated by their value '+/-hh:mm' UTC on the end of the date-time value. 

In the event that the system from which data is drawn only includes dates, and does not include time information, publishers should consider sensible defaults for each field. For example, the startDate time of a clarification period may be set to '00:00:00Z' to indicate that clarifications can be requested from any time on the date stated, with the endDate time set to 23:23:59Z to indicate that clarifications can be sent up until the end of the endDate given. Alternatively, if clarification requests are only accepted in standard office hours, these values might be 09:00:00Z and 17:00:00Z respectively. 

In the event that a date field is not bound to a specific time at all, publishers should choose a default time value of '23:23:59' and either 'Z' (for UTC) or the timezone of the publisher, indicating that the time refers to the end of the given date. 

#### Period

A period is an object consisting of a start date and end date, represented as date-times.

### Item

<div class="include-csv" data-src="standard/docs/field_definitions/release-item.csv" data-table-class="table table-striped schema-table"></div>


### Milestone

ToDo: Review once updated

<div class="include-csv" data-src="standard/docs/field_definitions/release-milestone.csv" data-table-class="table table-striped schema-table"></div>

### Value

<div class="include-csv" data-src="standard/docs/field_definitions/release-value.csv" data-table-class="table table-striped schema-table"></div>

### Location

The addition of location information is handled through an extension to the specification, with an integral location element considered for future versions of the specification.