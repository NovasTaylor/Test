---
title: SEND Data Sources
last_updated: 2020-01-02
sidebar: mydoc_sidebar
permalink: mydoc_senddata_data_sources.html
folder: mydoc
---

## Introduction

...Placeholder...

## Data Sources

The prototype uses data from the study "RE Function in Rats", located in the repository
at [/data/studies/RE Function in Rats](https://github.com/phuse-org/SENDConform/tree/master/data/studies/RE%20Function%20in%20Rats) and is limited to the Demographics (DM) and Trial Summary (TS) domains. Original source data is converted to .TTL using the driver script [r/driver.R](https://github.com/phuse-org/SENDConform/blob/master/r/driver.R) as described on the [Data Conversion](DataConversion.md) page.

The converted RDF data used for developing SHACL is available here: [SHACL/CJ16050Constraints/DM-CJ16050-R.TTL](https://github.com/phuse-org/SENDConform/blob/master/SHACL/CJ16050Constraints/DM-CJ16050-R.TTL)

Instructions on how to create validation reports in Stardog is available on the [Running Validation Reports](SHACL-RunValReport.md) page.

# SHACL Constraints

A detailed description of SHACL syntax is beyond the scope of this document. Please refer to the [SHACL Introduction](SHACL-Intro.md) page for a list of resources.

The SHACL shapes used in this project are available here:
* AnimalSubjectShape  [SHACL/CJ16050Constraints/SHACL-AnimalSubject.TTL](https://github.com/phuse-org/SENDConform/blob/master/SHACL/CJ16050Constraints/SHACL-AnimalSubject.TTL).



{% include links.html %}
