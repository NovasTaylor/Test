---
title: Graph Data and Validation
last_updated: 2020-12-20
sidebar: mydoc_sidebar
permalink: mydoc_intro_graph_data_and_validation.html
folder: mydoc
---

## Introduction 

<font class='toBeAdded'>Content to be added...</font>

## Graph Data
Source data for this project is converted to Resource Description Framework (RDF), where a Subject Node is linked to an Object Node by a Predicate that supplies a meaningful relation between the two nodes.

<img src="images/SubjectPredicateObject.PNG" width="500">

**Figure 1: Building Blocks of RDF: Subject, Predicate, Object**


Nodes and their relationships join together to create a network of relationships that can be described as a graph of data. The graph for a specific clinical trial has a shape that is defined by the entities and relationships within the graph. Entities within the graph denote like an Animal Subject, or a Treatment Arm have their shape defined by the data and relationships attached to them. Individual nodes have attributes that can be validated (node constraints) as can the incoming and outgoing relations from a node and the values associated with those connections.

## SHACL

When data has shape, it makes sense to define validation rules in terms of shapes. This is accomplished in RDF using the W3C Standard **Shapes Contraint Language (SHACL)**.  The book [Validating RDF Data](<https://book.validatingrdf.com/>) is an excellent resource for learning more about SHACL. 

<img src="images/SHACLShapeConcept.PNG"/>

**Figure 2: SHACL Shapes Concept for Data Validation**


Individual FDA SEND rules are deconstructed into their constituent components. Each rule is interpreted, transformed to its corresponding SHACL shape, applied to example data to generate a validation report, then confirmed independently using SPARQL.

{% include links.html %}
