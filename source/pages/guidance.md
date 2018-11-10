---
title: General Guidance and Definitions
layout: default
active: guidance
topofpage: true
---

{:.no_toc}

<!-- TOC  the css styling for this is \pages\assets\css\project.css under 'markdown-toc'-->

<!--* Do not remove this line (it will not be displayed)
 {:toc} -->

### Clinical Notes

Clinical notes are key component to communicate the current status of a patient.  In the context of this implementation guide, the term "clinical notes" refers to the wide variety of documents generated on behalf of a patient in many care activities. They include notes support transitions of care, care planning, quality reporting, billing and even handwritten notes by a providers.  This implementation guide does not define new note types or set content requirements per note type. Instead, this implementation guide focuses on the exchange of clinical notes between systems. 

Specifically, this implementation guide defines the exchange of the following five "Common Clinical Notes" *TODO - link or citation if is defined externally:*

* [Consultation Note (11488-4)]
* [Discharge Summary (18842-5)]
* [History & Physical Note (34117-2)]
* [Procedures Note (28570-0)]
* [Progress Note (11506-3)]

The Argonaut project team developed this initial list after surveying the participants in Argonaut and the US Veterans Administration (VA).  They represent the *minimum* set a system must support to claim conformance to this guide. In addition, systems are encouraged to support other common notes types such as:

*TODO - link these to LOINC too*
* Imaging
* Pathology narrative
* Cardiology Reports 
* Referral Note
* Surgical Operation Note
* Nurse Note

### FHIR Resources to Exchange Clinical Notes

Both [DocumentReference] and [DiagnosticReport] resources have been [profiled] to support the exchange of clinical notes. (See this [section](#justification-for-resources-choices) for a full discussion on the decision to use these resources.)

DocumentReference is the best choice when the narrative is broader then a specific order or report, such as a Progress Note or Discharge Summary Note. The DocumentReference Resource can point to a short 2-3 sentence status of the patient, or reference a complex CDA or Composition document which can include *both* a narrative and a discrete information.

DiagnosticReport is the best choice when a system needs to share discrete information or coded interpretations. The `DiagnosticReport.result` element supports the discrete information and the entire narrative report can be represented by the `DiagnosticReport.presentedForm` element.

Because of the overlapping scope of the underlying resources and variability in system implementation, there is no single best practice for representing a scanned, or narrative-only report. Reports may be represented by either a DocumentReference or a DiagnosticReport as demonstrated by the green area in Figure 1. For example, some systems consider any scanned report, or note, a DocumentReference. Other systems allow users to categorize the scanned report as Lab and store to DiagnosticReport.[^1]

{% include img.html img="DiagnosticReport_DocumentReference_Resource_Overlap.png" caption="Figure 1: DiagnosticReport and DocumentReference Report Overlap" %}

In order to enable consistent access to scanned narrative-only clinical reports the Argonaut Clinical Note Server **SHALL** expose these reports through *both* DiagnosticReport and DocumentReference by representing the same attachment url using the corresponding elements listed below.[^2]  Exposing the content in this manner guarantees the client will receive all the clinical information available for a patient and can easily identify the duplicate data.

* DocumentReference.content.attachment.url
* DiagnosticReport.presentedForm.url

For example, when `DiagnosticReport.presentedForm.url` references a Scan (PDF), that Attachment shall also be accessible through `DocumentReference.content.attachment.url`.(See Figure 2)

{% include img.html img="both-url.jpg" caption="Figure 2: Expose a PDF Report Through Both DiagnosticReport and DocumentReference" %}

Note that not all scanned information stored through DocumentReference will be exposed through DiagnosticReport since DocumentReference stores other non-clinical information (for example, an insurance card).

#### Support Requirements


This guide requires systems to implement the Argonaut Clinical Notes DocumentReference profile and to support a *minimum* of all five Common Clinical Notes listed above.  Systems and may extend there capabilities to other [Document types] as well.

This guide requires systems to implement the Argonaut Clinical Notes DiagnosticReport profile and to support a *minimum* of Cardiology, Radiology, and Pathology report categories. Other categories may be supported as well.  

The [section below](#determining-server-note-and-report-type-support-expand), describes how to discover the Note and Report types a server supports.

Note that this guide focuses on exposing existing information, and not dictating how systems allow their users to capture information.  The contents of the notes or reports, even using standard LOINC concepts, may vary widely by health system or even location. For example, CT Spleen WO contrast (LOINC 30621-7) may include individual sections for history, impressions, conclusions, or just an impressions section. Discharge Summaries may have different facility or regulatory content requirements.

#### Example Query Scenarios

The following section provides some example searches for common scenarios.

A client interested in all Radiology reports can use the following query:

	GET [base]/DiagnosticReport?patient=[id]&category=http://loinc.org|LP29684-5
	
A client interested in all Clinical Notes can use the following query:

	GET [base]/DocumentReference?patient=[id]&class=clinical-note

A client interested in all Discharge Summary Notes can use the following query:

	GET [base]/DocumentReference?patient=[id]&type=http://loinc.org|18842-5

### Determining Server Note and Report Type Support ($expand) 

A client can determine the Note and Report type support of a server by invoking the $expand operation. The [$expand](http://build.fhir.org/valueset-operation-expand.html) operation in the base FHIR R4 specification provides many parameters and features. This guide highlights the use of the operation to discover what specific notes can be requested, and what write formats a server supports. A FHIR server claiming support to this guide **SHOULD** support the $expand operation to discover a value set at a specific element.

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

Note, DocumentReference.class is updated to DocumentReference.category in FHIR R4.
	
### Justification for Resources Choices

The FHIR specification supports sharing narrative-only reports, or notes, in the DocumentReference and DiagnosticReport Resources. When reviewing the minimal number of elements required for each Resource, The [FHIR Version {{site.data.fhir.version}}]({{site.data.fhir.path}}) specification includes several appropriate places to include clinical notes: Composition, ClinicalImpression, DocumentReference, DiagnosticReport, etc.
To differentiate which resource was most appropriate these characteristics were considered:

* Discrete result information
* Note types
* Consistent Client access to scanned, or narrative-only, reports

* The [Composition] resource provides the basic structure to exchange [FHIR Documents].
* The [ClinicalImpression] resource records a clinical assessment. 
* The [DiagnosticReport] resource records findings and interpretation of diagnostic tests.
* The [DocumentReference] resource provides an index to clinical content.

The developers of this guide also considered creating a new ClinicalNotes resource. 

While each of these Resources work well for a specific use case, they don't solve the Client question of 'Which Resource do I query to find all Clinical Notes?'. 

Rather then focus on the specific use cases, the designers considered the variability of Note format. For example systems use text, XHTML, PDF, CDA to capture clinical notes. This variability led the designers to select the [DocumentReference and DiagnosticReport](guidance.html#documentreference-vs-diagnosticreport) as an index mechanisms to the underlying content. 

A Client can query one of these resources and it will return a pointer to specific Resource or the underlying Binary content. Consider the following situation for a Discharge Summary Note. 

* System A supports the Discharge Summary as a Composition resource
* System B supports the Discharge Summary as a CDA Document
* System C supports the Discharge Summary as a PDF Document

The following single query into DocumentReference supports all 3 scenarios:

	GET [base]/DocumentReference?patient=[id]&type=http://loinc.org|18842-5
	
The server returns either a pointer to the Composition or the Binary resource. If other more specific resources are developed for Clinical Notes systems can update their pointers to the new resource.  

#### Clinical Notes vs ClinicalImpression 

The [FHIR Version {{site.data.fhir.version}}]({{site.data.fhir.path}}) includes the [ClinicalImpression] resource to support the record of a clinical assessment. 

Taken from the Resource Definition:

>
A record of a clinical assessment performed to determine what problem(s) may affect the patient and before planning the treatments or management strategies that are best to manage a patient's condition. Assessments are often 1:1 with a clinical consultation / encounter, but this varies greatly depending on the clinical workflow. This resource is called "ClinicalImpression" rather than "ClinicalAssessment" to avoid confusion with the recording of assessment tools such as Apgar score
>

In existing EHRs, the clinical impression is often contained with in a broader note. The Argonauts didn't find the boundary between a clinical note and ClinicalImpression clear enough to include in the initial design to share Clinical Notes. 

### Future Work

Expand the number of notes systems must support.

---

[^1]: Storing scanned reports as a DiagnosticReport, with appropriate categorization, enables clients to access the scanned reports along with DiagnosticReports containing discrete information. For example, a client can request all DiagnosticReport.category="LAB" and receive reports with discrete information and any scanned reports. However, not all systems store and categorize Lab reports with DiagnosticReport.

[^2]: The developers of this guide considered requiring Clients query *both* DocumentReference and DiagnosticReport to get all the information for a patient. However, the requirement to query two places is potentially dangerous if a client doesn't understand or follow this requirement and queries only one resource type thereby potentially missing important information from the other type.

{% include link-list.md %}