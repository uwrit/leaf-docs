# Dataset Templates Reference
As mentioned in the [Introduction](../datasets), `Datasets` can be defined as either:

1. SQL statements which output **dynamic** columns unique to your data. Or,
2. SQL statements which output **templated** columns representating of <a href="https://www.hl7.org/fhir/resourcelist.html" target="_blank">FHIR resources</a>. 

This page describes **templated** `Dataset` shapes.

!!! info 
    SQL Columns of type `numeric` listed below can be `INT`, `FLOAT`, `MONEY`, `NUMERIC`, `REAL`, `SMALLINT`, `SMALLMONEY`, or `DECIMAL`.

## Observation
*Dataset Shape* **1** - A representation of the FHIR <a href="https://www.hl7.org/fhir/observation.html" target="_blank">Observation</a> Resource

!!! example "FHIR documention suggests using this resource for"
    - Vital signs such as body weight, blood pressure, and temperature
    - Laboratory Data like blood glucose, or an estimated GFR
    - Imaging results like bone density or fetal measurements
    - Clinical Findings such as abdominal tenderness
    - Device measurements such as EKG data or Pulse Oximetry data
    - Clinical assessment tools such as APGAR or a Glasgow Coma Score
    - Personal characteristics: such as eye-color
    - Social history like tobacco use, family support, or cognitive status
    - Core characteristics like pregnancy status, or a death assertion

SQL Columns (<a href="https://github.com/uwrit/leaf/blob/master/src/server/Model/Cohort/Observation.cs" target="_blank">source</a>):

| Name               | Type      | Required | Is PHI |
| -----------------  | --------- | -------- | ------ |
| personId           | nvarchar  | X        | X      |
| encounterId        | nvarchar  | X        | X      |
| category           | nvarchar  | X        |        |
| code               | nvarchar  | X        |        |
| effectiveDate      | datetime  | X        | X      |
| referenceRangeLow  | datetime  |          |        |
| referenceRangeHigh | nvarchar  |          |        |
| specimenType       | nvarchar  |          |        |
| valueString        | nvarchar  | X        |        |
| valueQuantity      | numeric   |          |        |
| valueUnit          | nvarchar  |          |        |

## Encounter
*Dataset Shape* **2** - A representation of the FHIR <a href="https://www.hl7.org/fhir/encounter.html" target="_blank">Encounter</a> Resource

!!! example "FHIR documention describes as"
    A patient encounter is characterized by the setting in which it takes place. Amongst them are ambulatory, emergency, home health, inpatient and virtual encounters. An Encounter encompasses the lifecycle from pre-admission, the actual encounter (for ambulatory encounters), and admission, stay and discharge (for inpatient encounters). During the encounter the patient may move from practitioner to practitioner and location to location. 


SQL Columns (<a href="https://github.com/uwrit/leaf/blob/master/src/server/Model/Cohort/Encounter.cs" target="_blank">source</a>):

| Name                 | Type      | Required | Is PHI |
| -----------------    | --------- | -------- | ------ |
| personId             | nvarchar  | X        | X      |
| encounterId          | nvarchar  | X        | X      |
| admitDate            | datetime  |          | X      |
| class                | nvarchar  | X        |        |
| dischargeDate        | datetime  | X        | X      |
| dischargeDisposition | nvarchar  |          |        |
| location             | nvarchar  | X        |        |
| status               | nvarchar  |          |        |

## Basic Demographics
*Dataset Shape* **3** - A representation of a combination of the <a href="https://www.hl7.org/fhir/person.html" target="_blank">Person</a> and <a href="https://www.hl7.org/fhir/patient.html" target="_blank">Patient</a> FHIR resources.

See the [Basic Demographics](#basic-demographics) section above for additional info.

SQL Columns (<a href="https://github.com/uwrit/leaf/blob/master/src/server/Model/Cohort/PatientDemographic.cs" target="_blank">source</a>):

| Name              | Type      | Required | Is PHI |
| ----------------- | --------- | -------- | ------ |
| personId          | nvarchar  | X        | X      |
| addressPostalCode | nvarchar  | X        |        |
| addressState      | nvarchar  | X        |        |
| birthDate         | datetime  | X        | X      |
| deceasedDateTime  | datetime  | X        | X      |
| ethnicity         | nvarchar  | X        |        |
| gender            | nvarchar  | X        |        |
| deceasedBoolean   | bit       | X        |        |
| hispanicBoolean   | bit       | X        |        |
| marriedBoolean    | bit       | X        |        |
| language          | nvarchar  | X        |        |
| maritalStatus     | nvarchar  | X        |        |
| mrn               | nvarchar  | X        | X      |
| name              | nvarchar  | X        | X      |
| race              | nvarchar  | X        |        |
| religion          | nvarchar  | X        |        |

## Condition
*Dataset Shape* **4** - A representation of the FHIR <a href="https://www.hl7.org/fhir/condition.html" target="_blank">Condition</a> Resource

!!! example "FHIR documention describes as"
    This resource is used to record detailed information about a **condition, problem, diagnosis**, or other event, situation, issue, or clinical concept that has risen to a level of concern. The condition could be a point in time diagnosis in context of an encounter, it could be an item on the practitioner’s Problem List, or it could be a concern that doesn’t exist on the practitioner’s Problem List. Often times, a condition is about a clinician's assessment and assertion of a particular aspect of a patient's state of health. It can be used to record information about a disease/illness identified from application of clinical reasoning over the pathologic and pathophysiologic findings (diagnosis), or identification of health issues/situations that a practitioner considers harmful, potentially harmful and may be investigated and managed (problem), or other health issue/situation that may require ongoing monitoring and/or management (health issue/concern)


SQL Columns (<a href="https://github.com/uwrit/leaf/blob/master/src/server/Model/Cohort/Condition.cs" target="_blank">source</a>):

| Name                 | Type      | Required | Is PHI |
| -----------------    | --------- | -------- | ------ |
| personId             | nvarchar  | X        | X      |
| encounterId          | nvarchar  | X        | X      |
| abatementDateTime    | datetime  |          | X      |
| category             | nvarchar  | X        |        |
| code                 | nvarchar  | X        |        |
| coding               | nvarchar  | X        |        |
| onsetDateTime        | datetime  | X        | X      |
| recordedDate         | nvarchar  |          | X      |
| text                 | nvarchar  | X        |        |

## Procedure
*Dataset Shape* **5** - A representation of the FHIR <a href="https://www.hl7.org/fhir/procedure.html" target="_blank">Procedure</a> Resource

!!! example "FHIR documention describes as"
    This resource is used to record the details of current and historical procedures performed on or for a patient. A procedure is an activity that is performed on, with, or for a patient as part of the provision of care. Examples include surgical procedures, diagnostic procedures, endoscopic procedures, biopsies, counseling, physiotherapy, personal support services, adult day care services, non-emergency transportation, home modification, exercise, etc. Procedures may be performed by a healthcare professional, a service provider, a friend or relative or in some cases by the patient themselves

SQL Columns (<a href="https://github.com/uwrit/leaf/blob/master/src/server/Model/Cohort/Procedure.cs" target="_blank">source</a>):

| Name                 | Type      | Required | Is PHI |
| -----------------    | --------- | -------- | ------ |
| personId             | nvarchar  | X        | X      |
| encounterId          | nvarchar  | X        | X      |
| category             | nvarchar  | X        |        |
| code                 | nvarchar  | X        |        |
| coding               | nvarchar  | X        |        |
| performedDateTime    | datetime  | X        | X      |
| text                 | nvarchar  | X        |        |

## Immunization
*Dataset Shape* **6** - A representation of the FHIR <a href="https://www.hl7.org/fhir/immunization.html" target="_blank">Immunization</a> Resource

!!! example "FHIR documention describes as"
    The Immunization resource is intended to cover the recording of current and historical administration of vaccines to patients across all healthcare disciplines in all care settings and all regions. This includes immunization of both humans and animals but does not include the administration of non-vaccine agents, even those that may have or claim to have immunological effects. While the terms "immunization" and "vaccination" are not clinically identical, for the purposes of the FHIR resources, the terms are used synonymously

SQL Columns (<a href="https://github.com/uwrit/leaf/blob/master/src/server/Model/Cohort/Immunization.cs" target="_blank">source</a>):

| Name                 | Type      | Required | Is PHI |
| -----------------    | --------- | -------- | ------ |
| personId             | nvarchar  | X        | X      |
| encounterId          | nvarchar  | X        | X      |
| coding               | nvarchar  | X        |        |
| doseQuantity         | numeric   |          |        |
| doseUnit             | nvarchar  |          |        |
| occurenceDateTime    | datetime  | X        | X      |
| route                | nvarchar  |          |        |
| text                 | nvarchar  | X        |        |
| vaccineCode          | nvarchar  | X        |        |

## Allergy
*Dataset Shape* **7** - A representation of the FHIR <a href="https://www.hl7.org/fhir/allergyintolerance.html" target="_blank">AllergyIntolerance </a> Resource

!!! example "FHIR documention describes as"
    A record of a clinical assessment of an allergy or intolerance; a propensity, or a potential risk to an individual, to have an adverse reaction on future exposure to the specified substance, or class of substance

SQL Columns (<a href="https://github.com/uwrit/leaf/blob/master/src/server/Model/Cohort/Allergy.cs" target="_blank">source</a>):

| Name                 | Type      | Required | Is PHI |
| -----------------    | --------- | -------- | ------ |
| personId             | nvarchar  | X        | X      |
| encounterId          | nvarchar  | X        | X      |
| category             | nvarchar  | X        |        |
| code                 | nvarchar  | X        |        |
| coding               | nvarchar  | X        |        |
| onsetDateTime        | datetime  | X        | X      |
| recordedDate         | datetime  |          | X      |
| text                 | nvarchar  | X        |        |

## MedicationRequest
*Dataset Shape* **8** - A representation of the FHIR <a href="https://www.hl7.org/fhir/medicationrequest.html" target="_blank">MedicationRequest</a> Resource

!!! example "FHIR documention describes as"
    This resource covers **all type of orders for medications for a patient**. This includes inpatient medication orders as well as community orders (whether filled by the prescriber or by a pharmacy). It also includes orders for over-the-counter medications (e.g. Aspirin), total parenteral nutrition and diet/ vitamin supplements. It may be used to support the order of medication-related devices. It is not intended for use in prescribing particular diets, or for ordering non-medication-related items (eyeglasses, supplies, etc.). In addition, the MedicationRequest may be used to report orders/request from external systems that have been reported for informational purposes and are not authoritative and are not expected to be acted upon (e.g. dispensed or administered)

SQL Columns (<a href="https://github.com/uwrit/leaf/blob/master/src/server/Model/Cohort/MedicationRequest.cs" target="_blank">source</a>):

| Name                 | Type      | Required | Is PHI |
| -----------------    | --------- | -------- | ------ |
| personId             | nvarchar  | X        | X      |
| encounterId          | nvarchar  | X        | X      |
| amount               | numeric   |          |        |
| authoredOn           | datetime  | X        | X      |
| category             | nvarchar  | X        |        |
| code                 | nvarchar  | X        |        |
| coding               | nvarchar  | X        |        |
| form                 | nvarchar  |          |        |
| text                 | nvarchar  | X        |        |
| unit                 | nvarchar  |          |        |

## MedicationAdministration
*Dataset Shape* **9** - A representation of the FHIR <a href="https://www.hl7.org/fhir/medicationadministration.html" target="_blank"> MedicationAdministration</a> Resource

!!! example "FHIR documention describes as"
    This resource covers the **administration of all medications and vaccines**. Please refer to the Immunization Resource/Profile for the treatment of vaccines. It will principally be used within care settings (including inpatient) to record the capture of medication administrations, including self-administrations of oral medications, injections, intra-venous adjustments, etc. It can also be used in outpatient settings to record allergy shots and other non-immunization administrations. In some cases, it might be used for home-health reporting, such as recording self-administered or even device-administered insulin

SQL Columns (<a href="https://github.com/uwrit/leaf/blob/master/src/server/Model/Cohort/MedicationAdministration.cs" target="_blank">source</a>):

| Name                 | Type      | Required | Is PHI |
| -----------------    | --------- | -------- | ------ |
| personId             | nvarchar  | X        | X      |
| encounterId          | nvarchar  | X        | X      |
| code                 | nvarchar  | X        |        |
| coding               | nvarchar  | X        |        |
| doseQuantity         | numeric   |          |        |
| doseUnit             | nvarchar  |          |        |
| effectiveDateTime    | datetime  | X        | X      |
| route                | nvarchar  |          |        |
| text                 | nvarchar  | X        |        |
