# Patient Data Feed: Additional Resources

## 1. Overview

This page outlines additional resources that may be supported by Servers implementing the Patient Data Feed. These resources are not required for the current implementation but are considered valuable for a more comprehensive patient data feed. Servers may choose to support any subset of these resources and filters based on their capabilities and use cases.

## 2. Optional Resources Table

The following table outlines additional resources that Servers may support for the Patient Data Feed:

<table>
  <thead>
    <tr>
      <th>Resource</th>
      <th>Required-Support Triggers</th>
      <th>Conditional-Support Triggers</th>
      <th>Required-Support Filters</th>
      <th>Recommended-Support Filters</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>AllergyIntolerance</td>
      <td><nobr>create</nobr><br><nobr>update</nobr></td>
      <td><nobr>delete</nobr></td>
      <td>patient<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>CarePlan</td>
      <td><nobr>create</nobr><br><nobr>update</nobr></td>
      <td><nobr>delete</nobr></td>
      <td>patient<br>category<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>CareTeam</td>
      <td><nobr>create</nobr><br><nobr>update</nobr></td>
      <td><nobr>delete</nobr></td>
      <td>patient<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>Condition</td>
      <td><nobr>create</nobr><br><nobr>update</nobr></td>
      <td><nobr>delete</nobr></td>
      <td>patient<br>category<br>trigger</td>
      <td>code</td>
    </tr>
    <tr>
      <td>Coverage</td>
      <td><nobr>create</nobr><br><nobr>update</nobr></td>
      <td><nobr>delete</nobr></td>
      <td>patient<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>Goal</td>
      <td><nobr>create</nobr><br><nobr>update</nobr></td>
      <td><nobr>delete</nobr></td>
      <td>patient<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>Immunization</td>
      <td><nobr>create</nobr><br><nobr>update</nobr></td>
      <td><nobr>delete</nobr></td>
      <td>patient<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>MedicationDispense</td>
      <td><nobr>create</nobr><br><nobr>update</nobr></td>
      <td><nobr>delete</nobr></td>
      <td>patient<br>type<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>MedicationRequest</td>
      <td><nobr>create</nobr><br><nobr>update</nobr></td>
      <td><nobr>delete</nobr></td>
      <td>patient<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>Patient</td>
      <td><nobr>create</nobr><br><nobr>update</nobr></td>
      <td><nobr>delete</nobr></td>
      <td>_id<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>Procedure</td>
      <td><nobr>create</nobr><br><nobr>update</nobr></td>
      <td><nobr>delete</nobr></td>
      <td>patient<br>trigger</td>
      <td>code</td>
    </tr>
    <tr>
      <td>QuestionnaireResponse</td>
      <td><nobr>create</nobr><br><nobr>update</nobr></td>
      <td><nobr>delete</nobr></td>
      <td>patient<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>RelatedPerson</td>
      <td><nobr>create</nobr><br><nobr>update</nobr></td>
      <td><nobr>delete</nobr></td>
      <td>patient<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>ServiceRequest</td>
      <td><nobr>create</nobr><br><nobr>update</nobr></td>
      <td><nobr>delete</nobr></td>
      <td>patient<br>category<br>trigger</td>
      <td>code</td>
    </tr>
    <tr>
      <td>Specimen</td>
      <td><nobr>create</nobr><br><nobr>update</nobr></td>
      <td><nobr>delete</nobr></td>
      <td>patient<br>trigger</td>
      <td></td>
    </tr>
  </tbody>
</table>

## 3. Guidance for "update" Trigger Implementation

When implementing the `update` trigger for these optional resources, Servers should consider the following guidance:

- **AllergyIntolerance**:
    - Initial allergy or intolerance information becomes available
    - Clinical status or verification status changes
    - Criticality is updated
    - Reaction information is added or modified

- **CarePlan**:
    - Initial care plan details become available
    - Care plan status changes (e.g., from 'draft' to 'active')
    - Goals, activities, or interventions are added, modified, or completed
    - Care team members are updated

- **CareTeam**:
    - Initial care team information becomes available
    - Team members are added or removed
    - Roles or responsibilities of team members change
    - Care team status is updated

- **Condition**:
    - Initial condition information becomes available
    - Clinical status or verification status changes
    - Condition severity or stage is updated
    - Related encounters or episodes of care are linked

- **Coverage**:
    - Initial coverage information becomes available
    - Coverage status changes
    - Policy details are modified
    - Beneficiary or payor information is updated

- **Goal**:
    - Initial goal information becomes available
    - Goal status changes
    - Target dates or values are modified
    - Achievement status is updated

- **Immunization**:
    - Initial immunization record is created
    - Immunization status is updated (e.g., from 'in-progress' to 'completed')
    - Additional dose information is added
    - Adverse events are recorded

- **MedicationDispense**:
    - Initial medication dispense record is created
    - Dispense status changes
    - Quantity or days supply is modified
    - Pickup or delivery information is updated

- **MedicationRequest**:
    - Initial medication request is created
    - Status changes (e.g., from 'active' to 'completed' or 'stopped')
    - Dosage instructions are modified
    - Fulfillment information is updated

- **Patient**:
    - Demographic information is modified
    - Contact details are updated
    - Marital status changes
    - Death information is recorded

- **Procedure**:
    - Initial procedure information becomes available
    - Procedure status changes (e.g., from 'in-progress' to 'completed')
    - Complications or follow-up details are added
    - Procedure notes or reports are updated

- **QuestionnaireResponse**:
    - Initial questionnaire response is recorded
    - Answers are modified or added
    - Status changes (e.g., from 'in-progress' to 'completed')
    - Linked encounters or authors are updated

- **RelatedPerson**:
    - Initial related person information is recorded
    - Relationship type or period changes
    - Contact information is modified
    - Active status is updated

- **ServiceRequest**:
    - Initial service request is created
    - Request status changes
    - Ordered item or service is modified
    - Scheduled date or performer is updated

- **Specimen**:
    - Initial specimen information is recorded
    - Specimen status changes
    - Collection details are modified
    - Processing or storage information is updated

This guidance illustrates how the `update` trigger might be applied to the additional resources. Implementers eill document specific criteria for when `update` notifications are sent based on their system's capabilities and clinical relevance.


## 4. Future Enhancements

As the FHIR community continues to develop and standardize subscription-related features, this list of additional resources may be updated, and resources may graduate from this list to the [Required Resources Table 1](spec.md).

Implementers are encouraged to provide feedback on their experiences with these additional resources to help shape future versions of this specification align with real-world experience.
