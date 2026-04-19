# Amazon Proton (amazon-proton)
AWS Proton is a managed service for platform engineers that helps them publish standardized container and serverless application templates to empower developers. It provides automated infrastructure provisioning and manages deployment pipelines for all your applications, enabling self-service developer workflows with platform-team guardrails.

**URL:** [https://aws.amazon.com/proton/](https://aws.amazon.com/proton/)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Tags:

 - AWS, DevOps, Infrastructure as Code, Platform Engineering, Serverless, Templates, Self-Service, CI/CD

## Timestamps

- **Created:** 2026-03-16
- **Modified:** 2026-04-19

## APIs

### AWS Proton API
The AWS Proton API provides programmatic access to create and manage environment templates, service templates, environments, services, components, and deployment pipelines for standardized application deployments with automated infrastructure provisioning.

**Human URL:** [https://aws.amazon.com/proton/](https://aws.amazon.com/proton/)

#### Tags:

 - DevOps, Infrastructure as Code, Platform Engineering, Templates, Environments, Services

#### Properties

- [Documentation](https://docs.aws.amazon.com/proton/latest/APIReference/Welcome.html)
- [OpenAPI](openapi/amazon-proton-openapi-original.yaml)
- [GettingStarted](https://aws.amazon.com/proton/getting-started/)
- [Pricing](https://aws.amazon.com/proton/pricing/)
- [FAQ](https://aws.amazon.com/proton/faqs/)
- [Authentication](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html)
- [RateLimits](https://docs.aws.amazon.com/proton/latest/userguide/ag-limits.html)

## Common Properties

- [Portal](https://aws.amazon.com/proton/)
- [Documentation](https://docs.aws.amazon.com/proton/)
- [TermsOfService](https://aws.amazon.com/service-terms/)
- [PrivacyPolicy](https://aws.amazon.com/privacy/)
- [Support](https://aws.amazon.com/premiumsupport/)
- [Blog](https://aws.amazon.com/blogs/containers/tag/aws-proton/)
- [GitHubOrganization](https://github.com/aws)
- [Console](https://console.aws.amazon.com/proton/)
- [SignUp](https://portal.aws.amazon.com/billing/signup)
- [StatusPage](https://health.aws.amazon.com/health/status)
- [SpectralRules](rules/amazon-proton-spectral-rules.yml)
- [NaftikoCapability](capabilities/platform-engineering.yaml)
- [Vocabulary](vocabulary/amazon-proton-vocabulary.yaml)

## Features

| Name | Description |
|------|-------------|
| Environment Templates | Create standardized environment templates with infrastructure-as-code definitions for platform engineers to publish. |
| Service Templates | Define reusable service templates that developers can use for self-service application deployments. |
| Automated Provisioning | Automatically provision and manage infrastructure for environments and services using CloudFormation or Terraform. |
| CI/CD Pipeline Management | Manage deployment pipelines for services with automatic updates when templates change. |
| Template Versioning | Version control environment and service templates with major and minor version support. |
| Components | Create infrastructure components that can be shared across services and environments. |
| Repository Connections | Connect to GitHub and Bitbucket repositories for template source and service pipeline definitions. |
| Drift Detection | Detect and remediate configuration drift in deployed environments and services. |

## Use Cases

| Name | Description |
|------|-------------|
| Developer Self-Service | Enable developers to deploy containerized and serverless applications without deep infrastructure knowledge. |
| Platform Standardization | Enforce organizational standards for infrastructure, security, and compliance through templates. |
| Multi-Account Deployment | Deploy standardized environments and services across multiple AWS accounts. |
| Microservices Orchestration | Manage the infrastructure for complex microservices architectures with consistent templates. |
| Serverless Workflows | Deploy Lambda-based serverless applications with standardized infrastructure patterns. |

## Integrations

| Name | Description |
|------|-------------|
| AWS CloudFormation | Use CloudFormation as the IaC engine for provisioning environment and service infrastructure. |
| HashiCorp Terraform | Use Terraform as an alternative IaC engine for environment and service provisioning. |
| AWS CodePipeline | Automatically create CI/CD pipelines for services using CodePipeline. |
| GitHub | Connect GitHub repositories as sources for environment templates and service pipeline definitions. |
| AWS CodeCommit | Use CodeCommit repositories for template and configuration management. |

## Artifacts

Machine-readable API specifications organized by format.

### OpenAPI

- [Amazon Proton OpenAPI (full - 84 operations)](openapi/amazon-proton-openapi-original.yaml)

### JSON Schema

230 schema files covering all Amazon Proton API resource types including environment templates, service templates, environments, services, and components.

### JSON Structure

230 JSON Structure files converted from JSON Schema with strict typing.

### JSON-LD

- [Amazon Proton Context](json-ld/amazon-proton-context.jsonld)

### Examples

73 example JSON files generated from JSON Schema definitions.

## Capabilities

Naftiko capabilities organized as shared per-API definitions composed into customer-facing workflows.

### Shared Per-API Definitions

- [Amazon Proton API](capabilities/shared/amazon-proton.yaml) — 8 operations for platform engineering and deployment

### Workflow Capabilities

| Workflow | APIs Combined | Tools | Persona |
|----------|--------------|-------|---------|
| [Platform Engineering](capabilities/platform-engineering.yaml) | Amazon Proton API | 8 | Platform Engineer, Application Developer |

## Vocabulary

- [Amazon Proton Vocabulary](vocabulary/amazon-proton-vocabulary.yaml) — Unified taxonomy mapping resources, actions, workflows, and personas across operational (OpenAPI) and capability (Naftiko) dimensions

## Rules

- [Amazon Proton Spectral Rules](rules/amazon-proton-spectral-rules.yml) — 17 rules across 8 categories enforcing Amazon Proton API conventions

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
