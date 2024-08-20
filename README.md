# Patient Data Feed: Subscriptions

## 1. Overview

This guidance describes the "Patient Data Feed", an optional feature for servers implementing US Core. It defines a canonical topic URL and a set of resources with named filters that servers can support to enable subscriptions to patient-focused data.

## 2. Canonical Topic URL

The canonical topic URL for the Patient Data Feed is:

`http://hl7.org/fhir/us/core/SubscriptionTopic/patient-data-feed`

Clients use this topic URL in their Subscription resources to indicate they are requesting the Patient Data Feed.

## 3. Supported Resources and Standardized Filters

Table 1 defines the resources and their associated standardized filters for the Patient Feed. Servers may choose to support any subset of these resources and filters.

Table 1: Supported Resources and Standardized Filters for Patient Data Feed

| Resource | Filters |
|----------|---------|
| AllergyIntolerance | patient |
| CarePlan | patient, category |
| CareTeam | patient |
| Condition | patient, category, code |
| Coverage | patient |
| DiagnosticReport | patient, category, code |
| DocumentReference | patient, category, type |
| Encounter | patient, type |
| Goal | patient |
| Immunization | patient |
| MedicationDispense | patient, type |
| MedicationRequest | patient |
| Observation | patient, category, code |
| Patient | identifier |
| Procedure | patient, code |
| QuestionnaireResponse | patient |
| RelatedPerson | patient |
| ServiceRequest | patient, category, code |
| Specimen | patient |

> **Maintenance of Table 1**
> 
> 1. Updates will be made any time new patient-focused resources are added to US Core.
> 2. For each new resource, filters will be predefined based on standard search parameters to include:
>    - Patient context
>    - Any category-level codes
>    - Any instance-level codes

## 4. Conformance

1. General Requirements
   1. Support for the Patient Data Feed is optional for servers implementing US Core.
   1. Servers that choose to support the Patient Data Feed SHALL implement the following requirements.

2. Resource and Filter Support
   1. Servers SHALL support at least one resource type from the list in Table 1.
   1. For each supported resource type, servers SHALL support the 'patient' filter (or 'identifier' for Patient resources).
   1. Servers MAY support the additional filters listed in Table 1 for each resource type.
   1. Servers MAY support filters beyond those listed in Table 1 for each resource type.
   1. Servers SHOULD align any additional filters with existing search parameter names, when applicable.

3. Notification Triggers
   1. Servers SHALL support notifications when resource statuses change for any resources that they support within the Patient Data Feed. (_Note: Resource creation is considered a status change for these purposes._)
   1. Servers MAY support notifications for other types of changes (e.g., updates to specific fields).

4. Subscription Channel and Payload
   1. Servers SHALL support the `rest-hook` channel type for notification delivery.
   1. Servers SHALL support the `id-only` payload type for notifications.
   1. Servers MAY support additional channel types and payload content types, and SHOULD clearly document any additional supported options.

5. Error Handling and Documentation
   1. Servers SHALL clearly document the following in their developer-facing documentation:
      1. Supported resources and filters
      1. Supported notification triggers
   1. Servers SHOULD provide clear error messages when rejecting subscription requests due to unsupported features.

7. Handling Filters for Multiple Resource Types
   1. Clients MAY include filters for multiple resource types in a single Subscription request.
   1. Servers SHOULD adjust the requested Subscription before persisting it, in order to communicate what filters will be honored.
       1. _Example:_ A server could remove an unsupported resource type from the Subscription to convey the lack of support.
       1. _Example:_ A server could append a "category" filter clause to indicate that only certain categories of Observation are supported.
   1. Clients SHOULD review the persisted Subscription resource to understand which filters are in effect.

## 5. Example Subscription Request

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
    "payload": "application/fhir+json",
    "_payload": {
      "extension": [
        {
          "url": "http://hl7.org/fhir/uv/subscriptions-backport/StructureDefinition/backport-payload-content",
          "valueCode": "id-only"
        }
      ]
    }
  }
}
```

In this example, the client is requesting notifications for:
1. Laboratory observations for a specific patient (123456)
2. Laboratory diagnostic reports for the same patient

The `criteria` element contains the canonical URL for the Patient Data Feed topic. The `_criteria` element uses extensions to specify the filters for each resource type. This approach allows for multiplexing different resource types within a single subscription.

The server would process this request based on its supported features and either accept the subscription or reject it if it doesn't support the requested filters or resources.

## 6. Topic Modeling and Discovery (*future work*)

The standardization of how to model and publish topics for discovery is ongoing work in the FHIR community. US Core's Patient Data Feed does not yet address the standardization of the topic resource itself. Instead, it focuses on standardizing the expectations of Subscription management, allowing for interoperability based on the canonical URL, supported resources, and filters as described in this document.

Future work may include:
1. Improving the structure and content of SubscriptionTopic resources
2. Enhancing discovery mechanisms for available topics, triggers, and filters
3. Formalizing the compositional nature of topics and triggers
