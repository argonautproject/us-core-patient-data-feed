# Patient Data Feed: Subscriptions

## 1. Overview

The Patient Data Feed is an optional feature for servers implementing US Core. It allows clients to receive FHIR Subscription Notifications when changes occur to patient-oriented data.

Servers can start simple. A minimal implementation might support just a few key US Core profiles to enable subscriptions for DiagnosticReport, DocumentReference, Encounter, and Observation. From there, servers can expand to cover more resources as needed. This flexibility lets implementers balance effort and usefulness.

This specification defines a canonical topic URL, subscription filters, and conformance requirements for the Patient Data Feed. It builds on FHIR R4 definitions from http://hl7.org/fhir/uv/subscriptions-backport.

## 2. Canonical Topic URL

The canonical topic URL for the Patient Data Feed is:

`http://hl7.org/fhir/us/core/SubscriptionTopic/patient-data-feed`

Clients use this topic URL when creating Subscriptions to indicate they are requesting the Patient Data Feed.

## 3. Resources, Filters, and Triggers

The table below defines the resources, their US Core search parameters, and the standardized triggering events for the Patient Feed. **Servers may choose to support any subset of these resources and filters**, as long as they meet the requirements described in _Conformance_.

<table>
  <thead>
    <tr>
      <th>Resource</th>
      <th>Required Filters</th>
      <th>Recommended Filters</th>
      <th>Required Triggers (Generic)</th>
      <th>Required Triggers (Specific)</th>
      <th>Recommended Triggers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>AllergyIntolerance</td>
      <td>patient<br>trigger</td>
      <td></td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td></td>
      <td><nobr>update</nobr></td>
    </tr>
    <tr>
      <td>CarePlan</td>
      <td>patient<br>category<br>trigger</td>
      <td></td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td></td>
      <td><nobr>update</nobr></td>
    </tr>
    <tr>
      <td>CareTeam</td>
      <td>patient<br>trigger</td>
      <td></td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td></td>
      <td><nobr>update</nobr></td>
    </tr>
    <tr>
      <td>Condition</td>
      <td>patient<br>category<br>trigger</td>
      <td>code</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td></td>
      <td><nobr>update</nobr></td>
    </tr>
    <tr>
      <td>Coverage</td>
      <td>patient<br>trigger</td>
      <td></td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td></td>
      <td><nobr>update</nobr></td>
    </tr>
    <tr>
      <td>DiagnosticReport</td>
      <td>patient<br>category<br>trigger</td>
      <td>code</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>available</nobr><nobr>amend</nobr></td>
      <td><nobr>update</nobr></td>
    </tr>
    <tr>
      <td>DocumentReference</td>
      <td>patient<br>category<br>trigger</td>
      <td>type</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>sign</nobr><br><nobr>amend</nobr></td>
      <td><nobr>update</nobr></td>
    </tr>
    <tr>
      <td>Encounter</td>
      <td>patient<br>trigger</td>
      <td>type</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>encounter-start</nobr><br><nobr>encounter-end</nobr></td>
      <td><nobr>update</nobr></td>
    </tr>
    <tr>
      <td>Goal</td>
      <td>patient<br>trigger</td>
      <td></td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td></td>
      <td><nobr>update</nobr></td>
    </tr>
    <tr>
      <td>Immunization</td>
      <td>patient<br>trigger</td>
      <td></td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td></td>
      <td><nobr>update</nobr></td>
    </tr>
    <tr>
      <td>MedicationDispense</td>
      <td>patient<br>type<br>trigger</td>
      <td></td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td></td>
      <td><nobr>update</nobr></td>
    </tr>
    <tr>
      <td>MedicationRequest</td>
      <td>patient<br>trigger</td>
      <td></td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td></td>
      <td><nobr>update</nobr></td>
    </tr>
    <tr>
      <td>Observation</td>
      <td>patient<br>category<br>trigger</td>
      <td>code</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>available</nobr><br><nobr>amend</nobr></td>
      <td><nobr>update</nobr></td>
    </tr>
    <tr>
      <td>Patient</td>
      <td>_id<br>trigger</td>
      <td></td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td></td>
      <td><nobr>update</nobr></td>
    </tr>
    <tr>
      <td>Procedure</td>
      <td>patient<br>trigger</td>
      <td>code</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td></td>
      <td><nobr>update</nobr></td>
    </tr>
    <tr>
      <td>QuestionnaireResponse</td>
      <td>patient<br>trigger</td>
      <td></td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td></td>
      <td><nobr>update</nobr></td>
    </tr>
    <tr>
      <td>RelatedPerson</td>
      <td>patient<br>trigger</td>
      <td></td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td></td>
      <td><nobr>update</nobr></td>
    </tr>
    <tr>
      <td>ServiceRequest</td>
      <td>patient<br>category<br>trigger</td>
      <td>code</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td></td>
      <td><nobr>update</nobr></td>
    </tr>
    <tr>
      <td>Specimen</td>
      <td>patient<br>trigger</td>
      <td></td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td></td>
      <td><nobr>update</nobr></td>
    </tr>
  </tbody>
</table>

> **Maintenance of Table 1**
> 
> 1. Updates will be made any time new patient-focused resources are added to US Core.
> 2. For each new resource, filters will be predefined whenever US Core's required search parameters include:
>    - Patient context
>    - Category-level codes
>    - Instance-level codes
> 3. The mandatory generic triggering events (create, delete, finalize) will be included for all resources.
> 4. Specific triggering events (e.g., encounter-start, available) will be added to relevant resources as they are defined.

## 4. Triggering Events and Notifications

### 4.1 Required Triggering Events

For all supported resources:

- Servers SHALL support:
  - `create`: A resource has been created
  - `delete`: A resource has been deleted
  - `finalize`: A resource has reached a state that is considered complete or ready for use. This includes, but is not limited to:
    - Entering a status such as "final" or "completed"
    - Reaching a point where the resource is considered clinically relevant and actionable
    - The resource may still be subject to future updates, such as amendments or corrections which could **re-trigger** `finalize`

- Servers SHOULD support:
  - `update`: A resource has been updated (this is a superset of `finalize`)

Note: A single change to a resource may trigger multiple events simultaneously (e.g., both `update` and `finalize`).

Servers that support the following US Core profiles SHALL also support these associated events:

- "US Core DocumentReference":
  - `sign`: A clinical note has been signed
  - `amend`: An existing clinical note has been amended
- "US Core Encounter":
  - `encounter-start`: An encounter has started or a patient has been admitted
  - `encounter-end`: An encounter has ended or a patient has been discharged
- "US Core Laboratory Observation":
  - `available`: A result has become available (e.g., preliminary or finalized)
  - `amend`: An existing result has been amended (e.g., corrected or updated)

- "US Core DiagnosticReport for Laboratory Results Reporting":
  - `available`: A result has become available (e.g., preliminary or finalized)
  - `amend`: An existing result has been amended (e.g., corrected or updated)

Note: These events can be detected by evaluating resource state, even in systems without native event-based processing. Servers are responsible for determining how to identify these events based on their specific implementation.

The triggering event codes above are defined in the `http://hl7.org/fhir/us/core/CodeSystem/trigger` CodeSystem.

### 4.2 Conveying Trigger Event Codes in Notifications

When sending a notification, servers SHALL include all applicable triggering event code(s) in the `notification-event` part, `trigger` sub-part of the notification Parameters resource. Multiple trigger codes may be included if a single change satisfies multiple trigger conditions.

Example of a Parameters resource for a notification with multiple triggers:

```json
{
  "resourceType": "Parameters",
  "parameter": [
    {
      "name": "subscription",
      "valueReference": {
        "reference": "Subscription/example"
      }
    },
    {
      "name": "topic",
      "valueCanonical": "http://hl7.org/fhir/us/core/SubscriptionTopic/patient-data-feed"
    },
    {
      "name": "status",
      "valueCode": "active"
    },
    {
      "name": "type",
      "valueCode": "event-notification"
    },
    {
      "name": "events-since-subscription-start",
      "valueString": "5"
    },
    {
      "name": "notification-event",
      "part": [
        {
          "name": "event-number",
          "valueString": "5"
        },
        {
          "name": "timestamp",
          "valueInstant": "2023-09-05T14:30:00Z"
        },
        {
          "name": "focus",
          "valueReference": {
            "reference": "Observation/lab-result-123"
          }
        },
        {
          "name": "trigger",
          "valueCoding": {
            "system": "http://hl7.org/fhir/us/core/CodeSystem/trigger",
            "code": "finalize"
          }
        },
        {
          "name": "trigger",
          "valueCoding": {
            "system": "http://hl7.org/fhir/us/core/CodeSystem/trigger",
            "code": "amend"
          }
        }
      ]
    }
  ]
}
```

## 5. Subscription Filters and Requests

Servers SHALL allow clients to create Subscriptions according to http://hl7.org/fhir/uv/subscriptions-backport.

### 5.1 Required Filter Parameters

For each supported resource type, servers SHALL support the Required filters as specified in Table 1 (e.g., `category` for applicable resources), and SHOULD support the Recommended filters.

### 5.2 Exapmle Subscription Request
Example Subscription request, demonstrating filters based on patient, category, and trigger event.

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
        "valueString": "Encounter?patient=123&trigger=encounter-end"
      },
      {
        "url": "http://hl7.org/fhir/uv/subscriptions-backport/StructureDefinition/backport-filter-criteria",
        "valueString": "Observation?patient=123&category=laboratory"
      },
      {
        "url": "http://hl7.org/fhir/uv/subscriptions-backport/StructureDefinition/backport-filter-criteria",
        "valueString": "DiagnosticReport?patient=123&category=LAB"
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

### 5.3 Handling Multiple Resource Types and Filters

1. Clients MAY include multiple resource types in a single Subscription request.
2. If a patient filter is included, it SHALL be applied consistently across all resource types in the subscription.
3. Servers SHOULD adjust the requested Subscription before persisting, based on the supported types and filters.
4. Clients SHOULD review the persisted Subscription resource to understand which resource types and filters are in effect.

Examples:
- A server that does not support CareTeam resources might remove "CareTeam" from the requested Subscription to indicate that CareTeam resources will never trigger notifications.
- A server that only supports notifications for laboratory observations might append a "category" filter clause to the requested Subscription's Observation criteria to indicate that other categories will never trigger notifications.

## 6. Additional Conformance Requirements

### 6.1 General

1. Support for the Patient Data Feed is optional for servers implementing US Core.
2. Servers that choose to support the Patient Data Feed SHALL implement the following requirements.

### 6.2 Resource and Filter Support
1. Servers SHALL support at least one resource type from the list in Table 1.
2. Servers MAY support filters beyond those listed in Table 1 for each resource type.
3. Servers SHOULD align any additional filters with existing search parameter names, when applicable.

### 6.4 Subscription Handling
1. Servers SHALL support the `rest-hook` channel type for notification delivery.
2. Servers SHALL support the `empty` and `id-only` payload types for notifications.
3. Servers MAY support additional channel types and payload types.

### 6.5 Profile-Specific Requirements
1. Servers that support the US Core Encounter profile SHALL support subscriptions for Encounter resources.
2. Servers that support the US Core DocumentReference profile SHALL support subscriptions for DocumentReference resources.
3. Servers that support the US Core Laboratory Result Observation profile SHALL support subscriptions for Observation resources, including:
   - Support for `Observation?category=laboratory` criteria
   - Acceptance of subscription requests for Observation resources without a category filter, with appropriate handling as described in section 5.2.

### 6.6 Documentation and Error Handling
1. Servers SHALL clearly document the following in their developer-facing documentation:
   - Supported resources and filters
   - Supported notification triggers
   - Supported channel types
   - Supported payload types 
2. Servers SHOULD provide clear error messages when rejecting subscription requests due to unsupported features.

## 7. Benefits

1. More targeted and efficient processing of notifications by clients.
2. Detailed understanding of what triggered changes in patient data.
3. Improved interoperability through standardized event codes.
4. Flexibility for servers to expose additional event details while maintaining a core set of standardized codes.

## 8. Future Work

The standardization of how to model and publish topics for discovery is ongoing work in the FHIR community. US Core's Patient Data Feed does not yet address the standardization of the SubscriptionTopic resource itself. Instead, it focuses on standardizing the functionality of the `patient-data-feed` topic and the expectations of Subscription management, allowing for interoperability based on the canonical URL, supported resources, filters, and triggers.

Future work may include:
1. Improving the structure and content of SubscriptionTopic resources
2. Enhancing discovery mechanisms for available topics, triggers, and filters
3. Formalizing the compositional nature of topics and triggers
