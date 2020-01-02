---
title: Conventions
last_updated: 2019-12-19
sidebar: mydoc_sidebar
permalink: mydoc_intro_conventions.html
folder: mydoc
---


## Validation Language
Shapes Constraint Language ([https://www.w3.org/TR/shacl/](SHACL)) is used to describe and validate the [https://www.w3.org/RDF/](RDF) data used in the prototype. A detailed description of SHACL syntax is beyond the scope of this document. Please refer to the [References and Resources](mydoc_references_and_resources.html) page to learn more about SHACL.

Data sources and their conversion from tabular form to RDF is described in within each sub-project.

## Content Conventions

Color coding provides a guide to the content.

<div class='ruleState'>
  <div class='ruleState-header'>Rule Statement</div>
  Grey boxes contain brief syntax / pseudo code for the rule.
</div>

<div class='def'>
  <div class='def-header'>Description</div>
  Blue boxes contain a textual description of the rule.
</div>

<pre class="data">
   This box contains a subset of data that serves as input to test the shapes graph.
   Intentional error values are <font class='error'>highlighted in red.</font>
   Data not relevant to the discussion is and omitted is shown as <font class='infoOmitted'>...</font>
</pre>

<pre class="sms">
   Stardog Mapping Syntax, used to import CSV files to the database.
</pre>

<pre class="owl">
   Contains an excerpt from an ontology that applies to the rule being described.
   Optional. Not all rules rely on the project ontologies.
</pre>

<pre class="shacl">
  Contains a representation of the shapes graph (in full or in part).
</pre>

<pre class="report">
  Excerpts from the SHACL Validation Report (the output results graph.)
</pre>

<pre class="sparql">
  SPARQL commands to retrieve additional information based on values identified in the report or to validate the validation report.
</pre>

<pre class="queryResult">
  Results of a SPARQL query for tracing information from the Report back to additional information or to verify
  the SHACL constraint is catching all test cases.
</pre>



{% include links.html %}
