# Patient Data Feed: Subscription Implementation Guidance

## 1. Overview

This guidance describes the "Patient Data Feed" capability, an optional feature for servers implementing US Core. It defines a standardized set of named filters and resources that servers may support for subscriptions related to US Core data.

## 2. Canonical URL

The canonical URL for the Patient Data Feed is:

`http://hl7.org/fhir/us/core/SubscriptionTopic/patient-data-feed`

Clients should use this URL in their Subscription resources to indicate they are requesting the Patient Data Feed.

## 3. Supported Resources and Standardized Filters

The following list defines the resources and their associated standardized filters for the Patient Feed. Servers may choose to support any subset of these resources and filters.

### AllergyIntolerance
- patient: Filter by patient

### CarePlan
- patient: Filter by patient
- category: Filter by CarePlan category

### CareTeam
- patient: Filter by patient

### Condition
- patient: Filter by patient
- category: Filter by Condition category
- code: Filter by Condition code

### Coverage
- patient: Filter by patient (beneficiary)

### DiagnosticReport
- patient: Filter by patient
- category: Filter by DiagnosticReport category
- code: Filter by DiagnosticReport code

### DocumentReference
- patient: Filter by patient
- category: Filter by DocumentReference category
- type: Filter by DocumentReference type

### Encounter
- patient: Filter by patient
- type: Filter by Encounter type

### Goal
- patient: Filter by patient

### Immunization
- patient: Filter by patient

### MedicationDispense
- patient: Filter by patient
- type: Filter by MedicationDispense type

### MedicationRequest
- patient: Filter by patient

### Observation
- patient: Filter by patient
- category: Filter by Observation category
- code: Filter by Observation code

### Patient
- identifier: Filter by Patient identifier

### Procedure
- patient: Filter by patient
- code: Filter by Procedure code

### QuestionnaireResponse
- patient: Filter by patient

### RelatedPerson
- patient: Filter by patient

### ServiceRequest
- patient: Filter by patient
- category: Filter by ServiceRequest category
- code: Filter by ServiceRequest code

### Specimen
- patient: Filter by patient


> **Maintenance of Guidance**
> 
> This list of supported resources and filters will be maintained as follows:
> 
> 1. Updates will be made any time new patient-focused resources are added to US Core.
> 2. For each new resource, filters will be predefined to include:
>    - Patient context
>    - Any category-level codes
>    - Any instance-level codes
> 3. The community will be notified of any updates to this guidance.
> 4. Version history will be maintained to allow implementers to track changes over time.

## 4. Notification Triggers

Servers supporting the Patient Data Feed have some flexibility in determining what changes trigger notifications, but must adhere to the following guidelines:

1. Servers SHALL support notifications when resource statuses change for any resources that they support within the Patient Data Feed. Note that resource creation is also considered a status change (from non-existent to existent) for these purposes.

2. Servers have discretion over what other changes trigger notifications (e.g., updates to specific fields) and SHOULD clearly document their notification triggers in their developer-facing documentation.

## 5. Implementation Guidelines

1. Support for the Patient Data Feed is optional for servers implementing US Core.

2. Servers that choose to support the Patient Data Feed SHOULD clearly document which resources and filters they support in their developer-facing documentation.

3. When implementing a filter, servers SHOULD adhere to the standard FHIR search parameter definitions for that filter.

4. Servers MAY add custom filters beyond those listed, and SHOULD clearly document any custom filters.

5. Clients SHOULD be prepared to handle subscription requests being rejected if they include unsupported filters or resources.

6. Servers SHOULD provide clear error messages when rejecting subscription requests due to unsupported features.

## 6. Security Considerations

Servers are responsible for implementing appropriate access control and managing authorization for subscriptions and notifications. Existing security practices and standards for FHIR implementations should be applied to subscription management and notification delivery.

## 7. Topic Modeling and Discovery

The standardization of how to model and publish topics for discovery is ongoing work in the FHIR community. This guidance does not yet address the standardization of the topic resource itself. Instead, it focuses on standardizing the expectations of how clients can create a subscription to a US Core Patient Data Feed.

While formal topic publication is not yet standardized, interoperability can still be achieved around subscriptions using this guidance. Servers and clients can agree on the canonical URL, supported resources, and filters as described in this document.

Future work may include:
1. Standardizing the structure and content of SubscriptionTopic resources
2. Defining discovery mechanisms for available topics
3. Formalizing the relationship between topics and subscriptions

Until then, implementers should rely on this guidance and server-specific documentation for creating and managing subscriptions.

## 8. Example Subscription Request

Here's an example of how a client might request a subscription for laboratory observations and diagnostic reports:

```json
{
  "resourceType": "Subscription",
  "status": "requested",
  "reason": "Notify on new lab results and reports",
  "criteria": "http://hl7.org/fhir/us/core/SubscriptionTopic/patient-data-feed",
  "_criteria": {
    "extension": [
      {
        "url": "http://hl7.org/fhir/uv/subscriptions-backport/StructureDefinition/backport-filter-criteria",
        "valueString": "Observation?category=laboratory&patient=123456"
      },
      {
        "url": "http://hl7.org/fhir/uv/subscriptions-backport/StructureDefinition/backport-filter-criteria",
        "valueString": "DiagnosticReport?category=LAB&patient=123456"
      }
    ]
  },
  "channel": {
    "type": "rest-hook",
    "endpoint": "https://client.example.org/fhir/subscription-notification",
    "payload": "application/fhir+json"
  }
}
```

In this example, the client is requesting notifications for:
1. Laboratory observations for a specific patient (123456)
2. Laboratory diagnostic reports for the same patient

The `criteria` element contains the canonical URL for the Patient Data Feed topic. The `_criteria` element uses extensions to specify the filters for each resource type. This approach allows for multiplexing different resource types within a single subscription.

The server would process this request based on its supported features and either accept the subscription or reject it if it doesn't support the requested filters or resources.
