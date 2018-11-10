---
title: Argonaut Clinical Notes 
layout: default
active: home
topofpage: true
---

{:.no_toc}

<!-- TOC  the css styling for this is \pages\assets\css\project.css under 'markdown-toc'-->

<!-- * Do not remove this line (it will not be displayed)
{:toc} -->

<!-- end TOC -->

## Introduction 

This implementation guide provides implementers with FHIR profiles and guidance to create, use, and share Clinical Notes. The requirements were developed by the Argonaut Clinical Notes project team and tested through pilot implementations, and HL7 sponsored Connectathons.  This guide is based on [FHIR Version {{site.data.fhir.version}}]({{site.data.fhir.path}}) The content and profiles used in this guide will be submitted to the HL7 US Realm Steering Committee for consideration in a future ballot of [US Core] for FHIR Version R4

## Argonaut Actors

The following actors are part of the Clinical Notes IG:

* Argonaut Requestor: An application that initiates a data access request to retrieve patient data. This can be thought of as the client in a client-server interaction.
* Argonaut Responder: A product that responds to the data access request providing patient data. This can be thought of as the server in a client-server interaction.


## Argonaut Clinical Notes Profiles

This implementation guide includes two profiles for exchanging clinical notes: 

1. [Argonaut Clinical Notes Profile] which is based upon the  [DocumentReference] resource.
1. [Argonaut Diagnostic Report Profile] which is based upon the  [DiagnosticReport] resource.

See the [DocumentReference vs DiagnosticReport] section for detailed guidance on when to use each profile.  Each profile defines the minimum mandatory elements, extensions and terminology requirements that **MUST** be present. The profile requirements and guidance are given in a simple narrative summary and a formal [logical views] of the content with references to appropriate terminologies.  Examples and "Quick Start" sections are provided to help implementers with easy API onboarding.

## Argonaut Clinical Notes Conformance Requirements

The [Capability Statements Section](capstatements.html) outlines conformance requirements for the Argonaut Clinical Notes Servers and Client applications, identifying the specific profiles that need to be supported, the specific RESTful operations that need to be supported, and the search parameters that need to be supported. Note: The individual Argonaut Clinical Notes profiles identify the structural constraints, terminology bindings and invariants, however, implementers must refer to the conformance requirements for details on the RESTful operations, specific profiles and the search parameters applicable to each of the Argonaut Clinical Notes actors.

<br/>

{% include link-list.md %}
