# Acme Corp Customer Support Voice Agent

## Project Overview

This project is an end-to-end AI Customer Support Voice Agent built for the Acme Corp customer support capstone assessment. The solution uses **Retell AI** for the voice agent and **n8n** webhook workflows for backend automation and mock customer-support operations.

The agent is designed to handle common customer-support conversations while enforcing identity verification before account-specific assistance, creating support tickets for unresolved issues, escalating complex requests to a human-support queue, and closing calls professionally.

## Key Features

- **Voice-based customer support** using Retell AI
- **FAQ / Knowledge Base** for approved support information
- **Caller identity verification** using `verify_identity`
- **Support ticket creation** using `create_ticket`
- **Human escalation** using `escalate_to_human`
- **Authentication gating** for account-specific information
- **Structured issue-detail collection** including identity, issue, affected product/service, and urgency
- **Professional call closure** after resolution or escalation
- **n8n webhook backend** for mock/automated tool execution
- **End-to-end call-flow testing** covering FAQ, verification, ticket creation, escalation, and unverified-caller handling

## Architecture

```text
Caller
  |
  v
Retell AI Voice Agent
  |
  +--> FAQ Knowledge Base
  |
  +--> verify_identity --> n8n Webhook
  |
  +--> create_ticket   --> n8n Webhook
  |
  +--> escalate_to_human --> n8n Webhook
  |
  v
Professional Call Closure
```

## Core Tool Functions

### `verify_identity`

**Inputs:**
- `name`
- `account_reference`

**Returns:**
- `verified`
- `customer_id`

Account-specific information must not be disclosed unless verification succeeds.

### `create_ticket`

**Inputs:**
- `customer_id`
- `issue_summary`

**Returns:**
- `status`
- `ticket_id`

The issue summary contains the relevant customer identity, issue description, affected product/service, urgency, and other details collected during the call.

### `escalate_to_human`

**Inputs:**
- `customer_id`
- `issue_summary`
- `reason`

**Returns:**
- `status`
- `eta`

The agent reports the actual escalation result and does not claim that a live transfer occurred when the backend only records an escalation/callback.

## Approved FAQ Knowledge

The agent uses the approved Acme Corp knowledge base for common questions, including:

- Support hours: Monday–Friday, 9 AM–6 PM EST
- Password reset: `acme.com/reset` and the emailed reset link
- Order status: account-specific information is available only after successful identity verification
- Damaged orders: collect the required issue details and create a support ticket
- Contact support: use the Acme Corp customer support phone line

## End-to-End Test Flow

1. **Greeting & FAQ** — Ask for customer support hours.
2. **Ticket request** — Ask to open a ticket for an account problem.
3. **Identity verification** — Provide the seeded test identity and confirm it.
4. **Ticket creation** — Provide the issue, affected service/product, and urgency; confirm ticket creation.
5. **Human escalation** — Request a human agent and confirm the escalation result and ETA.
6. **Call closure** — Confirm there are no further questions and close the call professionally.

## Example Test Identity

```text
Name: John Smith
Account Reference: ACME-9012
```

This identity is used for the end-to-end verification test and should be present in the n8n mock customer data.

## Project Technologies

- **Retell AI** — Voice AI agent and conversation flow
- **n8n** — Webhook-based backend automation and mock integrations
- **Knowledge Base** — Approved Acme Corp FAQ content
- **GitHub** — Source and project documentation

## Assessment / LMS Submission Description

> Built an end-to-end AI Customer Support Voice Agent for Acme Corp using Retell AI and n8n. The project implements a professional support persona, approved FAQ knowledge base, caller identity verification, support ticket creation, human escalation, authentication gating, structured issue-detail capture, and professional call closure. The three backend functions (`verify_identity`, `create_ticket`, and `escalate_to_human`) are connected through n8n webhook workflows with mock responses for testing. The solution was tested through an end-to-end voice call flow covering FAQ answering, identity verification, ticket creation, human escalation, and unverified-caller security handling. A Loom demonstration and configuration evidence are provided as part of the assessment submission.

## Notes & Limitations

- The project uses n8n mock/backend workflows for assessment testing rather than a production CRM or helpdesk platform.
- Account-specific information is protected behind successful identity verification.
- The escalation workflow records/escalates the request; it does not claim a live human transfer unless such a capability is actually available.
- Production deployment would require connecting the workflows to real customer records and a real ticketing/helpdesk system.

## Demo

A Loom demonstration of the configured voice-agent flow is included in the LMS submission.

## Assessment Deliverables

- Retell AI agent configuration
- Conversation flow and system prompt
- Knowledge base configuration
- `verify_identity` tool configuration
- `create_ticket` tool configuration
- `escalate_to_human` tool configuration
- n8n webhook workflows
- End-to-end test demonstration
- Loom video and supporting configuration evidence
