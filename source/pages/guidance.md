---
title: General Guidance and Definitions
layout: default
active: guidance
---

{:.no_toc}

<!-- TOC  the css styling for this is \pages\assets\css\project.css under 'markdown-toc'-->

<!--* Do not remove this line (it will not be displayed)
 {:toc} -->

### Clinical Notes

Clinical Notes are a key component to communicate the current status of a patient. Notes support transitions of care, care planning, quality reporting, billing and many other care activities. This implementation guide does not define new note types or set content requirements per note type. Instead, this implementation guide focuses on unlocking Clinical Notes captured in existing systems. 

Specifically, this implementation guide requires systems support the following 5 common clinical notes:

* Consultation Note (11488-4)
* Discharge Summary (18842-5)
* History & Physical Note (34117-2)
* Procedures Note (28570-0)
* Progress Note (11506-3)

These notes are defined in the [Argonaut Clinical Types Value Set]. These 5 note types represent the Common Clinical Notes and are the minimum set a system must support to claim conformance to this guide. Systems are encouraged to support many other common notes types, such as:

* Imaging
* Pathology narrative
* Cardiology Reports 
* Referral Note
* Surgical Operation Note
* Nurse Note

The Argonaut project team developed this initial list of 5 through a survey of the participants in Argonaut and the US Veterans Administration (VA).

### DocumentReference vs DiagnosticReport

The FHIR specification supports sharing narrative-only reports, or notes, in the DocumentReference and DiagnosticReport Resources. When reviewing the minimal number of elements required for each Resource, it's difficult to differentiate which is best. A few characteristics this guide considered to help systems differentiate:

* Discrete result information
* Note types
* Consistent Client application access to scanned, or narrative-only, reports

DiagnosticReport is the best choice when a system needs to share discrete information or coded interpretations. The DiagnosticReport Resource includes explicit structures (DiagnosticReport.result) to support this information and the entire narrative report (DiagnosticReport.presentedForm). 

DocumentReference is the best choice when the narrative is broader then a specific order or report, such as a Progress Note or Discharge Summary Note. The DocumentReference Resource can point to a short 2-3 sentence status of the patient, or reference a complex CDA or Composition document which can include narrative and discrete information. 

The best practice for consistent scanned, or narrative-only, report access for client applications is more challenging due to variability in system implementations.  

For example, some systems consider any scanned report, or note, a DocumentReference. Other systems allow users to categorize the scanned report as Lab and store to DiagnosticReport. With this variability reports may end up in either resource as demonstrated by the green area in Figure 1.

{% include img.html img="DiagnosticReport_DocumentReference_Resource_Overlap.png" caption="Figure 1: DiagnosticReport and DocumentReference Report Overlap" %}
	
Categorizing the scanned report as a DiagnosticReport enables clients to access the unstructured reports along with structured information. For example, a client can request all DiagnosticReport.category="LAB" and receive reports with discrete information and any scanned reports. However, not all systems categorize scanned reports. 

In order to enable consistent access to scanned narrative-only reports the Argonaut servers agreed to expose these reports through both DiagnosticReport and DocumentReference.

  When DiagnosticReport.presentedForm (Attachment) references a Scan (PDF), then that Attachment **SHALL** also be accessible through DocumentReference.content.attachment.
	
Exposing the content from both Resources guarantees a Client will receive the clinical information available for a patient. The DocumentReference and DiagnosticReport resources representing the same data will point to the same attachment, so a client can easily identify these duplicates. 

* DocumentReference.content.attachment.url
* DiagnosticReport.presentedForm.url 
 
 
 
	**What else should be added?**
Should the guide include:

* Systems SHOULD categorize scanned narrative-only reports to provide consistent access to client applications????

* The designers of this guide considered an custom operation to allow a client to ask for all but decided against since a variety of resources would be returned AND clients might not find the operation and inappopriately ask for all of one resource and not get all the ifnormaiotn resulting in patient safety issue. 

DocumentReference.content.attachment (Attachment) and DiagnosticReport.presentedForm (Attachment) 


### Selection of DocumentReference

* Include history of resources considered, plus a new resource
* Discussion on flipping to think about different formats rather than resources.
*


### Clinical Notes vs ClinicalImpression

The [FHIR Version {{site.data.fhir.version}}]({{site.data.fhir.path}}) includes the [ClinicalImpression] resource to support the record of a clinical assessment. 

Taken from the Resource Definition:

A record of a clinical assessment performed to determine what problem(s) may affect the patient and before planning the treatments or management strategies that are best to manage a patient's condition. Assessments are often 1:1 with a clinical consultation / encounter, but this varies greatly depending on the clinical workflow. This resource is called "ClinicalImpression" rather than "ClinicalAssessment" to avoid confusion with the recording of assessment tools such as Apgar score

* How do systems expect to store and expose "Progress satisfactory, continue with treatment"?
* Will use 7/18/2018 call to flesh out 


### ValueSet Support on Read vs Write ($expand enhancement)

During the May 2018 Cologne Connectathon the participants identified a requirement to support different value sets on read vs write. The participants considered a new extension for the CapabilityStatement to capture this requirement, or a new operation which would allow a client to ask which value sets are supporting at a specific resource element and a given direction. After discussion Argonaut discussion with the FHIR Core team they agreed to enhance [$expand]({{site.data.fhir.path}}/valueset-operations.html#expand) operation. See [gForge #17321](https://gforge.hl7.org/gf/project/fhir/tracker/?action=TrackerItemEdit&tracker_item_id=17321&start=0) for latest details. 

Example use location: DocumentReference.content.attachment.contentType

* System A accepts contentType "text/plain" in a create and returns "text/html" in a read.
* System B accepts contentType "text/xhtml" in a create and returns "application/pdf" in a read.

**ADD specific details when posted to current build**

### Future Work

Expand the number of notes systems must support.



#### Usage

example how to use a button to expand an inline example....

{% include examplebutton.html example="foo" %}

{% include link-list.md %}

[ClinicalImpression]: {{site.data.fhir.path}}/clinicalimpression.html
[Argonaut Clinical Types Value Set]: ValueSet-argonaut-clinical-note-type.html