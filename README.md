# OrthoNow - MarTech & Web Developer Assignment

This repository contains the completed solutions for the **Developer - Position 1 (Client Web + Martech)** assignment for the client **OrthoNow** (a chain of 9 orthopaedic clinics).

---

## 📂 Repository Directory Map

The assignment is divided into three distinct tasks, each organized in its respective directory:

### 1. [Task 01: GTM Event Schema & GA4 Configuration](./task-01-gtm-schema/Task_1_Solution.md)
*   **GTM Event Schema Table**: Full tracking setup for clinic location pageviews, appointment forms, CTAs (calls, WhatsApp), patient guide downloads, and blog scroll depth.
*   **Funnel Tracking Explanation**: Clear GTM firing rules, custom triggers, and dataLayer events for each step of the booking process.
*   **Anonymized dataLayer Snippets**: Production-level JavaScript triggers designed to bypass PII transmission rules while retaining campaign and scheduling metadata.
*   **GTM Tagging Setup**: Configuration details for tags, user-defined variables, and triggers.
*   **GA4 Funnel Explorer Setup**: Guide for setting up a closed funnel and calculating abandonment rates.
*   **Google Ads Optimization Strategy**: Selection and justification of primary vs. secondary conversions for Smart Bidding.

### 2. [Task 02: Landing Page Build](./task-02-landing-page/index.html)
*   **Self-Contained File**: A single responsive HTML file with inlined Vanilla CSS and Vanilla JS.
*   **High-Conversion Design**: Copy targeting Bengaluru desk workers experiencing chronic back/knee pain.
*   **Form Validation**: Validates name and phone inputs (conforming to standard Indian mobile structures).
*   **dataLayer Trigger**: Pushes the `consultation_form_submitted` payload dynamically on successful submission without page reload.
*   **Performance Optimization**: Switched to system fonts and removed heavy CSS filters to achieve a **100/100 Mobile PageSpeed score**.
*   **PageSpeed Screenshot**: Verify the Mobile Lighthouse performance score screenshot [here](./task-02-landing-page/pagespeed_screenshot.png).

### 3. [Task 03: HubSpot CRM & WhatsApp Integration Design](./task-03-integration/Task_3_Solution.md)
*   **End-to-End System Schema**: Flowchart mapping how the serverless middleware orchestrates HubSpot and Karix APIs.
*   **HubSpot Deduplication Solution**: Outlines the workaround for HubSpot's limitation of deduplicating only by email by performing a pre-submit search by phone.
*   **API Failure Fallback**: Implements a persistent asynchronous queue (SQS/Redis) with exponential retry backoff.
*   **WhatsApp SLA Monitoring**: Tracks Karix webhook logs ($T_0$ vs $T_d$) to verify deliveries are under 120 seconds.
