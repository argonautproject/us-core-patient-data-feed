# Patient Data Feed: Subscriptions

## 1. Overview

The Patient Data Feed is an optional feature for servers implementing US Core. It allows clients to receive FHIR Subscription Notifications when changes occur to patient-oriented data.

Servers can start simple. A minimal implementation could support just Encounters and Laboratory Observations. From there, servers can expand to cover more resources as needed. This flexibility lets implementers balance effort and usefulness.

This specification defines a canonical topic URL, subscription filters, and conformance requirements for the Patient Data Feed. It builds on FHIR R4 definitions from http://hl7.org/fhir/uv/subscriptions-backport.

## 2. Canonical Topic URL

The canonical topic URL for the Patient Data Feed is:

`http://hl7.org/fhir/us/core/SubscriptionTopic/patient-data-feed`

Clients use this topic URL when creating Subscriptions to indicate they are requesting the Patient Data Feed.

## 3. Supported Resources and Standardized Filters

The table below defines the resources and their US Core search parameters for the Patient Feed. Servers may choose to support any subset of these resources and filters, as long as they meet the requirements described in _Conformance_.

Table 1: Resources and Filters for the Patient Data Feed

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
| Patient | _id |
| Procedure | patient, code |
| QuestionnaireResponse | patient |
| RelatedPerson | patient |
| ServiceRequest | patient, category, code |
| Specimen | patient |

> **Maintenance of Table 1**
> 
> 1. Updates will be made any time new patient-focused resources are added to US Core.
> 2. For each new resource, filters will be predefined whenever US Core's required search parameters include:
>    - Patient context
>    - Category-level codes
>    - Instance-level codes

## 4. Conformance

1. General Requirements
   1. Support for the Patient Data Feed is optional for servers implementing US Core.
   1. Servers that choose to support the Patient Data Feed SHALL implement the following requirements.

2. Resource and Filter Support
   1. Servers SHALL support at least one resource type from the list in Table 1.
   1. Servers that support the US Core Encounter profile SHALL support subscriptions for Encounter resources.
   1. Servers that support the US Core DocumentReference profile SHALL support subscriptions for DocumentReference resources.
   1. Servers that support the US Core Laboratory Result Observation profile SHALL support subscriptions for Observation resources. Specifically:
      - SHALL support `Observation?category=laboratory` criteria
      - SHALL accept subscription requests for Observation resources that don't include a category filter. When accepting Observation subscriptions without a category filter, servers SHOULD append any implicit limits to the subscription. For example:
        - If the server only supports laboratory observations, it would append `&category=laboratory` to the criteria
        - If the server supports multiple categories but not all, it would append the supported categories (e.g., `&category=laboratory,vital-signs`)
   1. For each supported resource type, servers SHALL support the `patient=` filter (or `_id=` for Patient resources).
   1. For each supported resource type, servers SHALL support the `triggering-event=` filter for any supported events.
   1. Servers MAY support the additional filters listed in Table 1 for each resource type.
   1. Servers MAY support filters beyond those listed in Table 1 for each resource type.
   1. Servers SHOULD align any additional filters with existing search parameter names, when applicable.

3. Notifications when Resources Change
   1. Servers SHALL support notifications when a resource is first created.
   1. Servers SHALL support notifications when a resource status reaches a terminal value (e.g., `entered-in-error`) or the resource is deleted.
   1. Servers SHALL support [labeled notifications](triggering-events.md) for the following mandated events:
       - Servers that support the US Core Encounter profile SHALL support `encounter-start`, `encounter-end` [Core Events](triggering-events.md#2-core-events).
       - Servers that support the US Core Laboratory Result Observation profile SHALL support `result-available`, `result-amended` [Core Events](triggering-events.md#2-core-events).
       - Servers that support the US Core DocumentReference profile SHALL support `note-sign`, `note-amended` [Core Events](triggering-events.md#2-core-events).
   1. Servers MAY send labeled notifications with additional event codes to convey additional semantics about the triggering event.
   1. Servers SHOULD support notifications for all status changes of supported resource types.
   1. Servers SHOULD support notifications for any clinically meaningful changes to the resource.
   1. Servers MAY support notifications for other types of changes (e.g., updates to specific fields).
   1. Servers MAY generate notifications that do not represent actual changes to the resource (e.g., due to internal processing events).

   > Note: It is recognized that EHR systems may have limitations in their ability to generate notifications for every status change or clinically meaningful event. The intent is to provide as comprehensive coverage as possible while allowing for system-specific constraints.

   > Example: An EHR system might base its implementation on an existing event catalog, such as HL7v2 event codes or a custom event catalog. For instance, it could map all ADT (Admission, Discharge, Transfer) A* events from HL7v2 to notifications for the Encounter resource. This approach would likely capture most significant status changes and clinically relevant events, even if it doesn't guarantee coverage of every possible status change.

4. Subscription Channel and Payload
    1. Servers SHALL support the `rest-hook` channel type for notification delivery.
    1. Servers SHALL support the `empty` and `id-only` payload types for notifications.
    1. Servers MAY support additional channel types and payload types

5. Error Handling and Documentation
    1. Servers SHALL clearly document the following in their developer-facing documentation:
        - Supported resources and filters
        - Supported notification triggers
        - Supported channel types
        - Supported payload types 
    1. Servers SHOULD provide clear error messages when rejecting subscription requests due to unsupported features.

7. Handling Multiple Resource Types and Filters
    1. Clients MAY include filters for multiple resource types in a single Subscription request.
    2. If a patient filter is included, it SHALL be applied consistently across all resource types in the subscription.
    3. Servers SHOULD adjust the requested Subscription before persisting, based on the supported types and filters (Note: [search self link](https://www.hl7.org/fhir/search.html#selflink) uses a similar technique.)
    4. Clients SHOULD review the persisted Subscription resource to understand which resource types and filters are in effect.
    * _Examples_
        * A server might remove "CareTeam" from the requested Subscription to indicate that CareTeam resources will never trigger notifications.
        * A server might append a "category" filter clause to the requested Subscription's Observation filter to indicate that other categories will never trigger notifications.

## 5. Example Subscription Request

Here's an example of how a client might request a subscription for laboratory observations and diagnostic reports for a specific patient:

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