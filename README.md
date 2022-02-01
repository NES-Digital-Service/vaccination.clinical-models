# vaccination.clinical-models
openEHR content models for vaccinations


# Current composition description

We have used the [flat composition](./exported/vacs-composition-example-flat.json) fields below to document their meaning.

| Field | Details |
| --- | --- |
| ctx/language | The language of the composition |
| ctx/territory\|code | ISO 3188 territory code |
| ctx/composer_name | The author of the composition |
| ctx/composer_id | The author of the compositions email address |
| ctx/id_namespace | Composer namespace NHSScotland is consistent with ReSPECT |
| ctx/id_scheme | Composer scheme NHSScotland is consistent with ReSPECT |
| ctx/health_care_facility\|name | The venue of the composition author |
| ctx/health_care_facility\|id | The venue code of the composition author |
| nes_vaccination_record/category\|code | openEHR event code |
| nes_vaccination_record/category\|terminology | openEHR event terminology |
| nes_vaccination_record/category\|value | openEHR event value |
| nes_vaccination_record/vaccination_management/_other_participation:0\|id | Identifier of the person that administered the vaccination |
| nes_vaccination_record/vaccination_management/_other_participation:0\|id_namespace | Namespace of the identifier, for CHI "chi-number" would be consistent with ReSPECT |
| nes_vaccination_record/vaccination_management/_other_participation:0\|id_scheme | Scheme fo the identifier, for CHI number "http://fhir.nhs.scot.uk/Id/national/chi-number" would be consistent with ReSPECT |
| nes_vaccination_record/vaccination_management/_other_participation:0\|name | Name of the person who administered the vaccine. *Question*: NCDS store these separately. If we need to map back to FHIR that could be interesting/challenging depending on data quality, perhaps better to store separately? |
| nes_vaccination_record/vaccination_management/_other_participation:0\|function | Role of the person that administered the vaccination |
| nes_vaccination_record/event_context/xds_metadata/document_type\|code | SNOMED code for immunization |
| nes_vaccination_record/event_context/xds_metadata/document_type\|value | Description of event |
| nes_vaccination_record/vaccination_management/ism_transition/current_state\|code | Current ISM status code - https://specifications.openehr.org/releases/RM/Release-1.1.0/ehr.html#_the_standard_instruction_state_machine_ism |
| nes_vaccination_record/vaccination_management/ism_transition/current_state\|value | Current ISM status description - https://specifications.openehr.org/releases/RM/Release-1.1.0/ehr.html#_the_standard_instruction_state_machine_ism |
| nes_vaccination_record/vaccination_management/ism_transition/current_state\|terminology | Terminology for current state |
| nes_vaccination_record/vaccination_management/ism_transition/careflow_step\|code | Code for the care flow step either 0006 (administered) or at0018 (not given) |
| nes_vaccination_record/vaccination_management/ism_transition/careflow_step\|value | Either `Vaccine dose administered` or `Vaccine dose not given` (must match code) |
| nes_vaccination_record/vaccination_management/ism_transition/careflow_step\|terminology | Terminology fo the care flow step |
| nes_vaccination_record/vaccination_management/vaccine_name | Vaccine name. *Note*: We'll change the template to coded text so we can store the description and the code i.e. SNOMED code -  39114911000001105? |
| nes_vaccination_record/vaccination_management/medication/manufacturer | Vaccine manufacturer |
| nes_vaccination_record/vaccination_management/medication/batch_id | Vaccine batch code |
| nes_vaccination_record/vaccination_management/medication/expiry | Vaccine expiration data. *Note*: the field will remain in the template (prohibited), so we'll not use it |
| nes_vaccination_record/vaccination_management/medication_supply_amount/amount_description | Amount given. *Note*: the field will remain in the template (prohibited), so we'll not use it |
| nes_vaccination_record/vaccination_management/reason:0 | FHIR code - https://www.hl7.org/fhir/valueset-immunization-reason.html or https://www.hl7.org/fhir/valueset-immunization-status-reason.html |
| nes_vaccination_record/vaccination_management/administration_details/route | FHIR code - https://www.hl7.org/fhir/valueset-immunization-route.html (NCDS have a longer list) |
| nes_vaccination_record/vaccination_management/administration_details/body_site | FHIR code - https://www.hl7.org/fhir/valueset-immunization-site.html (NCDS have a longer list) |
| nes_vaccination_record/vaccination_management/nes_vaccination_event_details/vaccination_administration_purpose|code | Either booster dose (at0009) or primary (at0008) |
| nes_vaccination_record/vaccination_management/dose_number | Dose number |
| nes_vaccination_record/vaccination_management/note | *Note*: supported but may not be used in NCDS |
| nes_vaccination_record/vaccination_management/appointment_id | The appointment Id |
| nes_vaccination_record/vaccination_management/appointment_id\|issuer | Organisation that issued the appointment |
| nes_vaccination_record/vaccination_management/appointment_id\|assigner | Assigner |
| nes_vaccination_record/vaccination_management/appointment_id\|type | appointment type i.e. AppointmentID |
| nes_vaccination_record/vaccination_management/nes_vaccination_protocol/target_disease:0 | Disease SNOMED code *Note*: going to change this to coded text |
| nes_vaccination_record/vaccination_management/nes_vaccination_protocol/country_of_vaccination\|code | Country code |
| nes_vaccination_record/vaccination_management/nes_vaccination_protocol/country_of_vaccination\|value | Country value |
| nes_vaccination_record/vaccination_management/nes_vaccination_protocol/country_of_vaccination\|terminology | Country ISO |
| nes_vaccination_record/vaccination_management/nes_vaccination_protocol/vaccination_centre | Vaccination centre name |
| nes_vaccination_record/vaccination_management/time | Time vaccination given |

## How to use the composition

We can create a new composition in EHRbase using the below cURL command or our [postman collection](https://nesdigital.atlassian.net/wiki/spaces/NDSENG/pages/130383926). Note that
 - you will have to create an EHR before you can create a composition (see the [postman collection](https://nesdigital.atlassian.net/wiki/spaces/NDSENG/pages/130383926) if your unsure how to do this)
 - the address of the endpoint differs from the canonical JSON endpoint

``` bash
curl --location --request POST 'http://{{host}}/ehrbase/rest/ecis/v1/composition?format=FLAT&ehrId={{ehr-id}}&templateId=NES%20Vaccination%20Record' \
--header 'Content-Type: application/json' \
--header 'Authorization: Basic {{auth-secret}}' \
--data-raw '{{flat-composition-json}}'
```
