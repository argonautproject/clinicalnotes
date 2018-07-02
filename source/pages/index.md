---
title: Argonaut Clinical Notes 
layout: default
active: home
topofpage: true
---

{% include publish-box.html %}
{:.no_toc}

<!-- TOC  the css styling for this is \pages\assets\css\project.css under 'markdown-toc'-->

<!-- * Do not remove this line (it will not be displayed)
{:toc} -->

<!-- end TOC -->

## Introduction

This implementation guide provides implementers with FHIR profiles and guidance to create, use, and share Clinical Notes. The requirements were developed by the Argonaut Clinical Notes project team and tested through pilot implementations, and HL7 sponsored Connectathons. The implementation guide is based on [FHIR Version {{site.data.fhir.version}}]({{site.data.fhir.path}}) with profiles on [DocumentReference] and [CapabilityStatement].

This profiles included here will be submitted to the HL7 US Realm Steering Committee for consideration in a future ballot of [US Core].

## Argonaut Actors

The following actors are part of the Clinical Notes IG:

* Argonaut Requestor: An application that initiates a data access request to retrieve patient data. This can be thought of as the client in a client-server interaction.
* Argonaut Responder: A product that responds to the data access request providing patient data. This can be thought of as the server in a client-server interaction.


## Argonaut Clinical Notes Profiles

This implementation guide includes a single [Argonaut Clinical Notes] profile. The profile defines the minimum mandatory elements, extensions and terminology requirements that **MUST** be present. The profile requirements and guidance are given in a simple narrative summary. A formal hierarchical table that presents a [logical view] of the content in both a differential and snapshot view is also provided along with references to appropriate terminologies and examples.  In addition it has a "Quick Start" section which is intended as an implementer friendly overview of the required search and read operations.

[Argonaut Clinical Notes]


## Argonaut Clinical Notes Conformance Requirements

The [Capability Statements Section](capstmnts.html) outlines conformance requirements for the US Core Servers and Client applications, identifying the specific profiles that need to be supported, the specific RESTful operations that need to be supported, and the search parameters that need to be supported. Note: The individual US Core profiles identify the structural constraints, terminology bindings and invariants, however, implementers must refer to the conformance requirements for details on the RESTful operations, specific profiles and the search parameters applicable to each of the US Core actors.





## Ignore Content Below!

Figure 1 is a picture of a cat to show how to insert an image using markdown.

{% include img.html img="cat.jpg" caption="Figure 1: Meow" %}

#### Jekyll Site Variables

These are the site variables defined [here](http://wiki.hl7.org/index.php?title=IG_Publisher_Documentation#Jekyll):

- IG Business version specification (defined in ig.json)- {% raw %}{{site.data.fhir.ig.version}} {% endraw %} = {{site.data.fhir.ig.version}}

- IG status (defined in ig.xml)- {% raw %}{{site.data.fhir.ig.status}} {% endraw %} = {{site.data.fhir.ig.status}}

- Whether is experimental IG (defined in ig.xml) - {% raw %}{{site.data.fhir.ig.experimental}} {% endraw %} = {{site.data.fhir.ig.experimental}}

- IG Publisher name (defined in ig.xml) - {% raw %}{{site.data.fhir.ig.publisher}} {% endraw %} = {{site.data.fhir.ig.publisher}}

- dependency url - e.g. "uscore" : Base url of a dependency implementation Guide (defined in ig.json) -  {% raw %} {{site.data.fhir.uscore}} {% endraw %}= {{site.data.fhir.uscore}}

- igName : Title of the implementation Guide (defined in ig.xml) -  {% raw %} {{site.data.fhir.igName}} {% endraw %}= {{site.data.fhir.igName}}

- path : path to the main FHIR specification (defined in ig.json)-  {% raw %} {{site.data.fhir.path}} {% endraw %}= {{site.data.fhir.path}}

- canonical : canonical path to this specification (defined in ig.json)-  {% raw %} {{site.data.fhir.canonical}} {% endraw %} = {{site.data.fhir.canonical}}

- errorCount : number of errors in the build file (not including HTML validation errors) -  {% raw %} {{site.data.fhir.errorCount}} {% endraw %} = {{site.data.fhir.errorCount}}

- version : version of FHIR -  {% raw %} {{site.data.fhir.version}} {% endraw %} = {{site.data.fhir.version}}

- revision : revision of FHIR -  {% raw %} {{site.data.fhir.revision}} {% endraw %} = {{site.data.fhir.revision}}

- versionFull : version-revision -  {% raw %} {{site.data.fhir.versionFull}} {% endraw %} = {{site.data.fhir.versionFull}}

- totalFiles : total number of files found by the build -  {% raw %} {{site.data.fhir.totalFiles}} {% endraw %} = {{site.data.fhir.totalFiles}}

- processedFiles : number of files genrated by the build -  {% raw %} {{site.data.fhir.processedFiles}} {% endraw %} = {{site.data.fhir.processedFiles}}

- genDate : date of generation (so date stamps in the pages can match those in the conformance resources) -  {% raw %} {{site.data.fhir.genDate}} {% endraw %} = {{site.data.fhir.genDate}}


### More Stuff

#### And More Stuff

[US Core]: http://hl7.org/fhir/us/core/index.html
[Argonaut Clinical Notes]: StructureDefinition-argo-clinicalnotes.html
[logical view]: {{site.data.fhir.path}}/formats.html#table
[DocumentReference]: {{site.data.fhir.path}}/documentreference.html
[CapabilityStatement]: {{site.data.fhir.path}}/capabilitystatement.html

{% include link-list.md %}
