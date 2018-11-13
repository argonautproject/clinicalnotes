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

Clinical notes are a key component to communicate the current status of a patient. In the context of this implementation guide, the term "clinical notes" refers to the wide variety of documents generated on behalf of a patient in many care activities. They include notes to support transitions of care, care planning, quality reporting, billing and even handwritten notes by a providers. This implementation guide does not define new note types or set content requirements per note type. Instead, this implementation guide focuses on exposing clinical notes stored in existing systems. 

Specifically, this implementation guide defines the exchange of the following five "Common Clinical Notes".

* [Consultation Note (11488-4)]
* [Discharge Summary (18842-5)]
* [History & Physical Note (34117-2)]
* [Procedures Note (28570-0)]
* [Progress Note (11506-3)]

The Argonaut project team developed this initial list after surveying the participants in Argonaut and the US Veterans Administration (VA). They represent the *minimum* set a system must support to claim conformance to this guide. In addition, systems are encouraged to support other common notes types such as:

*TODO - link these to LOINC too*
* Imaging
* Pathology narrative
* Cardiology Reports 
* Referral Note
* Surgical Operation Note
* Nurse Note

### FHIR Resources to Exchange Clinical Notes

Both [DocumentReference] and [DiagnosticReport] resources have been [profiled] to support the exchange of clinical notes. (See  [Resource Selection](#resource-selection) for a full discussion on the decision to use these resources.)

DocumentReference is the best choice when the narrative is broader then a specific order or report, such as a Progress Note or Discharge Summary Note. The DocumentReference Resource can point to a short 2-3 sentence status of the patient, or reference a complex CDA or Composition document which can include *both* a narrative and a discrete information.

DiagnosticReport is the best choice when a system needs to share discrete information or coded interpretations. The `DiagnosticReport.result` element supports the discrete information and the entire narrative report can be represented by the `DiagnosticReport.presentedForm` element.

There is no single best practice for representing a scanned, or narrative-only report due to the overlapping scope of the underlying resources and variability in system implementation. Reports may be represented by either a DocumentReference or a DiagnosticReport as demonstrated by the green area in Figure 1. For example, some systems consider any scanned report, or note, a DocumentReference. Other systems allow users to categorize the scanned report as Lab and store to DiagnosticReport.[^1]

{% include img-portrait.html img="DiagnosticReport_DocumentReference_Resource_Overlap.png" caption="Figure 1: DiagnosticReport and DocumentReference Report Overlap" %}

In order to enable consistent access to scanned narrative-only clinical reports the Argonaut Clinical Note Server **SHALL** expose these reports through *both* DiagnosticReport and DocumentReference by representing the same attachment url using the corresponding elements listed below.[^2]  Exposing the content in this manner guarantees the client will receive all the clinical information available for a patient and can easily identify the duplicate data.

* DocumentReference.content.attachment.url
* DiagnosticReport.presentedForm.url

For example, when `DiagnosticReport.presentedForm.url` references a Scan (PDF), that Attachment shall also be accessible through `DocumentReference.content.attachment.url`.(See Figure 2)

{% include img.html img="both-url.jpg" caption="Figure 2: Expose a PDF Report Through Both DiagnosticReport and DocumentReference" %}

Note that not all scanned information stored through DocumentReference will be exposed through DiagnosticReport since DocumentReference stores other non-clinical information. For example, DocumentReference can point to an insurance card.

#### Support Requirements

This guide requires systems implement the Argonaut Clinical Notes DocumentReference profile and to support a *minimum* of all five Common Clinical Notes listed above. Systems and may extend there capabilities to other [Document types] as well.

This guide requires systems implement the Argonaut Clinical Notes DiagnosticReport profile and to support a *minimum* of the three report categories:

- Cardiology (LP29684-5)
- Radiology (LP29708-2)
- Pathology (LP7839-6)
 
Other categories may be supported as well.  

A method for discovery of the types of notes and reports that a server supports is described in the [section below](#determining-server-note-and-report-type-support-expand).

Note that this guide focuses on exposing existing information, and not how systems allow their users to capture information. The contents of the notes or reports, even using standard LOINC concepts, may vary widely by health system or even location. For example, CT Spleen WO contrast (LOINC 30621-7) may include individual sections for history, impressions, conclusions, or just an impressions section. Discharge Summaries may have different facility or regulatory content requirements.

#### Search

To retrieve clinical notes and reports, the standard FHIR [search] API is used. In this guide, the Argonaut Clinical Notes Server [CapabilityStatement] and the *Quick Start* sections for the Argonaut Clinical Notes and Diagnostic Report profiles define the required search parameters and describe how they are used.

Common client search scenarios include:

1. A client interested in all Radiology reports can use the following query:
   
	 `GET [base]/DiagnosticReport?patient=[id]&category=http://loinc.org|LP29684-5`

1. A client interested in all Clinical Notes can use the following query:
   
	 `GET [base]/DocumentReference?patient=[id]&class=clinical-note`

1. A client interested in all Discharge Summary Notes can use the following query:
  
	`GET [base]/DocumentReference?patient=[id]&type=http://loinc.org|18842-5`
	
<br/>

### Determining Server Note Type Support Using The Value Set Expansion Operation ($expand)

In addition to inspecting a server CapabilityStatement, a client can determine the note and report types support by a server by invoking the standard FHIR Value Set Expansion ([$expand]) operation defined in the **FHIR R4 specification**. Because servers may support different read and write formats, it also is used to determine the formats (for example, text, pdf) the server supports read and write transactions. A FHIR server claiming support to this guide **SHOULD** support the $expand operation.

#### Discovering Note and Report Types
	
The note and report types for a particular server are discovered by invoking the #expand operation as follows: 

`GET [base]/ValueSet/$expand?context=[context]&contextDirection=[contextDirection]`

where:
- [contextDirection] = `incoming` for write operations and `outgoing` for read operations.
 - [context] = `DiagnosticReport.category` for DiagnosticReport report type discovery, `DocumentReference.type` for DocumentReference note type discovery and `DocumentReference.class` for DocumentReference note category discovery.

 Example 

 **Scenario 1**

A client determines the types of reports they can access through DiagnosticReport. The server responds with 'foo','bar' ,and 'baz' report types:

**Request for DiagnosticReport report type**
 
~~~
GET [base]/ValueSet/$expand?context=DiagnosticReport.codes&contextDirection=incoming
~~~

**Response**

~~~
HTTP/1.1 200 OK
[other headers]
~~~

**Response body**

~~~
{
  "resourceType": "ValueSet",
  "id": "scenario1-server-diagnosticreport-codes",
  "url": "http://acme.org/ValueSet/diagnosticreport-codes",
  "version": "3.0.1",
  "name": "acme-diagnosticreport-codes",
  "title": "Acme DiagnosticReport Codes",
  "status": "draft",
  "date": "2018-11-08T20:29:00+00:00",
  "expansion": {
    "identifier": "urn:uuid:5fc51f5a-4dbc-44f8-8fe5-203d261222f0",
    "timestamp": "2018-11-13T02:55:48.405Z",
    "parameter": [
      {
        "name": "DiagnosticReport.codes",
        "valueString": "DiagnosticReport.codes"
      },
      {
        "name": "contextDirection",
        "valueString": "outgoing"
      }
    ],
    "contains": [
      {
        "system": "http://acme.org",
        "code": "foo",
        "display": "Foo"
      },
      {
        "system": "http://acme.org",
        "code": "bar",
        "display": "Bar"
      },
      {
        "system": "http://acme.org",
        "code": "baz",
        "display": "baz"
      }
    ]
  }
}
~~~

**Scenario 2**

A client determines the types of note or reports they can access through DocumentReference.  The server responds with the five "Common Clinical Notes" types:

**Request for DocumentReference note or report type**

~~~
GET [base]/ValueSet/$expand?context=DocumentReference.type&contextDirection=outgoing
~~~

**Response**

~~~
HTTP/1.1 200 OK
[other headers]
~~~

**Response body**

~~~
{
  "resourceType": "ValueSet",
  "id": "scenario2-server-clinical-note-type",
  "url": "http://fhir.org/guides/argonaut-clinicalnotes/ValueSet/argonaut-clinical-note-type",
  "version": "3.0.1",
  "name": "ArgonautClinicalNotes",
  "title": "Argonaut DocumentReferences Type Value Set",
  "status": "draft",
  "date": "2018-11-08T20:29:00+00:00",
  "expansion": {
    "identifier": "urn:uuid:5fc51f5a-4dbc-44f8-8fe5-203d261222f0",
    "timestamp": "2018-11-13T02:55:48.405Z",
    "parameter": [
      {
        "name": "DocumentReference.type",
        "valueString": "DiagnosticReport.codes"
      },
      {
        "name": "contextDirection",
        "valueString": "outgoing"
      }
    ],
    "contains": [
      {
        "system": "http://loinc.org",
        "code": "18842-5",
        "display": "Discharge Summary"
      },
      {
        "system": "http://loinc.org",
        "code": "11488-4",
        "display": "Consultation Note"
      },
      {
        "system": "http://loinc.org",
        "code": "34117-2",
        "display": "History & Physical Note"
      },
      {
        "system": "http://loinc.org",
        "code": "11506-3",
        "display": "Progress Note"
      },
      {
        "system": "http://loinc.org",
        "code": "28570-0",
        "display": "Procedures Note"
      }
    ]
  }
}
~~~

**Scenario 3**

A client is only interested in retrieving notes by class (Note, DocumentReference.class is updated to DocumentReference.category in FHIR R4).  The server responds with the single category of 'Clinical Notes':

**Request for DocumentReference note categories**

~~~
GET [base]/ValueSet/$expand?context=DocumentReference.class&contextDirection=outgoing
~~~

**Response**

~~~
HTTP/1.1 200 OK
[other headers]
~~~

**Response body**

~~~
{
  "resourceType": "ValueSet",
  "id": "scenario3-server-documentreference-category",
  "url": "http://fhir.org/guides/argonaut-clinicalnotes/ValueSet/documentreference-category",
  "version": "3.0.1",
  "name": "ArgonautDocumentReferenceCategoryCodes",
  "title": "Argonaut DocumentReference Category Codes",
  "status": "draft",
  "date": "2018-11-08T20:29:00+00:00",
  "expansion": {
    "identifier": "urn:uuid:5fc51f5a-4dbc-44f8-8fe5-203d261222f0",
    "timestamp": "2018-11-13T02:55:48.405Z",
    "parameter": [{
        "name": "context",
        "valueString": "DocumentReference.class"
      },
      {
        "name": "contextDirection",
        "valueString": "outgoing"
      }
    ],
    "contains": [{
        "system": "http://fhir.org/guides/argonaut-clinicalnotes/CodeSystem/documentreference-category",
        "code": "clinical-note",
        "display": "Clinical Note"
      }
    ]
  }
}
~~~

**Scenario 4**

A client determines the category of reports they can access through DiagnosticReport. The server responds with Radiology, Pathology ,and Cardiology report categories:

**Request for DiagnosticReport report type**
 
~~~
GET [base]/ValueSet/$expand?context=DiagnosticReport.category&contextDirection=incoming
~~~

**Response**

~~~
HTTP/1.1 200 OK
[other headers]
~~~

**Response body**

~~~
{
  "resourceType": "ValueSet",
  "id": "scenario4-server-diagnosticreport-category",
  "url": "http://fhir.org/guides/argonaut-clinicalnotes/ValueSet/diagnosticreport-category",
  "version": "3.0.1",
  "name": "ArgonautDiagnosticReportCategoryCodes",
  "title": "Argonaut DiagnosticReport Category Value Set",
  "status": "draft",
  "date": "2018-11-08T20:29:00+00:00",
  "expansion": {
    "identifier": "urn:uuid:5fc51f5a-4dbc-44f8-8fe5-203d261222f0",
    "timestamp": "2018-11-13T02:55:48.405Z",
    "parameter": [
      {
        "name": "context",
        "valueString": "DiagnosticReport.category"
      },
      {
        "name": "contextDirection",
        "valueString": "outgoing"
      }
    ],
    "contains": [
      {
        "system": "http://loinc.org",
        "code": "LP29684-5",
        "display": "Radiology"
      },
      {
        "system": "http://loinc.org",
        "code": "LP29708-2",
        "display": "Cardiology"
      },
      {
        "system": "http://loinc.org",
        "code": "LP7839-6",
        "display": "Pathology"
      }
    ]
  }
}
~~~
<br/>

#### Discovering Server Read and Write Formats

The read and write formats for a particular server are discovered by invoking the #expand operation as follows:

`GET [base]/ValueSet/$expand?context=[context]&contextDirection=[contextDirection]`

where:
- [contextDirection] = `incoming` for write operations and `outgoing` for read operations.
 - [context] = `DocumentReference.content.attachment.contentType` for DocumentReference note content type discovery and `DiagnosticReport.presentedForm.contentType` for DiagnosticReport report content type discovery.

Example 

**Scenario 1**

System A accepts contentType `text/plain` in a create transaction and returns `text/html` in a read transaction.

**Request for Write contentType**

~~~
GET [base]/ValueSet/$expand?context=DocumentReference.content.attachment.contentType&contextDirection=incoming
~~~

**Response**

~~~
HTTP/1.1 200 OK
[other headers]
~~~

**Response body**

~~~
{
  "resourceType": "ValueSet",
  "id": "scenario1-server-write-contenttype",
  "url": "http://acme.org/fhir/ValueSet/in-mimetypes-1",
  "version": "3.0.1",
  "name": "ArgonautServerWriteMimeTypes",
  "title": "Argonaut Server Write Mime Types Value Set",
  "status": "draft",
  "date": "2018-11-08T20:29:00+00:00",
  "expansion": {
    "identifier": "urn:uuid:5fc51f5a-4dbc-44f8-8fe5-203d261222f0",
    "timestamp": "2018-11-13T02:55:48.405Z",
    "parameter": [
      {
        "name": "context",
        "valueString": "DocumentReference.content.attachment.contentType"
      },
      {
        "name": "contextDirection",
        "valueString": "incoming"
      }
    ],
    "contains": [
      {
        "system": "urn:ietf:bcp:13",
        "code": "text/plain"
      }
    ]
  }
}
~~~

**Request for Read contentType**

~~~
GET [base]/ValueSet/$expand?context=DocumentReference.content.attachment.contentType&contextDirection=outgoing
~~~

**Response**

~~~
HTTP/1.1 200 OK
[other headers]
~~~

**Response body**

~~~
{
  "resourceType": "ValueSet",
  "id": "scenario1-server-read-contenttype",
  "url": "http://acme.org/fhir/ValueSet/out-mimetypes-1",
  "version": "3.0.1",
  "name": "ArgonautServerReadMimeTypes",
  "title": "Argonaut Server Read Mime Types Value Set",
  "status": "draft",
  "date": "2018-11-08T20:29:00+00:00",
  "expansion": {
    "identifier": "urn:uuid:5fc51f5a-4dbc-44f8-8fe5-203d261222f0",
    "timestamp": "2018-11-13T02:55:48.405Z",
    "parameter": [
      {
        "name": "context",
        "valueString": "DocumentReference.content.attachment.contentType"
      },
      {
        "name": "contextDirection",
        "valueString": "outgoing"
      }
    ],
    "contains": [
      {
        "system": "urn:ietf:bcp:13",
        "code": "text/html"
      }
    ]
  }
}
~~~

**Scenario 2**

System A accepts contentType `text/xhtml` in a create transaction and returns `application/pdf` in a read transaction.

**Request for Write contentType`**

~~~
GET [base]/ValueSet/$expand?context=DocumentReference.content.attachment.contentType&amp;contextDirection=incoming
~~~

**Response**

~~~
HTTP/1.1 200 OK
[other headers]
~~~

**Response body**

~~~
{
  "resourceType": "ValueSet",
  "id": "scenario2-server-write-contenttype",
  "url": "http://acme.org/fhir/ValueSet/in-mimetypes-2",
  "version": "3.0.1",
  "name": "ArgonautServerWriteMimeTypes",
  "title": "Argonaut Server Write Mime Types Value Set",
  "status": "draft",
  "date": "2018-11-08T20:29:00+00:00",
  "expansion": {
    "identifier": "urn:uuid:5fc51f5a-4dbc-44f8-8fe5-203d261222f0",
    "timestamp": "2018-11-13T02:55:48.405Z",
    "parameter": [
      {
        "name": "context",
        "valueString": "DocumentReference.content.attachment.contentType"
      },
      {
        "name": "contextDirection",
        "valueString": "incoming"
      }
    ],
    "contains": [
      {
        "system": "urn:ietf:bcp:13",
        "code": "text/xhtml"
      }
    ]
  }
}
~~~

**Request for Write contentType**

~~~
GET [base]/ValueSet/$expand?context=DocumentReference.content.attachment.contentType&amp;contextDirection=outgoing
~~~

**Response**

~~~
HTTP/1.1 200 OK
[other headers]
~~~

**Response body**

~~~
{
  "resourceType": "ValueSet",
  "id": "scenario2-server-read-contenttype",
  "url": "http://acme.org/fhir/ValueSet/out-mimetypes-2",
  "version": "3.0.1",
  "name": "ArgonautServerReadMimeTypes",
  "title": "Argonaut Server Read Mime Types Value Set",
  "status": "draft",
  "date": "2018-11-08T20:29:00+00:00",
  "expansion": {
    "identifier": "urn:uuid:5fc51f5a-4dbc-44f8-8fe5-203d261222f0",
    "timestamp": "2018-11-13T02:55:48.405Z",
    "parameter": [
      {
        "name": "context",
        "valueString": "DocumentReference.content.attachment.contentType"
      },
      {
        "name": "contextDirection",
        "valueString": "outgoing"
      }
    ],
    "contains": [
      {
        "system": "urn:ietf:bcp:13",
        "code": "application/pdf"
      }
    ]
  }
}
~~~

### Resource Selection

When reviewing the minimal number of elements required for each Resource, the [FHIR Version {{site.data.fhir.version}}]({{site.data.fhir.path}}) specification includes several appropriate places to include clinical notes such as Composition, ClinicalImpression, DocumentReference, DiagnosticReport, etc.  The developers of this guide also considered creating a new ClinicalNotes resource. To differentiate which resource was most appropriate these characteristics were considered:

* Discrete result information
* Note types
* Consistent Client access to scanned, or narrative-only, reports

While several resources work well for a specific use case, they don't solve the question "find all Clinical Notes for a patient?" expecially when considering the variability of Note formats. For example systems use text, XHTML, PDF, CDA to capture clinical notes. This variability led the designers to select the [DocumentReference and DiagnosticReport](guidance.html#documentreference-vs-diagnosticreport) as an index mechanisms to the underlying content. In other words, a client can query one of these resources and it will return a pointer to specific resource or the underlying binary content. 

For example, consider the following situation for a Discharge Summary Note:

* System A supports the Discharge Summary as a Composition resource
* System B supports the Discharge Summary as a CDA Document
* System C supports the Discharge Summary as a PDF Document

The following single query into DocumentReference supports all 3 scenarios:

	GET [base]/DocumentReference?patient=[id]&type=http://loinc.org|18842-5
	
The server returns either a pointer to the Composition or the Binary resource. If other more specific resources are developed for Clinical Notes systems can update their pointers to the new resource.  

#### Clinical Notes vs ClinicalImpression 

[ClinicalImpression] resource supports the record of a clinical assessment. 

>
A record of a clinical assessment performed to determine what problem(s) may affect the patient and before planning the treatments or management strategies that are best to manage a patient's condition. Assessments are often 1:1 with a clinical consultation / encounter, but this varies greatly depending on the clinical workflow. This resource is called "ClinicalImpression" rather than "ClinicalAssessment" to avoid confusion with the recording of assessment tools such as Apgar score
>

However, in existing EHRs, the clinical impression is often contained with in a broader note construct and the Argonauts didn't find the boundary between a clinical note and ClinicalImpression clear enough to leverage the resources to share clinical notes.

### Future Work

Expand the number of notes systems must support.

---
footnotes:

[^1]: Storing scanned reports as a DiagnosticReport, with appropriate categorization, enables clients to access the scanned reports along with DiagnosticReports containing discrete information. For example, a client can request all DiagnosticReport.category="LAB" and receive reports with discrete information and any scanned reports. However, not all systems store and categorize Lab reports with DiagnosticReport.

[^2]: The developers of this guide considered requiring Clients query *both* DocumentReference and DiagnosticReport to get all the information for a patient. However, the requirement to query two places is potentially dangerous if a client doesn't understand or follow this requirement and queries only one resource type thereby potentially missing important information from the other type.

{% include link-list.md %}
