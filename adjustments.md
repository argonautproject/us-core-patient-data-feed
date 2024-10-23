# Subscription Adjustment Mechanism

## Overview

This specification defines a mechanism for servers to communicate necessary adjustments to subscription requests that cannot be fulfilled as submitted. It uses an OperationOutcome to provide detailed information about the adjustments, allowing clients to modify and resubmit their requests.

## Mechanism

1. When a client submits a subscription request that the server cannot fulfill as-is, the server responds with an HTTP 422 Unprocessable Entity status and an OperationOutcome.
2. The OperationOutcome contains an error issue with a specific code to indicate a subscription adjustment scenario, and an extension detailing the original criteria, adjusted criteria, and optionally, a human-readable explanation of the adjustments.
3. Clients can use this information to decide whether to accept the adjusted subscription, modify their request, or abandon the subscription attempt.

## OperationOutcome Structure

The OperationOutcome includes the following key elements:

1. An `issue` element with:
   - `severity`: "error"
   - `code`: "not-supported"
   - `details`: Contains a coding that identifies this as a subscription adjustment scenario

2. A custom extension: `http://hl7.org/fhir/us/core/StructureDefinition/us-core-subscription-adjustment`

### Subscription Adjustment Code

To recognize a subscription adjustment scenario, clients should look for the following code in the `issue.details.coding` element:

```json
{
  "system": "http://hl7.org/fhir/us/core/CodeSystem/us-core-operation-outcome-codes",
  "code": "subscription-adjusted"
}
```

This specific code indicates that the OperationOutcome contains information about necessary subscription adjustments.

### Extension Structure

The `us-core-subscription-adjustment` extension can repeat and contains the following sub-extensions:
1. `original-criteria` (1..1): The criteria as submitted by the client
2. `adjusted-criteria` (0..*): The adjusted criteria that the server can support
3. `human-explanation` (0..1): A clear, human-readable explanation of the adjustments (optional)

Cardinality notes:
- The entire `us-core-subscription-adjustment` extension can repeat (0..*) to allow for multiple sets of adjustments.
- One `original-criteria` can correspond to zero to many `adjusted-criteria`.
- The `human-explanation` is optional and may not be present in all cases.

**Important**: If an `original-criteria` has no corresponding `adjusted-criteria`, it means that the server does not support that resource type or criteria at all.

## Example

### Request

```http
POST /fhir/Subscription HTTP/1.1
Host: example.com
Content-Type: application/fhir+json

{
  "resourceType": "Subscription",
  "status": "requested",
  "reason": "Patient monitoring",
  "criteria": "http://hl7.org/fhir/us/core/SubscriptionTopic/patient-data-feed",
  "_criteria": {
    "extension": [
      {
        "url": "http://hl7.org/fhir/uv/subscriptions-backport/StructureDefinition/backport-filter-criteria",
        "valueString": "Encounter?patient=123&trigger=create,update,delete"
      },
      {
        "url": "http://hl7.org/fhir/uv/subscriptions-backport/StructureDefinition/backport-filter-criteria",
        "valueString": "Observation?patient=123&category=laboratory,vital-signs"
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
    "payload": "application/fhir+json"
  }
}
```

### Response

```http
HTTP/1.1 422 Unprocessable Entity
Content-Type: application/fhir+json

{
  "resourceType": "OperationOutcome",
  "id": "subscription-adjustment",
  "issue": [
    {
      "severity": "error",
      "code": "not-supported",
      "details": {
        "coding": [
          {
            "system": "http://hl7.org/fhir/us/core/CodeSystem/us-core-operation-outcome-codes",
            "code": "subscription-adjusted"
          }
        ],
        "text": "The subscription request cannot be processed as submitted and requires adjustment."
      },
      "diagnostics": "The server does not support all requested resources and filters. Adjusted versions have been proposed.",
      "expression": ["Subscription"]
    }
  ],
  "extension": [
    {
      "url": "http://hl7.org/fhir/us/core/StructureDefinition/us-core-subscription-adjustment",
      "extension": [
        {
          "url": "original-criteria",
          "valueString": "Encounter?patient=123&trigger=create,update,delete"
        },
        {
          "url": "adjusted-criteria",
          "valueString": "Encounter?patient=123&trigger=create,update"
        },
        {
          "url": "human-explanation",
          "valueString": "The server does not support 'delete' trigger for Encounters. This has been removed from the adjusted criteria."
        }
      ]
    },
    {
      "url": "http://hl7.org/fhir/us/core/StructureDefinition/us-core-subscription-adjustment",
      "extension": [
        {
          "url": "original-criteria",
          "valueString": "Observation?patient=123&category=laboratory,vital-signs"
        },
        {
          "url": "adjusted-criteria",
          "valueString": "Observation?patient=123&category=laboratory"
        },
        {
          "url": "adjusted-criteria",
          "valueString": "Observation?patient=123&category=vital-signs"
        },
        {
          "url": "human-explanation",
          "valueString": "The server supports laboratory and vital-signs categories as separate criteria."
        }
      ]
    },
    {
      "url": "http://hl7.org/fhir/us/core/StructureDefinition/us-core-subscription-adjustment",
      "extension": [
        {
          "url": "original-criteria",
          "valueString": "DiagnosticReport?patient=123&category=LAB"
        },
        {
          "url": "human-explanation",
          "valueString": "The server does not support subscriptions for DiagnosticReport resources."
        }
      ]
    }
  ]
}
```

In this example:
- The Encounter criteria is adjusted to remove the unsupported 'delete' trigger.
- The Observation criteria is split into two separate criteria for laboratory and vital-signs categories.
- The DiagnosticReport criteria has no `adjusted-criteria`, indicating that the server does not support subscriptions for DiagnosticReport resources at all.

## Client Algorithm

Clients can use the following simplified algorithm to process the OperationOutcome and adjust their subscription request:

1. Submit the initial subscription request.
2. If the response is a 422 status with an OperationOutcome:
    1. Check for the `subscription-adjusted` code in the `issue.details.coding`.
    1. If present:
        1. Start with the set of all initially requested `backport-filter-criteria`.
        1. Remove any criteria that appear in the OperationOutcome's `original-criteria`.
        1. Add any criteria that appear in the OperationOutcome's `adjusted-criteria`.
4. Create a new Subscription resource using the resulting set of criteria.
5. Submit a new POST request with the modified Subscription resource.
