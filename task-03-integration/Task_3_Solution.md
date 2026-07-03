# Task 3 Solution: CRM & WhatsApp Integration Design

This document details the integration design connecting OrthoNow’s landing page form to HubSpot CRM and the Karix WhatsApp Business API.

---

### 1. End-to-End Architecture & Middleware Justification
To connect the landing page to HubSpot and Karix, we implement a server-side middleware handler (e.g., a Node.js Serverless Function). Standard client-side connections or basic integrations are discarded in favor of this middleware to address:
*   **API Security**: Prevents raw API credentials (tokens, private keys) from being exposed in public client-side JavaScript.
*   **Centralized Business Logic**: Allows custom formatting (like E.164 phone formatting) and database queue operations to be controlled at a single, maintainable endpoint.
*   **Deduplication Handling**: Bypasses HubSpot’s deduplication limit (which natively merges contacts only by Email, not by Phone). The middleware queries HubSpot's Search API (`POST /crm/v3/objects/contacts/search`) by phone. If a match is found, it calls the Update API (`PATCH /crm/v3/objects/contacts/{id}`) setting Lead Status to 'New Enquiry' and Source to 'Google Ads'. Otherwise, it calls the Create API (`POST /crm/v3/objects/contacts`).
*   **Retry Handling & Scalability**: Caches and processes payloads asynchronously to scale under traffic spikes.
*   **Centralized Monitoring**: Enables unified logs for troubleshooting external API downtimes.

**Trigger Sequence**: 
1. The form submits. The middleware formats the phone number, checks deduplication, and routes the payload to HubSpot.
2. The middleware triggers the Karix API (`POST /v1/message/whatsapp`) to send the verification template.
3. The Google Ads conversion should only fire after the backend successfully confirms that the lead has been accepted for processing. This prevents failed or incomplete submissions from being counted as advertising conversions.

### 2. Single Point of Failure & Fallback
Synchronous API failures or timeouts at HubSpot or Karix represent the primary point of failure, leading to lost leads. 
*Fallback*: We implement a persistent queue (e.g., BullMQ or AWS SQS). The middleware writes lead payloads to the queue and immediately returns a success response. A background worker processes the queue, retrying failed requests up to 5 times using exponential backoff. Failed jobs route to a Dead Letter Queue (DLQ) and trigger a Slack alert.

### 3. SLA & Delivery Monitoring (WhatsApp < 2 Min)
*Risks*: Congestion, invalid numbers, or blocked templates can breach the 2-minute SLA.
*Monitoring*: We track Karix webhooks (`sent`, `delivered`, `failed`). The middleware monitors the delta between submission ($T_0$) and delivery ($T_d$). Alerts (via Opsgenie/Slack) trigger if delivery latency ($T_d - T_0$) exceeds 120 seconds, or if failure rates exceed 2% over a rolling 10-minute window.
