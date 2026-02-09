# Vidmo

**Vidmo** is a serverless **micro-platform** designed to manage and host technical video demos. It combines cloud storage with an automated review and publication workflow, ensuring that every video is vetted before going public.

![Vidmo screenshot](/assets/vidmo.png)

## User Privacy
Vidmo is **tracker-free by design**. We do not use cookies, analytics, or third-party pixels. No user behavior or personal data is collected or stored, ensuring a direct and private connection between the viewer and the content.

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
3.  **Slack Authorization:** A message is sent to a dedicated Slack channel. The workflow pauses at a **man-in-the-loop** stage until a reviewer approves the content directly within Slack.
4.  **Promotion:** Upon approval, n8n moves the file from `/staging` to `/public` and creates the related HTML file.
5.  **Cleamup:** The uploaded file is deleted from `/staging` path if the approver user clicks on "Decline" button in Slack.


## Technical Stack

* **Infrastructure:** AWS CloudFormation (S3, CloudFront, OAC).
* **Automation:** n8n (Self-hosted or Cloud) for form handling, Slack integration, and S3 orchestration.
* **Interface:** Vanilla HTML5/CSS3/JS with i18n support and [Plyr.io](https://plyr.io/).
* **AI Assistance:** The HTML template was generated with **Gemini 3 Flash**. Every line of code produced by LLMs is accurately reviewed and controlled by our developers before production.

## Deployment

To deploy the infrastructure:
1. Download the `template.yaml` file from this repository.
2. Deploy the stack using the [AWS CloudFormation Console](https://console.aws.amazon.com/cloudformation/home) or the **AWS CLI**.
3. Provide the required parameters before running the stack creation.
4. Import the workflow JSON into your n8n instance and configure your **AWS** and **Slack** credentials.
