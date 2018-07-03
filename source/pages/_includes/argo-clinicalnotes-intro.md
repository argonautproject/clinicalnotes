

This profile sets minimum expectations for searching and fetching Clinical Notes using the [DocumentReference] resource. It is built upon the [US Core DocumentReference] profile. It identifies the mandatory core elements, extensions, vocabularies and value sets which **SHALL** be present in the DocumentReference resource when using this profile.

**Example Usage Scenarios:**

The following are example usage scenarios for the Argonaut Clinical Notes profile:

-   Query for a specific Clinical Note type (e.g. Discharge Summary)
-   Query for all Clinical Notes belonging to a Patient
-   Write a new Notes to a Patient's Chart

##### Mandatory Data Elements and Terminology

The following data-elements are mandatory (i.e data MUST be present). These are presented below in a simple human-readable explanation. Profile specific guidance and examples are provided as well. The [**Formal Profile Definition**](#profile) below provides the  formal summary, definitions, and  terminology requirements.  

**Each Clinical Note DocumentReference must have:**

1.  a category of the DocumentReference, referred to as class
1.  an author

Additional mandatory elements inherited from [US Core DocumentReference]

1.  a patient
1.  a code describing the type of document
1.  a status
1.  an https address where the document can be retrieved

In addition it should have (if available):

1.  the organization responsible for the document, referred to as custodian. 

Additional Inherited from [US Core DocumentReference]

1.  an identifier
1.  a document creation date
1.  a code identifying the specific details about the format of the document — over and above the content's MIME type
1.  the patient encounter date that is being referenced


**Profile specific implementation guidance:**

The `DocumentReference.type` binding must support at a minimum these [5 concepts](ValueSet-dr-type.html) and may extend to the full  [HITSP C80 Table 2-144 Document Class Value Set Definition](http://build.fhir.org/valueset-c80-doc-typecodes.html)


##### Examples

- TBD

[US Core DocumentReference]: http://build.fhir.org/ig/HL7/US-Core/StructureDefinition-us-core-documentreference.html
[MedicationStatement]:{{ site.data.fhir.path }}/medicationstatement.html
[DocumentReference]:{{ site.data.fhir.path }}/DocumentReference.html
 [Conformance]: daf-core-DocumentReference-conformance.html
 [boundaries section]: {{ site.data.fhir.path }}/DocumentReference.html#bnr
 [IHE MHD]: http://ihe.net/uploadedFiles/Documents/ITI/IHE_ITI_Suppl_MHD.pdf

 