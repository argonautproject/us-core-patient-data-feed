# Extending US Core Patient Data Feed with EHR Triggering Events

## 1. Overview

This document describes how Electronic Health Record (EHR) systems can extend the US Core Patient Data Feed to provide more detailed information about triggering events that affect patient data. 

It's important to note that EHRs typically don't store data natively as FHIR resources. Instead, they map their internal data to FHIR representations when needed. This extension helps bridge the gap between EHR triggering events and the FHIR representation of data in the US Core Patient Data Feed.

In plain language, this extension:
- Allows EHRs to communicate specific events that trigger changes in patient data
- Enables subscribers to optionally filter notifications based on these triggering events
- Provides more context about why data changed, not just that it changed

The primary goals of this extension are:
1. Maintain full transparency and compatibility with the existing US Core Patient Data Feed
2. Provide insights that can inform future standards development around the Patient Data Feed
3. Enable a more detailed understanding of EHR triggering events and their impact on patient data

## 2. Event Code Catalog

EHRs can choose to maintain their own catalog of triggering event codes or use existing standards such as HL7v2 event codes. Here's an example catalog that includes both custom codes and HL7v2 event codes:

- `http://example.com/events|ENC_STARTED`: Patient encounter has started
- `http://example.com/events|ENC_UPDATED`: Patient encounter details have been updated
- `http://example.com/events|ENC_CLOSED`: Patient encounter has been closed
- `http://terminology.hl7.org/CodeSystem/v2-0003|A01`: ADT/ACK - Admit/visit notification
- `http://terminology.hl7.org/CodeSystem/v2-0003|A03`: ADT/ACK - Discharge/end visit
- `http://example.com/events|LAB_ORDERED`: Laboratory test has been ordered
- `http://example.com/events|LAB_COLLECTED`: Laboratory sample has been collected
- `http://terminology.hl7.org/CodeSystem/v2-0003|R01`: ORU/ACK - Unsolicited observation message
- `http://example.com/events|LAB_VERIFIED`: Laboratory result has been verified

When using HL7v2 event codes, EHRs should use the fixed system `http://terminology.hl7.org/CodeSystem/v2-0003` and the code should indicate the event type (e.g., 'A01', 'R01').

## 3. Subscription Request Example

A client can request a US Core Patient Data Feed subscription filtered by triggering event code:

```json
{
  "resourceType": "Subscription",
  "status": "requested",
  "reason": "Notify on lab result unsolicited observation",
  "criteria": "http://hl7.org/fhir/us/core/SubscriptionTopic/patient-data-feed",
  "_criteria": {
    "extension": [
      {
        "url": "http://hl7.org/fhir/uv/subscriptions-backport/StructureDefinition/backport-filter-criteria",
        "valueString": "Observation?category=laboratory&patient=123456&triggering-event=http://terminology.hl7.org/CodeSystem/v2-0003|R01"
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

## 4. Notification Example

When a notification is triggered in the US Core Patient Data Feed, the EHR includes the triggering event code:

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
          "name": "triggering-event",
          "valueCoding": {
            "system": "http://terminology.hl7.org/CodeSystem/v2-0003",
            "code": "R01"
          }
        }
      ]
    }
  ]
}
```

## 5. Implementation Guidelines

### 5.1 For EHRs

1. Document the triggering event codes used in your system that relate to US Core Patient Data Feed resources. This can include custom codes or HL7v2 event codes.
2. Include the triggering event code in the `notification-event` part of the notification Parameters resource.
3. If supporting filtering, document the filter parameter name (e.g., `triggering-event`) for use with the US Core Patient Data Feed.

### 5.2 For Clients

1. Consult the EHR's documentation for available triggering event codes related to the US Core Patient Data Feed.
2. Use the documented filter parameter to subscribe to specific triggering events within the Patient Data Feed if desired.
3. Parse the `triggering-event` value from the `notification-event` part of received notifications.

## 6. Benefits

1. More detailed understanding of what triggered changes in the FHIR representation of patient data within the US Core Patient Data Feed.
2. Enhanced ability to track and audit specific events within the EHR system as they relate to the Patient Data Feed.
3. Potential for more targeted and efficient processing of Patient Data Feed notifications by clients.
4. Improved insights into EHR workflows and data changes, facilitating future enhancements to FHIR and US Core standards.
5. Opportunity for EHR vendors to showcase the depth and quality of their processes.
6. Clients can choose to leverage or ignore this additional information based on their needs, maintaining compatibility with existing implementations.
7. Use of standardized event codes (like HL7v2 event codes) improves interoperability and understanding across different systems.
