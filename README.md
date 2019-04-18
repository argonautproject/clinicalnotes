## Publishing Information

**Publish History** http://www.fhir.org/guides/argonaut/clinicalnotes/history.shtml

**IG code:** `guides/argonaut/clinicalnotes`
<!--Proposed IG realm and code
What is the realm code (2-character country code or 'uv') and IG code to use for the path when the IG is published under http://hl7.org/fhir? E.g. us/ccda -->

**Short Description:** Guidance on creating and accessing Clinical Notes.
<!--1-2 sentences describing the purpose/scope of the IG for inclusion in the registry- this is the sentence that will be used here: http://www.fhir.org/guides/registry. This must describe the IG from the perspective of an implementer scanning a registry -->

**Long Description:** The goal of the Argonaut Clinical Notes Project is to develop guidance to create, use, and share Clinical Notes. The requirements were developed by the Argonaut Clinical Notes project team and tested through pilot implementations, and HL7 sponsored Connectathons. This guide is based on [FHIR Version 3.0.1](http://hl7.org/fhir/STU3/). The content and profiles used in this guide were submitted to the HL7 US Realm Steering Committee for consideration in the December 2018 ballot of US Core for FHIR Version R4.

<!-- 1(-2) paragraphs describing the purpose/scope of the IG in more detail for inclusion in the version history - this is content that will be used in your IG's equivalent of this: http://www.hl7.org/fhir/us/core/history.cfml. Again, this must describe the IG from the perspective of an implementer scanning a registry, it should not talk about the project and should NOT be copied from the PSS.  -->


# Welcome to the [Argonaut](http://argonautwiki.hl7.org/index.php?title=Main_Page) Clinical Notes Profile

Argonaut Lead: [Micky Tripathi](mtripathi@maehc.org)

Project Coordinator: [Jennifer Monahan](jmonahan@maehc.org)

FHIR SME and Facilitator: [Eric Haas](ehaas@healthedatainc.com)

FHIR SME and Facilitator: [Brett Marquard](brett@waveoneassociates.com)


## Scope of Work

Support basic patient and provider access to Clinical Notes. See [wiki](https://github.com/argonautproject/clinicalnotes/wiki) for timelines.

## Directory Tree

~~~
├── README.md
├── base.html
├── definitions.csv
├── dependencies
│   └── uscore-vp
├── docs
│   ├──{xhtml output}
├── ex.html
├── format.html
├── framework
│   ├── _includes
│   ├── _layouts
│   └── assets
├── generated_output
│   ├── qa
│   ├── temp
│   └── txCache
├── ig.json
├── meeting-notes
├── sd-definitions.html
├── sd-mappings.html
├── sd.html
└── source
    ├── examples
    ├── pages
    └── resources

20 directories,

~~~
