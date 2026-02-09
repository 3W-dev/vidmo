# Vidmo

**Vidmo** is a platform designed to manage and host technical video demos. It combines cloud storage with an automated review and publication workflow, ensuring that every video is vetted before going public.

![Vidmo screenshot](/assets/vidmo.png)

## Core Features

### Hybrid Infrastructure
The architecture leverages **Amazon S3** for storage and **Amazon CloudFront** for global content delivery. The orchestration is handled by **n8n**, making the system compatible with both self-hosted instances and n8n Cloud for a fully serverless ecosystem.

### Privacy & Environment Isolation
Security is enforced via AWS policies and path-based isolation:
* **Origin Access Control (OAC):** The S3 bucket remains private; public traffic is routed exclusively through CloudFront.
* **IP-Restricted Staging:** Direct S3 access is permitted only for a specific IP address parameter, specifically for the `/staging` path used during the preview phase.
* **Versioning:** S3 Bucket Versioning is enabled to prevent data loss and track asset iterations.

### Slack video publish approval process
Publication is governed by an automated n8n workflow that integrates human authorization via Slack:
1.  **Submission:** The user submits a video file, title, and description through an **n8n Form**.
2.  **Staging:** The workflow uploads the file to the IP-restricted `/staging` path in S3.
3.  **Slack Authorization:** A message is sent to a dedicated Slack channel with the video details. The workflow pauses at a **man-in-the-loop** stage until a reviewer clicks the "Approve" button directly within Slack.
4.  **Promotion:** Upon approval, n8n moves the file from `/staging` to `/public` and creates the related HTML file.
5.  **Cleamup:** The uploaded file is deleted from `/staging` path if the approver user clicks on "Decline" button in Slack.

## Technical Stack

* **Infrastructure:** AWS CloudFormation (S3, CloudFront, OAC).
* **Automation:** n8n (Self-hosted or Cloud) for form handling, Slack integration, and S3 orchestration.
* **Interface:** Vanilla HTML5/CSS3/JS with i18n support and [Plyr.io](https://plyr.io/).
* **AI Assistance:** The HTML template was generated with **Gemini 3 Flash**. We strictly avoid the term "vibecoded" as every line of code produced by LLMs is accurately reviewed and controlled by our senior developers before production.

## Quick Deployment

Deploy the AWS infrastructure immediately using the button below:

[![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?templateURL=https://raw.githubusercontent.com/3W-dev/vidmo/refs/heads/main/template.yaml)

**Post-Deployment Setup:**
1. Import the provided workflow JSON into your n8n instance.
2. Configure n8n credentials for both **AWS** and **Slack**.
