# Assigning Event Codes to Subscription Notifications

## 1. Overview

This document describes how servers can provide information about triggering events that affect patient data. 

The primary goals:
1. Establish consistent labels for "Core Events"
1. Enable clients to focus subscriptions based on triggering events
1. Inform the future development of a more complete event catalog

## 2. Core Events

The following triggering events are defined for system
`http://hl7.org/fhir/us/core/CodeSystem/triggering-event`:

| Code | Display | Definition |
|------|---------|------------|
| `encounter-start` | Encounter Start | An encounter has started or a patient has been admitted |
| `encounter-end` | Encounter End | An encounter has ended or a patient has been discharged |
| `result-available` | Result Available | A result has become available (e.g., finalized or preliminary) |
| `result-amended` | Result Amended | An existing result has been amended (e.g., corrected, updated) |
| `note-signed` | Note Signed | A clinical note has been signed |
| `note-amended` | Note Amended | An existing clinical note has been amended |

Servers are expected to map internal events to these core events. The mapping can be from underlying private events, HL7v2 events, or other representations. Servers have flexibility in interpreting how these events apply to their specific workflows, as long as they maintain the spirit and intent of the defined events.

## 3. Additional Event Codes

Servers MAY choose to support additional event codes beyond the core ones. These can be custom codes or drawn from existing standards such as HL7v2 event codes. Here's an example catalog that includes both custom codes and HL7v2 event codes:

- `http://terminology.hl7.org/CodeSystem/v2-0003|A01`: ADT/ACK - Admit/visit notification
- `http://terminology.hl7.org/CodeSystem/v2-0003|A03`: ADT/ACK - Discharge/end visit
- `http://terminology.hl7.org/CodeSystem/v2-0003|R01`: ORU/ACK - Unsolicited observation message
- `http://example.com/events|ALLERGY_CONFIRMED`:An allergy has been confirmed

When using HL7v2 event codes, Servers should use the fixed system `http://terminology.hl7.org/CodeSystem/v2-0003` and the code should indicate the event type (e.g., 'A01', 'R01').

## 4. Subscription Request Example

A client can request a US Core Patient Data Feed subscription filtered by triggering event code:

```json
{
  "resourceType": "Subscription",
  "status": "requested",
  "reason": "Notify only when encounters end",
  "criteria": "http://hl7.org/fhir/us/core/SubscriptionTopic/patient-data-feed",
  "_criteria": {
    "extension": [
      {
        "url": "http://hl7.org/fhir/uv/subscriptions-backport/StructureDefinition/backport-filter-criteria",
        "valueString": "Encounter?patient=123456&triggering-event=encounter-end"
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

The corresponding notifications will include the triggering event information:

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
            "reference": "Encounter/123"
          }
        },
        {
          "name": "triggering-event",
          "valueCoding": {
            "system": "http://hl7.org/fhir/us/core/CodeSystem/triggering-event",
            "code": "encounter-end"
          }
        }
      ]
    }
  ]
}
```

## 5. Implementation Guidelines

### 5.1 For Servers

1. Document the triggering event codes used in your system that relate to US Core Patient Data Feed resources. This can include custom codes or HL7v2 event codes.
2. Include the triggering event code in the `notification-event` part of the notification Parameters resource.
3. If supporting filtering, document the filter parameter name (e.g., `triggering-event`) for use with the US Core Patient Data Feed.

### 5.2 For Clients

1. Consult the Server's documentation for available triggering event codes related to the US Core Patient Data Feed.
2. Use the documented filter parameter to subscribe to specific triggering events within the Patient Data Feed if desired.
3. Parse the `triggering-event` value from the `notification-event` part of received notifications.

## 6. Benefits

1. More detailed understanding of what triggered changes in the FHIR representation of patient data within the US Core Patient Data Feed.
2. Enhanced ability to track and audit specific events within the Server as they relate to the Patient Data Feed.
3. Potential for more targeted and efficient processing of Patient Data Feed notifications by clients.
4. Improved insights into EHR workflows and data changes, facilitating future enhancements to FHIR and US Core standards.
5. Opportunity for EHRs to showcase the depth and quality of their processes.
6. Clients can choose to leverage or ignore this additional information based on their needs, maintaining compatibility across diverse implementations.
7. Use of standardized event codes improves interoperability and understanding across different systems.
