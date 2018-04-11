

The [DocumentReference] resource is used to index documents in a healthcare ssytem.  For more information about the context and usage, refer to the resource page in the FHIR specification.  This profile sets minimum expectations for recording, searching and fetching a patient's DocumentReference resource. It identifies which core elements, extensions, vocabularies and value sets **SHALL** be present in the resource when using this profile.

**Example Usage Scenarios:**

The following are example usage scenarios:

- Query for Clinical Notes for a set time period
- Query for Clinical None for an Encounter
- Query for specific Documents


##### Mandatory Data Elements and Terminology

The following data-elements are mandatory (i.e data MUST be present). These are presented below in a simple human-readable explanation.  Profile specific guidance and examples are provided as well.  The [**Formal Profile Definition**](#profile) below provides the  formal summary, definitions, and terminology requirements.

**Each DocumentReference must have:**

1.  TBD

In addition, a system [*Must Support*](http://hl7.org/fhir/us/core/guidance.html#must-support).

1. TBD

**Profile specific implementation guidance:**

*  None

##### Examples

- TBD


  [Medication Clinical Drug (RxNorm)]: valueset-daf-medication-codes.html
  [MedicationOrderStatus]: http://hl7.org/fhir/us/daf/valueset-medication-order-status.html
[DocumentReferenceStatus]: http://hl7.org/fhir/us/daf/valueset-medication-Administration-status.html
[MedicationStatement]:{{ site.data.fhir.path }}/medicationstatement.html
[DocumentReference]:{{ site.data.fhir.path }}/DocumentReference.html
 [MedicationOrder]: {{ site.data.fhir.path }}/medicationorder.html
 [Medication]:{{ site.data.fhir.path }}/medication.html
 [Conformance]: daf-core-DocumentReference-conformance.html
 [boundaries section]: {{ site.data.fhir.path }}/DocumentReference.html#bnr

 