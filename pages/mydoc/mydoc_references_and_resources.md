---
title: References and Resources
last_updated: 2012-12-21
sidebar: mydoc_sidebar
permalink: mydoc_references_and_resources.html
folder: mydoc
---

## FDA
### Techincal Rejection Criteria

### SEND Validation
nformation can be found on the FDA Website: https://www.fda.gov/industry/fda-resources-data-standards/study-data-standards-resources

* [FDA Study Validator Rules](FDA/FDA-Validator-Rules.xlsx) (Excel)
   * Validator Rules, FDA source:  https://www.fda.gov/industry/fda-resources-data-standards/study-data-standards-resources  (then follow "Validator Rules" spreadsheet)
* [Technical Rejection Criteria](FDA/Technical-Rejection-Criteria-Final-v20190122.pdf)  (PDF)
   * FDA Source: https://www.fda.gov/media/100743/download
   
* [The eCTD Backbone File Specification for Study Tagging Files](FDA/STFV2-6-1.pdf)  (PDF)

* [Providing Regulatory Submissions In Electronic Format - Standardized study Data](FDA/ProvideRegSubInEleFormat-StandardizedStudyDataGuidance.pdf) (PDF)

## CDISC
### SEND ...


## SHACL
An introduction to SHACL is beyond the scope of this project. The following resources may be helpful:

* [Validating RDF Data](https://book.validatingrdf.com/) . Website version of an excellent book.
* [Data Validation and SHACL](https://fetch.stardog.com/training-data-validation/). Webinar by [Stardog Union](https://www.stardog.com/) illustrating their implementation of SHACL and their Stardog Studio environment.
* [W3C SHACL Spec](https://www.w3.org/TR/shacl/)

* [Designing Resusable SHACL Shapes and Implement a Linked Data Validation Pipeline](https://journal.code4lib.org/articles/14711)


## Ontology
<font class='toBeAdded'>Placeholder for references about ontology and ontology development. Temporarily contains information about the ontologies used in the
project, to be moved to other pages.</font>

Placeholder for Ontology documentation.
***TO BE UPDATED***

# Future Plans
Ontology development is taking place in TopBraid as of the end of 2019. The team plans to move to Web Protege in 2020. This page will be updated when the move occurs.  


## Ontology Notes from AO 
...On ontology files and related components created using TopBraid.

The protocol file imports the study ontology. The instance file imports the protocol file. When ready to "publish" to SEND format it also imports the SEND ontology into a new file called:  cj160500send.shapes.ttl

 cj160500send.shapes.ttl has everything integrated and linked into one mega-graph, so that SHACL rules pertaining to data quality have a home by linking to resources under the study: namespace and SHACL rules pertaining to SEND conformance issues can be linked to resources under the send: namespace. For example, time interval is a universal concept with start date must be before end date is a universal, "data quality" type of issue, and sure enough the rules can be linked to the time:Interval class. On the other hand, the fact that a SEND domain like DM or TS must be present, this is a SEND specific conformance rule that can be linked to the send:SENDDomain_DM  or send:SENDDomain_TS class.
 
send.ttl file is a bare bones ontology written by AO from scratch to support the pilot. It in essence documents the requirements for what a robust SEND ontology should be able to support at a minimum to allow round-tripping and now, SHACL rules validation.  It would be useful if other team members could look at this work and try replacing this rudimentary ontology with something more robust, capable of supporting all the domains not just DM/TS.


{% include links.html %}
