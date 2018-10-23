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
* Consistent Client access to scanned, or narrative-only, reports

DiagnosticReport is the best choice when a system needs to share discrete information or coded interpretations. The DiagnosticReport Resource includes explicit structures (DiagnosticReport.result) to support this information and the entire narrative report (DiagnosticReport.presentedForm). 

DocumentReference is the best choice when the narrative is broader then a specific order or report, such as a Progress Note or Discharge Summary Note. The DocumentReference Resource can point to a short 2-3 sentence status of the patient, or reference a complex CDA or Composition document which can include narrative and discrete information. 

The best practice for consistent scanned, or narrative-only, report access for client applications is more challenging due to variability in system implementations.  

For example, some systems consider any scanned report, or note, a DocumentReference. Other systems allow users to categorize the scanned report as Lab and store to DiagnosticReport. With this variability reports may end up in either resource as demonstrated by the green area in Figure 1.

{% include img.html img="DiagnosticReport_DocumentReference_Resource_Overlap.png" caption="Figure 1: DiagnosticReport and DocumentReference Report Overlap" %}
	
Storing scanned reports as a DiagnosticReport, with appropriate categorization, enables clients to access the scanned reports along with DiagnosticReports containing discrete information. For example, a client can request all DiagnosticReport.category="LAB" and receive reports with discrete information and any scanned reports. However, not all systems store and categorize Lab reports with DiagnosticReport.

The developers of this guide considered requiring Clients query both DocumentReference and DiagnosticReport to get all information for a patient. Querying both places would provide all the information for a patient. However, the requirement to query two places is potentially dangerous if a client doesn't understand this requirement and queries only one.

In order to enable consistent access to scanned narrative-only clinical reports the Argonaut servers agreed to expose these reports through both DiagnosticReport and DocumentReference.

  When DiagnosticReport.presentedForm (Attachment) references a Scan (PDF), then that Attachment **SHALL** also be accessible through DocumentReference.content.attachment.
	
Exposing the content in this manner guarantees a Client will receive the clinical information available for a patient. The DocumentReference and DiagnosticReport resources representing the same data will point to the same attachment, so a client can easily identify these duplicates. 

* DocumentReference.content.attachment.url
* DiagnosticReport.presentedForm.url 

Note, not all scanned information stored through DocumentReference will be exposed through DiagnosticReport since DocumentReference stores other non-clinical information (e.g. insurance card).

#### Support Requirements

This guide requires systems implement DocumentReference and at a minimum these [5 concepts](ValueSet-argonaut-clinical-note-type.html) and may extend to the full  [HITSP C80 Table 2-144 Document Class Value Set Definition](http://build.fhir.org/valueset-c80-doc-typecodes.html).

This guide requires systems implement DiagnosticReport and the `DiagnosticReport.category` element must support at a minimum the [3 concepts](ValueSet-diagnosticreport-category.html) of Cardiology, Radiology, and Pathology. Other categories may be supported. 

Clients interested in determining what server supports may use the [$expand operation.](guidance.html#determining-server-note-and-report-type-support-expand)

The contents of the notes or reports, even using standard LOINC concepts, may vary by health system or even location. For example, CT Spleen WO contrast (LOINC 30621-7) may include individual sections for history, impressions, conclusions, or just an impressions section. Discharge Summaries may have different facility or local regulatory content requirements.

This guide focuses on exposing existing information, and not dictating how systems allow their users to capture information.

#### Example Query Scenarios

A client interested in all Radiology reports can use the following query:

	GET [base]/DiagnosticReport?patient=[id]&category=http://loinc.org|LP29684-5
	
A client interested in all Clinical Notes can use the following query:

	GET [base]/DocumentReference?patient=[id]&class=clinical-note

A client interested in all Discharge Summary Notes can use the following query:

	GET [base]/DocumentReference?patient=[id]&type=http://loinc.org|18842-5

### Determining Server Note and Report Type Support ($expand) 

A client can determine the Note and Report type support of a server by invoking the $expand operation. The [$expand](http://build.fhir.org/valueset-operation-expand.html) operation in the base FHIR R4 specification provides many parameters and features. This guide highlights the use of the operation to discover what specific notes can be requested, and what write formats a server supports.

#### Discover Read and Write Support Using $expand 

Servers may support different read and write formats. For example,

* System A accepts contentType "text/plain" in a create and returns "text/html" in a read.
* System B accepts contentType "text/xhtml" in a create and returns "application/pdf" in a read.

Invoking this will determine what a server supports on write (create):

	GET [base]/ValueSet/$expand?context=DocumentReference.content.attachment.contentType&amp;contextDirection=incoming
	
Invoking this will determine what a server supports on read (access):

	GET [base]/ValueSet/$expand?context=DocumentReference.content.attachment.contentType&amp;contextDirection=outgoing
	
#### Discover Note and Report Type Using $expand 
	
Servers will support different Note and Report types. 

This allows a client to determine the types of Note or reports they can access through DiagnosticReport:

	GET [base]/ValueSet/$expand?context=DiagnosticReport.type&amp;contextDirection=outgoing

This allows a client to determine the types of Note or reports they can access through DocumentReference:

	GET [base]/ValueSet/$expand?context=DocumentReference.type&amp;contextDirection=outgoing
	
If a client is only interested in retrieving notes by categories they may use the following:

	GET [base]/ValueSet/$expand?context=DiagnosticReport.category&amp;contextDirection=outgoing
	GET [base]/ValueSet/$expand?context=DocumentReference.class&amp;contextDirection=outgoing 

Note, DocumentReference.class is updated DocumentReference.category in FHIR R4.
	
### Add section on how we selected of DocumentReference (KEEP OR REMOVE?)

* Include history of resources considered, plus a new resource
* Discussion on flipping to think about different formats rather than resources. *


### Add section on Clinical Notes vs ClinicalImpression (KEEP OR REMOVE?)

The [FHIR Version {{site.data.fhir.version}}]({{site.data.fhir.path}}) includes the [ClinicalImpression] resource to support the record of a clinical assessment. 

Taken from the Resource Definition:

A record of a clinical assessment performed to determine what problem(s) may affect the patient and before planning the treatments or management strategies that are best to manage a patient's condition. Assessments are often 1:1 with a clinical consultation / encounter, but this varies greatly depending on the clinical workflow. This resource is called "ClinicalImpression" rather than "ClinicalAssessment" to avoid confusion with the recording of assessment tools such as Apgar score

* How do systems expect to store and expose "Progress satisfactory, continue with treatment"?
* Will use 7/18/2018 call to flesh out 


### Future Work

Expand the number of notes systems must support.



#### Usage

example how to use a button to expand an inline example....

{% include examplebutton.html example="foo" %}

{% include link-list.md %}

[ClinicalImpression]: {{site.data.fhir.path}}/clinicalimpression.html
[Argonaut Clinical Types Value Set]: ValueSet-argonaut-clinical-note-type.html