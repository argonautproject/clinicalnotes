

`GET [base]/DiagnosticReport?patient=[id]&category=RAD`

**Example:** GET [base]/DiagnosticReport?patient=f201&category=RAD

*Support:* Mandatory to support search by patient and category code = 'RAD'.

*Implementation Notes:* Search based on diagnostic report category code = 'RAD'. This fetches a bundle of all RAD related DiagnosticReport resources for the specified patient  [(how to search by reference)] and [(how to search by token)].


-----------

`GET [base]/DiagnosticReport?patient=[id]&code=[LOINC{,LOINC2,LOINC3,...}]`

**Example:**
  -  Search for all radiology reports (LOINC = 18782-3  *X-ray*) for a patient
  - GET [base]/DiagnosticReport?patient=1032702&code=18782-3


*Support:* Mandatory support search by a Radiology order code. SHOULD support search by multiple order codes.

*Implementation Notes:* Search based on DiagnosticReport code(s). This fetches a bundle of all DiagnosticReport resources for a specific diagnostic order code(s) for the specified patient  [(how to search by reference)] and [(how to search by token)].


-----------

`GET [base]/DiagnosticReport?patient=[id]&category=RAD&date=[date]{&date=[date]}`

**Example:** Find all the RAD reports issued after 2010-01-14

- GET [base]/DiagnosticReport?patient=f201&category=RAD&date=ge2010-01-14

*Support:*  Mandatory support search by category code = 'RAD' and date or period.

*Implementation Notes:*  Search based on Radiology (RAD) category code and date. This fetches a bundle of all DiagnosticReport resources with category 'RAD' for the specified patient for a specified time period   [(how to search by reference)], [(how to search by token)] amd [(how to search by date)].



  [(how to search by reference)]: {{site.data.fhir.path}}/search.html#reference
  [(how to search by token)]: {{site.data.fhir.path}}/search.html#token
  [Composite Search Parameters]: {{site.data.fhir.path}}/search.html#combining
  [(how to search by date)]: {{site.data.fhir.path}}/search.html#date
