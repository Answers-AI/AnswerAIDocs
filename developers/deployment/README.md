---
description: Learn how to deploy Flowise to the cloud
---

# Deployment

***

# AnswerAI Deployment

## Deploying an Existing Environment

1. Make sure you have the env keys in the root of Flowise
2. Make sure you have the folder for environment under copilot. (if not copy a folder and updaet the manifest with the new name)
to switch to IAS profile you need to run this export AWS_PROFILE=saml
to go back to the other profile export AWS_PROFILE=default

Flowise is designed with a platform-agnostic architecture, ensuring compatibility with a wide range of deployment environments to suit your infrastructure needs.

## Local Machine

To deploy Flowise locally, follow our [Get Started](../../getting-started/) guide.

## Modern Cloud Providers

Modern cloud platforms prioritize automation and focus on developer workflows, simplifying cloud management and ongoing maintenance.&#x20;

This reduces the technical expertise needed, but may limit the level of customization you have over the underlying infrastructure.

* [Elestio](https://elest.io/open-source/flowiseai)
* [Hugging Face](hugging-face.md)
* [Railway](railway.md)
* [Render](render.md)
* [Replit](replit.md)
* [RepoCloud](https://repocloud.io/details/?app\_id=29)
* [Sealos](sealos.md)
* [Zeabur](zeabur.md)

## Established Cloud Providers

Established cloud providers, on the other hand, require a higher level of technical expertise to manage and optimize for your specific needs.&#x20;

This complexity, however, also grants greater flexibility and control over your cloud environment.

* [AWS](aws.md)
* [Azure](azure.md)
* [DigitalOcean](digital-ocean.md)
* [GCP](gcp.md)
* [Kubernetes using Helm](https://artifacthub.io/packages/helm/cowboysysop/flowise)
