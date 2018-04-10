#### Complete Summary of the Mandatory Requirements

1.  One status in `MedicationAdministration.status` which has a [required]({{ site.data.fhir.path }}/terminologies.html#required) binding to:
-   [MedicationAdministrationStatus] value set.
1.  One medication via `MedicationAdministration.medicationCodeableConcept` or `MedicationAdministration.medicationReference`   
-  `MedicationAdministration.medicationCodeableConcept` has an [extensible]({{ site.data.fhir.path }}/terminologies.html#extensible) binding to [Medication Clinical Drug (RxNorm)] value set.
1.  One patient reference in `MedicationAdministration.subject`
1.  One date or period in `MedicationAdministration.effectiveDateTime` or `MedicationAdministrationTakenment.effectivePeriod`

#### Summary of the Must Support Requirements

1.  One or more references to a patient, practitioner, or related-person in `MedicationAdministration.performer.actor`
1.  Dosage Information in `MedicationAdministration.dosage`

  [Medication Clinical Drug (RxNorm)]: {{ site.data.fhir.uscore }}ValueSet-us-core-medication-codes.html

[MedicationAdministrationStatus]: {{ site.data.fhir.path }}/valueset-medication-admin-status.html
