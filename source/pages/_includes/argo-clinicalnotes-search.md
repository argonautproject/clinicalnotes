
Typically, DocumentReference resources are used with document indexing systems, such as [IHE XDS]. However, document references may also may be created "on-the-fly" in response to a Document Query request.  In other words there MAY NOT be pre-existing index of references to a patient's documents at the FHIR endpoint. This results in an empty bundle being returned when searching using a normal FHIR Query.  Therefore, the [$docref operation] has been defined to both create and fetch patient DocumentReference Resources.

The following search criteria describe fetching pre-indexed documents and those created "on-the-fly".

**Searching pre-indexed documents:**

----

**`GET [base]/DocumentReference?patient=[id]`**

Example: GET [base]/DocumentReference?patient=2169591


*Support:* Mandatory to support search by patient.

*Implementation Notes:* Search for all documents for a patient. Fetches a bundle of all DocumentReference resources for the specified patient [(how to search by reference)].

------
`GET [base]/DocumentReference?patient=[id]&type=[type]`

**Example:**

-  GET [base]/DocumentReference?patient=1234&type=http://loinc.org | 11488-4


*Support:* Mandatory for server and client to support search by patient and note type....rest all TBD. Mandatory for client to support the _include parameter. Optional for server to support the _include parameter.

*Implementation Notes:*   This query searches for all MedicationAdministration resources for a patient and returns a Bundle of all MedicationAdministration and resources for the specified patient. The server application represents the medication using either an inline code or a contained or external reference to the Medication resource. [(how to search by reference)], and [(how to search by _include)].

-------

  [(how to search by reference)]: {{ site.data.fhir.path }}/search.html#reference
  [(how to search by token)]: {{ site.data.fhir.path }}/search.html#token
  [Composite Search Parameters]: {{ site.data.fhir.path }}/search.html#combining
  [(how to search by date)]: {{ site.data.fhir.path }}/search.html#date
  [(how to search by _include)]: {{ site.data.fhir.path }}/search.html#_include
