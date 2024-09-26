# Patient Data Feed: Future Considerations

## 1. Additional Resources

The following table outlines additional resources that may be supported in future versions of the Patient Data Feed. These resources are not required for the current implementation but are being considered for future expansion.

**Servers may choose to support any subset of these resources and filters**.

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

## 2. Event Trigger Definitions

### 2.3 Profile-Specific Mapping Advice

Here's how the `update` event maps to specific profiles for resources in the futures table:

- **AllergyIntolerance**:
  - `update`: Triggered when:
    - Initial allergy or intolerance information becomes available
    - Clinical status or verification status changes
    - Criticality is updated
    - Reaction information is added or modified

- **CarePlan**:
  - `update`: Triggered when:
    - Initial care plan details become available
    - Care plan status changes (e.g., from 'draft' to 'active')
    - Goals, activities, or interventions are added, modified, or completed
    - Care team members are updated

- **CareTeam**:
  - `update`: Triggered when:
    - Initial care team information becomes available
    - Team members are added or removed
    - Roles or responsibilities of team members change
    - Care team status is updated

- **Condition**:
  - `update`: Triggered when:
    - Initial condition information becomes available
    - Clinical status or verification status changes
    - Condition severity or stage is updated
    - Related encounters or episodes of care are linked

- **Coverage**:
  - `update`: Triggered when:
    - Initial coverage information becomes available
    - Coverage status changes
    - Policy details are modified
    - Beneficiary or payor information is updated

- **Goal**:
  - `update`: Triggered when:
    - Initial goal information becomes available
    - Goal status changes
    - Target dates or values are modified
    - Achievement status is updated

- **Immunization**:
  - `update`: Triggered when:
    - Initial immunization record is created
    - Immunization status is updated (e.g., from 'in-progress' to 'completed')
    - Additional dose information is added
    - Adverse events are recorded

- **MedicationDispense**:
  - `update`: Triggered when:
    - Initial medication dispense record is created
    - Dispense status changes
    - Quantity or days supply is modified
    - Pickup or delivery information is updated

- **MedicationRequest**:
  - `update`: Triggered when:
    - Initial medication request is created
    - Status changes (e.g., from 'active' to 'completed' or 'stopped')
    - Dosage instructions are modified
    - Fulfillment information is updated

- **Patient**:
  - `update`: Triggered when:
    - Demographic information is modified
    - Contact details are updated
    - Marital status changes
    - Death information is recorded

- **Procedure**:
  - `update`: Triggered when:
    - Initial procedure information becomes available
    - Procedure status changes (e.g., from 'in-progress' to 'completed')
    - Complications or follow-up details are added
    - Procedure notes or reports are updated

- **QuestionnaireResponse**:
  - `update`: Triggered when:
    - Initial questionnaire response is recorded
    - Answers are modified or added
    - Status changes (e.g., from 'in-progress' to 'completed')
    - Linked encounters or authors are updated

- **RelatedPerson**:
  - `update`: Triggered when:
    - Initial related person information is recorded
    - Relationship type or period changes
    - Contact information is modified
    - Active status is updated

- **ServiceRequest**:
  - `update`: Triggered when:
    - Initial service request is created
    - Request status changes
    - Ordered item or service is modified
    - Scheduled date or performer is updated

- **Specimen**:
  - `update`: Triggered when:
    - Initial specimen information is recorded
    - Specimen status changes
    - Collection details are modified
    - Processing or storage information is updated

These examples illustrate how the `update` trigger might be applied to the resources in the futures table. Implementers should define specific criteria for when `update` notifications are sent based on their system's capabilities and clinical relevance.

## 3. Future Enhancements

Future work on the Patient Data Feed may include:

1. Refining the `update` event trigger definition and its application across different resource types
2. Improving the structure and content of SubscriptionTopic resources
3. Enhancing discovery mechanisms for available topics, triggers, and filters
4. Formalizing the compositional nature of topics and triggers
5. Expanding support for additional resources as listed in the table above
6. Adding new triggers as needed based on implementer feedback
7. Improving performance optimization techniques for high-volume subscriptions
8. Enhancing security measures and providing more detailed guidance on implementation

As the FHIR community continues to develop and standardize subscription-related features, this specification will be updated to align with best practices and emerging standards.