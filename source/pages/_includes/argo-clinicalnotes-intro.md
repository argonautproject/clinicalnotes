

The [MedicationAdministration] resource is used to record a patient consuming or otherwise being administered a medication.  For more information about the context and usage, refer to the resource page in the FHIR specification.  This profile sets minimum expectations for recording, searching and fetching a patient's MedicationAdministration resource. It identifies which core elements, extensions, vocabularies and value sets **SHALL** be present in the resource when using this profile.

**Example Usage Scenarios:**

The following are example usage scenarios:

- Query for medications that have been administered to a particular patient
- Query for all medications administered during an encounter
- Query for all medications which were not administered during an encounter

##### Mandatory Data Elements and Terminology

The following data-elements are mandatory (i.e data MUST be present). These are presented below in a simple human-readable explanation.  Profile specific guidance and examples are provided as well.  The [**Formal Profile Definition**](#profile) below provides the  formal summary, definitions, and terminology requirements.

**Each MedicationAdministration must have:**

1.  a status
1.  a medication
1.  a patient
1.  a date or date range

In addition, a system [*Must Support*](http://hl7.org/FHIR/us/daf/2016Sep/daf-core.html#mustsupport).

1. who administered the medication
2. dosage information

**Profile specific implementation guidance:**

*  None

##### Examples

- [Med Admin 1](MedicationAdministration-medadmin-1.html)


  [Medication Clinical Drug (RxNorm)]: valueset-daf-medication-codes.html
  [MedicationOrderStatus]: http://hl7.org/fhir/us/daf/valueset-medication-order-status.html
[MedicationAdministrationStatus]: http://hl7.org/fhir/us/daf/valueset-medication-Administration-status.html
[MedicationStatement]:{{ site.data.fhir.path }}/medicationstatement.html
[MedicationAdministration]:{{ site.data.fhir.path }}/medicationadministration.html
 [MedicationOrder]: {{ site.data.fhir.path }}/medicationorder.html
 [Medication]:{{ site.data.fhir.path }}/medication.html
 [Conformance]: daf-core-medicationAdministration-conformance.html
 [boundaries section]: {{ site.data.fhir.path }}/medicationadministration.html#bnr
