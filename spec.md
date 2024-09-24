# Patient Data Feed: Subscriptions

## 1. Overview

The Patient Data Feed is an optional feature for servers implementing US Core. It allows clients to receive FHIR Subscription Notifications when changes occur to patient-oriented data.

Servers can start simple. A minimal implementation might support just a few key US Core profiles to enable subscriptions for DiagnosticReport, DocumentReference, Encounter, and Observation. From there, servers can expand to cover more resources as needed. This flexibility lets implementers balance effort and usefulness.

This specification defines a canonical topic URL, subscription filters, triggers, and conformance requirements for the Patient Data Feed. It builds on FHIR R4 definitions from http://hl7.org/fhir/uv/subscriptions-backport.

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
      <th>Required-Support Triggers</th>
      <th>Recommended-Support Triggers</th>
      <th>Required-Support Filters</th>
      <th>Recommended-Support Filters</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>AllergyIntolerance</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr></td>
      <td>patient<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>CarePlan</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr></td>
      <td>patient<br>category<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>CareTeam</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr></td>
      <td>patient<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>Condition</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr></td>
      <td>patient<br>category<br>trigger</td>
      <td>code</td>
    </tr>
    <tr>
      <td>Coverage</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr></td>
      <td>patient<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>DiagnosticReport</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>draft</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr></td>
      <td>patient<br>category<br>trigger</td>
      <td>code</td>
    </tr>
    <tr>
      <td>DocumentReference</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>draft</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr></td>
      <td>patient<br>category<br>trigger</td>
      <td>type</td>
    </tr>
    <tr>
      <td>Encounter</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>active</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr></td>
      <td>patient<br>trigger</td>
      <td>type</td>
    </tr>
    <tr>
      <td>Goal</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr></td>
      <td>patient<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>Immunization</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr></td>
      <td>patient<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>MedicationDispense</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr></td>
      <td>patient<br>type<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>MedicationRequest</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr></td>
      <td>patient<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>Observation</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>draft</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr></td>
      <td>patient<br>category<br>trigger</td>
      <td>code</td>
    </tr>
    <tr>
      <td>Patient</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr></td>
      <td>_id<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>Procedure</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr><br><nobr>active</nobr></td>
      <td>patient<br>trigger</td>
      <td>code</td>
    </tr>
    <tr>
      <td>QuestionnaireResponse</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr></td>
      <td>patient<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>RelatedPerson</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr></td>
      <td>patient<br>trigger</td>
      <td></td>
    </tr>
    <tr>
      <td>ServiceRequest</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr></td>
      <td>patient<br>category<br>trigger</td>
      <td>code</td>
    </tr>
    <tr>
      <td>Specimen</td>
      <td><nobr>create</nobr><br><nobr>delete</nobr><br><nobr>finalize</nobr></td>
      <td><nobr>update</nobr></td>
      <td>patient<br>trigger</td>
      <td></td>
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
> 4. Specific triggering events (e.g., draft, active) will be added to relevant resources as they are defined.

## 4. Triggering Events and Notifications

#### Definitions of Generic Triggers
- `create`: A resource has been created
- `update`: A resource has been updated.
- `delete`: A resource has been deleted
- `draft`: A resource is in a preliminary state with partial content.
- `active`: A resource is currently in use or being acted upon.
- `finalize`: A resource has reached a state that is considered complete or ready for use, sucy as:
   - Entering a status such as "final", "completed", "amended", or "entered-in-error"
   - Reaching a point where the resource is considered clinically relevant and actionable
   - Note: The resource may still be subject to future updates, such as amendments or corrections which could **re-trigger** `finalize`


#### Understanding Events for Key Profiles

- **US Core DocumentReference**:
  - `draft`: A clinical note has draft content available
  - `finalize`: A clinical note has been signed, amended, or otherwise entered a final state.

- **US Core Encounter**:
  - `active`: An encounter has started or a patient has been admitted.
  - `finalize`: An encounter has ended, a patient has been discharged, or the encounter has otherwise entered a final status.

- **US Core Laboratory Observation**:
  - `draft`: A preliminary result has become available.
  - `finalize`: A final result, amended result, or corrected result has become available, or the observation has otherwise entered a final status.

- **US Core DiagnosticReport for Laboratory Results Reporting**:
  - `draft`: A preliminary result has become available.
  - `finalize`: A final result, amended result, or corrected result has become available, or the diagnostic report has otherwise entered a final status.

### 4.2 Conformance Requirements for Triggering Events

For each supported resource type, servers SHALL support the Required-Support Triggers as specified in Table 1, and SHOULD support the Recommended-Support Triggers.

When sending a notification, servers SHALL include all applicable supported triggering event code(s) in the `notification-event` part, `trigger` sub-part of the notification Parameters resource. Multiple trigger codes may be included if a single change satisfies multiple trigger conditions.

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
        }
      ]
    }
  ]
}
```

## 5. Subscription Filters and Requests

Servers SHALL allow clients to create Subscriptions according to http://hl7.org/fhir/uv/subscriptions-backport. This includes "filter-criteria" extensions for each resource where notifications are requested.

For each supported resource type, servers SHALL support the Required-Support Filters as specified in Table 1, and SHOULD support the Recommended-Support Filters.

Servers SHALL support Subscriptions with zero or more supported filters in any filter-criteria extension.

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
        "valueString": "Encounter?patient=123&trigger=finalize"
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

1. Clients MAY include multiple resource types by supplying multiple filter-criteria extensions in a Subscription request; Servers SHALL process all supplied filter-criteria extensions.
2. Clients SHALL NOT include `patient=` filters for different patients in the same Subscription.
3. Servers SHOULD adjust the requested Subscription before persisting, based on the supported types and filters.
4. Clients SHOULD review the persisted Subscription resource to understand which resource types and filters are in effect.

Examples:
- A server that does not support CareTeam resources might remove the client's `CareTeam` filter-criteria extension to indicate that CareTeam resources will never trigger notifications.
- A server that only supports notifications for laboratory observations might adjust the client's `Observation` filter-criteria extension to `Observation?category=laboratory` to indicate that other categories will never trigger notifications.

## 6. Additional Conformance Requirements

### 6.1 General

1. Support for the Patient Data Feed is optional for servers implementing US Core.
2. Servers that choose to support the Patient Data Feed SHALL implement the following requirements.

### 6.2 Resource Support

1. Servers SHALL support at least one resource type from the list in Table 1.
2. Servers supporting the following profiles SHALL support subscriptions for their resources:
   - US Core DocumentReference Profile 
   - US Core Encounter Profile
   - US Core DiagnosticReport Profile for Laboratory Results Reporting
   - US Core Laboratory Result Observation Profile

### 6.3 Filter Support

1. Servers MAY support filters beyond those listed in Table 1 for each resource type.
2. Servers SHOULD align any additional filters with existing search parameter names, when applicable.

### 6.4 Delivery (Channel and Payload Support)
1. Servers SHALL support the `rest-hook` channel type for notification delivery.
2. Servers SHALL support the `empty` and `id-only` payload types for notifications.
3. Servers MAY support additional channel types and payload types.

### 6.6 Documentation and Error Handling
1. Servers SHALL clearly document the following in their developer-facing documentation:
   - Supported resources, filters, and triggers
   - Supported notification triggers
   - Supported channel types
   - Supported payload types 
2. Servers SHOULD provide clear error messages when rejecting subscription requests due to unsupported features.

## 7. Additional Considerations

### 7.1 Performance Optimization

Servers implementing the Patient Data Feed should consider the following performance optimizations:

1. Event Coalescing: When multiple changes occur to a single resource in rapid succession, servers can coalesce these changes into a single event notification that includes all applicable trigger codes to represent the nature of the changes.

2. Event Batching: For subscriptions with high update frequencies, servers can implement batching strategies to group multiple events into a single notification bundle.


### 7.2 Security Considerations

While detailed security implementations are beyond the scope of this specification, servers must maintain the same level of access control and security for subscription notifications as they do for direct REST API access. Key considerations include authentication, authorization, encryption in transit (https), and audit logging.  Implementers are encouraged to refer to the FHIR Security and Privacy Module (http://hl7.org/fhir/security.html) and relevant security best practices for additional guidance.

## 8. Future Work

The standardization of how to model and publish topics for discovery is ongoing work in the FHIR community. US Core's Patient Data Feed does not yet address the standardization of the SubscriptionTopic resource itself. Instead, it focuses on standardizing the functionality of the `patient-data-feed` topic and the expectations of Subscription management, allowing for interoperability based on the canonical URL, supported resources, filters, and triggers.

Future work may include:
1. Improving the structure and content of SubscriptionTopic resources
2. Enhancing discovery mechanisms for available topics, triggers, and filters
3. Formalizing the compositional nature of topics and triggers

