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

These notes are defined in the **INSERT VALUE SET HERE**. These 5 note types represent the Common Clinicanl Notes and are the minimum set a system must support to claim conformance to this guide. Systems are encouraged to support many other common notes types, such as:

* Imaging
* Laboratory and pathology narrative
* Referral Note
* Surgical Operation Note
* Nurse Note

The Argonaut project team developed the initial list of 5 through a survey of the participants in Arognaut and the VA.

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


New operation to retrieve value sets at a specific element


### Future Work

Expand the number of notes systems must support.



#### Usage

example how to use a button to expand an inline example....

{% include examplebutton.html example="foo" %}

{% include link-list.md %}

[ClinicalImpression]: {{site.data.fhir.path}}/clinicalimpression.html