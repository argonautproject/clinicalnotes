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

This implementation guide provides implementers with FHIR profiles and guidance to create, use, and share Clinical Notes. The requirements were developed by the Argonaut Clinical Notes project team and tested through pilot implementations, and HL7 sponsored Connectathons. The implementation guide is based on [FHIR Version {{site.data.fhir.version}}]({{site.data.fhir.path}}) with profiles on [DocumentReference] and [DiagnosticReport].

This profiles included here will be submitted to the HL7 US Realm Steering Committee for consideration in a future ballot of [US Core].

## Argonaut Actors

The following actors are part of the Clinical Notes IG:

* Argonaut Requestor: An application that initiates a data access request to retrieve patient data. This can be thought of as the client in a client-server interaction.
* Argonaut Responder: A product that responds to the data access request providing patient data. This can be thought of as the server in a client-server interaction.


## Argonaut Clinical Notes Profiles

This implementation guide includes a [Argonaut Clinical Notes] profile and a [Argonaut Diagnostic Reports] profile. These profile define the minimum mandatory elements, extensions and terminology requirements that **MUST** be present. The profile requirements and guidance are given in a simple narrative summary. A formal hierarchical table that presents a [logical view] of the content in both a differential and snapshot view is also provided along with references to appropriate terminologies and examples.  In addition it has a "Quick Start" section which is intended as an implementer friendly overview of the required search and read operations.

[Argonaut Clinical Notes]

[Argonaut Diagnostic Reports]

See the [DocumentReference vs DiagnosticReport] section for detailed guidance on when to use each profile.

## Argonaut Clinical Notes Conformance Requirements

The [Capability Statements Section](capstatements.html) outlines conformance requirements for the US Core Servers and Client applications, identifying the specific profiles that need to be supported, the specific RESTful operations that need to be supported, and the search parameters that need to be supported. Note: The individual US Core profiles identify the structural constraints, terminology bindings and invariants, however, implementers must refer to the conformance requirements for details on the RESTful operations, specific profiles and the search parameters applicable to each of the US Core actors.

<br/>

{% include link-list.md %}
