---
title: SEND Data Conversion
last_updated: 2019-12-19
sidebar: mydoc_sidebar
permalink: mydoc_senddata_data_conversion.html
folder: mydoc
---

# SEND Data Conversion and Mapping

## Introduction

The prototype uses data from the study "RE Function in Rats", located in the repository
at [/data/studies/RE Function in Rats](https://github.com/phuse-org/SENDConform/tree/master/data/studies/RE%20Function%20in%20Rats) and is limited to the Demographics (DM) and Trial Summary (TS) domains. Original source data is converted to .TTL using the driver script [r/driver.R](https://github.com/phuse-org/SENDConform/blob/master/r/driver.R) as described on the [Data Conversion](DataConversion.md) page.

The converted RDF data used for developing SHACL is available here: [SHACL/CJ16050Constraints/DM-CJ16050-R.TTL](https://github.com/phuse-org/SENDConform/blob/master/SHACL/CJ16050Constraints/DM-CJ16050-R.TTL)

Instructions on how to create validation reports in Stardog is available on the [Running Validation Reports](SHACL-RunValReport.md) page.

# SHACL Constraints

A detailed description of SHACL syntax is beyond the scope of this document. Please refer to the [SHACL Introduction](SHACL-Intro.md) page for a list of resources.

The SHACL shapes used in this project are available here:
* AnimalSubjectShape  [SHACL/CJ16050Constraints/SHACL-AnimalSubject.TTL](https://github.com/phuse-org/SENDConform/blob/master/SHACL/CJ16050Constraints/SHACL-AnimalSubject.TTL).


The proof of concept starts with the Demographics (<b>DM</b>) and Trial Summary (<b>TS</b>) domains. Additional domains will be added as the project progresses. Data was copied from the PHUSE Scripts repository [SEND subfolder](https://github.com/phuse-org/phuse-scripts/tree/master/data/send) and to our repository at: /SENDConform/data/studies/<font class="parameter">Study Name</font>  .

The easiest way to convert this source data to RDF would be to read in the row-by-column source data and convert it directly to RDF, using column names to identity the types of entities represented by the values in each cell, with the rows representing individuals. This is not the approach taken in this project. Data is re-formed to match an ontology that contains the types of entities and their relationships in the trial, based on knowledge of both the data and the clinical trial process.


Two alternate methods are provided for the data conversion process. In method one, R scripts read the source SAS XPT file and convert it to TTL for import into a triplestore. The same data conversion R scripts simultaneously create .CSV files to support a second method of importing data into a Stardog triplestore [using Stardog Mapping Syntax (SMS)](DataMapping-StardogSMS.md). ***CAUTION: The .CSV files do not contain the full set of data for evaluating the test cases.***

Additional data conversion methods using SAS or Python may be developed, time and expertise permitting.

## Source Data

SAS transport XPT data files for conversion and testing are located in the folder:
<pre>
  SENDConform/data/studies/<font class="parameter">Study Name</font>
</pre>

These files are read in by the conversion scripts. Converted data is exported to the /ttl folder:
<pre>
    SENDConform/data/studies/<font class="parameter">Study Name</font>/<b>ttl</b>
</pre>

The /csv folder contains data in comma-delimited format for ease of viewing the data in  Excel. These files are raw, source data and are not used for mapping into the triplestore.

<pre>
    SENDConform/data/studies/<font class="parameter">Study Name</font>/csv
</pre>

## Test Case Data

The data conversion process adds observations as violations to the various SHACL shapes that represent the rule components. Test observations are identified by `subjid` and `usubjid` values containing the pattern 99T<font class='parameter'>n</font>, in contrast to the original study data values of 00M0<font class='parameter'>n</font>. Test cases are documented in the file [TestCases.xlsx](https://github.com/phuse-org/SENDConform/blob/master/SHACL/CJ16050Constraints/TestCases.xlsx)


## General Guidance

<a name='AnimalUniqueID'></a>
### Generating Unique Identifiers for Animal Subjects
It may seem reasonable to use SUBJID or USUBJID when forming IRIs for Animal Subjects. IRI creation is simple and the human-readable value facilitates traceability back to the original source. For example, the IRI for Subject 00M01 would be:
`cj16050:Animal_00M01`

However, the use of SUBJID is fraught with problems. Consider cases where:

* A SUBJID is accidentally re-used and assigned to more than one animal. Two unique individuals would have the same ID number and the resulting RDF would have all observations assigned to a single IRI. It would be difficult to detect this duplication after data is converted to the graph.

* The same animal is accidentally assigned two different SUBJID values. Values are incorrectly assigned to two separate individuals.

* A row of data is accidentally duplicated, a condition that could go undetected when converting the data to RDF.

A solution is to create IRIs for critical components like **Animal Subject** and **Reference Interval** that are independent from values in the source data. For the purpose of this prototype, a truncated SHA-1 hash of a randomly generated value (with a known seed value) is used to create select IRIs where missing, duplicate, or partial data would be problematic.

When this method of IRI generation is followed:

* IRIs remain constant throughout multiple project development runs over time.
* IRIs for subjects, intervals, and other critical components become independent of the source data.
* Testing for duplicate, missing, and incorrect instance data becomes possible thanks to IRIs that are independent from instance data.

Example Animal Subject IRI: `cj16050:Animal_a6d09184`

Methods to generate UIDs for subjects in real-world settings is beyond the mandate of this project.See the [Technical Details page](https://github.com/phuse-org/UIDPharma/blob/master/UUIDTechDetails.md)) of the project [Unique Identifiers for the Pharmaceutical Industry](https://github.com/phuse-org/UIDPharma) for more information on generating unique identifiers.


### Reference Interval IRIs

The modeling of Reference Intervals for Animal Subjects require some explanation as they may not be constructed in a way that you expect.

Date values for reference start date (rfstdtc) and reference end date (rfendtc) are not directly attached to the Animal Subject IRI. Rather, the Animal Subject IRI <code>cj16050:Animal_<font class="parameter">hashvalue</font></code> is attached to a Reference Interval IRI <code>cj16050:Interval_<font class="parameter">hashvalue</font></code> which in turn has two date IRIs attached via the `time:hasBeginning` and `time:hasEnd` predicates (**Figure 1**).  

<img src="images/RefIntervalStructureDateFail.PNG"/>

**Figure 1: Animal_99T1 (incomplete data)**

Reference Interval IRIs are created even when either start date or end date is missing (**Figure 2**), because the data for the corresponding non-missing date must still be represented in the graph. A Reference Interval is  also be created when ***both*** start and end dates are missing, showing that the concept of the interval is still present but the data supporting it is not available.

<img src="images/RefIntervalStructureMissEndDate.PNG"/>

**Figure 2: Animal_99T5 Missing rfendtc**

See the [Animal Subject Reference Interval](SHACL-AnimalSubject-ReferenceInterval-Details.md) page for how SHACL shapes are constructed based on this model.


### RDF Project Conventions
#### Labels

* `skos:prefLabel` is the primary label used in the graph. For controlled terms, `skos:prefLabel` contains the industry standard (CDISC) label, which is often in plural form (DAYS, WEEKS, etc.) while `rdfs:label` contains the W3C standard in singular form (DAY, WEEK, etc.). `rdfs:label` is optional for all other triples.

# Conversion Details

## R Programs

R scripts for data conversion are located in the `/r` folder.

| Order  | File                 | Description                                  |
| ------ | -------------------- | ---------------------------------------------|
| 1.     | driver.R             | Main driver program for data conversion. Graph metadata creation.|
| 2.     | DM-convert.R         | DM instance data conversion to TTL, addition of observations to test constraints. (Under construction)|
| 3.     | TS-convert.R         | TS instance data conversion to TTL, addition of observations to test constraints. (Not yet written)|


## Graph Metadata
Graph metadata, including data conversion date and graph version, is created within the **driver.R** script and exported to a TTL file for upload into the triplestore. A corresponding .csv file is created for SMS mapping purposes.

The .csv and .ttl files are located in the folder:  `\data\studies\<font class="parameter">Study Name</font>\ttl`

| File      | Role                     | Description                                  |
| --------- | ------------------------ | ---------------------------------------------|
|Graphmeta-<font class='parameter'>StudyName</font>.csv | Basic graph metadata | Description of graph content, status, version, and time stamp information. |
|Graphmeta-<font class='parameter'>StudyName</font>-map.TTL|SMS Map | Map CSV to Stardog graph. |
|Graphmeta-<font class='parameter'>StudyName</font>.TTL| RDF Triples | TTL file for loading directly into triplestore. |


## DM


The .csv and .ttl files are located in the folder:  `\data\studies\<font class="parameter">Study Name</font>\ttl`

| File      | Role                     | Description                                  |
| --------- | ------------------------ | ---------------------------------------------|
| DM-CJ16050.CSV | Demographics        | May be a subset during development.
| DM-CJ16050-R-map.TTL | SMS Map       | Map CSV to Stardog graph.
| DM-CJ16050-R.TTL | RDF Triples       | TTL file for loading directly into triplestore.

### Considerations for Study: CJ16050
#### Data Imputation

Creation of values not in the original study data, or located in domains that are not part of the pilot include:.

| Variable     | Value(s)            | Description                                  |
| ------------ | ------------------- | ---------------------------------------------|
| SPECIESCD_IM |  "Rat"              | Species Code not specified in DM data file.
| AGEUNIT_IM   |  "Week"             | A representation of the age unit that is used to link to time namespace.
| DURATION_IM  | "P56D"              | Duration code, derived from 8 weeks x 7 days/wk.


#### Data
-   **/data/studies/RE Function in Rats/ttl**

File  | Description    | Contact
---------|----------------------------------|-----------------------------------
cj16050.ttl | Instance data file. Outdated as of 2019-08-02 |
cjprot.ttl | Nonclinical study protocol file for study CJ16050 | AO
cj160500send.shapes.ttl | Combines the instance file with the SEND ontology to support automated SEND dataset creation. It currently recreates the first record of the pilot DM domain. TS not yet included.  | AO
DM-CJ16050-R.csv | Data file created by R for mapping DM domain data to triplestore using SMS | TW
DM-CJ16050-R.TTL | Data file created by R for direct load into triplestore | TW
DM-CJ16050-R-map.TTL | SMS map for DM-CJ16050-R.csv  to Stardog
Graphmeta-CJ16050.csv | Graph metadata file for mapping to triplestore using SMS | TW
GraphMeta-CJ16050.TTL | Graph metadata file for direct load into triplestore  | TW
Graphmeta-CJ16050-map.TTL | SMS map for Graphmeta-CJ16050.csv to Stardog | TW
SENDConform-CJ106050LoadDriver.bat | Driver .BAT file that calls SENDConform-CJ106050LoadSequence.bat. Needed for Windows. | TW
SENDConform-CJ106050LoadSequence.bat | Loads data into Stardog using a series of SMS calls.
study.ttl | Ontology file from the CTDasRDF project, updated to support nonclinical data | AO
send.ttl |  "bare bones" SEND ontology to allow exporting protocol and instance data into SEND format. | AO


### TS

*Future development*

# Data Mapping with Stardog SMS

## Introduction
Stardog Mapping Syntax (SMS) [(stardog.com)](https://www.stardog.com/docs/#_stardog_mapping_syntax) is provided as an alternative data mapping and upload process. The same data conversion scripts that produce TTL files for upload into a triplestore also create a .CSV file that can be mapped to the database. ***The .CSV files do not contain the full set of data for evaluating the test cases.***


Why create this redundancy when the data does not contain the full set of test cases? The team benefits from having an RShiny app that reads in the SMS file and produces a visualization of the data schema. This schema is used during development to help ensure the nodes and relations are being constructed correctly. The visualization also aids in SHACL Shape and SPARQL query development.


## Conversion and Mapping Details
The [Data Conversion and Mapping](DataConversion.md) page documents the source data and R Scripts used to create the .CSV files used by the SMS maps, including generation of values like SHA-1 hashes used in both methods.

Each CSV file has a corresponding map file in TTL format with "-map" appended to the name.

### Graph Metadata

| File      | Role                     | Description                                  |
| --------- | ------------------------ | ---------------------------------------------|
|Graphmeta-<font class='parameter'>StudyName</font>.csv | Basic graph metadata | Description of graph content, status, version, and time stamp information. |
|Graphmeta-<font class='parameter'>StudyName</font>-map.TTL|SMS Map | Map CSV to Stardog graph. |

### DM

| File      | Role                     | Description                                |
| --------- | ------------------------ ---------------------------------------------|
| DM-CJ16050.CSV | Demographics        | May be a subset during development.
| DM-CJ16050-R-map.TTL | SMS Map       | Map CSV to Stardog graph.

## SMS Format
The SMS files follow very strict formatting rules that go beyond the Stardog specification, primarily due to weak parsing expressions in the RShiny visualization code (this can easily be improved!). These rules include:
* `subject` is <i>hard left</i> on line by itself.
* `predicate`, `object` line:
    * indented at least one space.
    * end with a `;` on same line, no trailing spaces
* No short hand for `predicate`s. Use 'rdf:type', not 'a' .
* File must end with carriage return on a line by itself.

This excerpt from the DM domain mapping file shows the AnimalSubject triples.  Values within `{ }` are substituted from the named columns in the .CSV file as the file is processed line-by-line.

<pre class="sms">
# Animal Subject
cj16050:Animal_{DMROWSHORTHASH_IM}
  rdf:type                    study:AnimalSubject ;
  skos:prefLabel              "Animal {<font class='nodeBold'>subjid</font>}"^^xsd:string ;
  study:hasReferenceInterval  cj16050:Interval_{<font class='nodeBold'>DMROWSHORTHASH_IM</font>} ;
  study:hasSubjectID          cj16050:SubjectIdentifier_{<font class='nodeBold'>subjid</font>} ;
  study:hasUniqueSubjectID    cj16050:UniqueSubjectIdentifier_{<font class='nodeBold'>usubjid</font>} ;
  study:memberOf              cjprot:Set_{<font class='nodeBold'>setcd</font>} ;
  study:memberOf              code:Species_{<font class='nodeBold'>SPECIESCD_IM</font>} ;
  study:participatesIn        cj16050:AgeDataCollection_{<font class='nodeBold'>DMROWSHORTHASH_IM</font>} ;
  study:participatesIn        cj16050:SexDataCollection_{<font class='nodeBold'>DMROWSHORTHASH_IM</font>} ;
.
</pre>

## Data Upload
Mapping and upload is accomplished by issuing a series of import commands similar to the following, where the database (SENDConform) is specified followed by the mapping file and then the CSV file:

`stardog-admin virtual import SENDConform DM-CJ16050-R-map.TTL DM-CJ16050-R.CSV`

A series of these commands is chained together in a batch file to upload all graphs at the same time, including additional files like the supporting ontology.


## Visualization
An RShiny app for visualization the SMS files is available at [/r/vis/SMSMapVis-appSEND](https://github.com/phuse-org/phuse-scripts/tree/master/r/vis/SMSMapVis-app). Paths within the file `global.R` must change to point to your local clone of the repository.  **Figure 1** shows a screen shot of the SMS files for the DM and Graph Metadata portions of the graph.

<img src="images/SMS-Map-RShinyVis.PNG"/>

**Figure 1: Screen shot from RShiny SMS visualization.**


{% include links.html %}
