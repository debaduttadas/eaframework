# Cloud Resource Naming Standards

## 1. Purpose

This document defines standard naming conventions for cloud resources used by Integrity Systems. Consistent naming:

- Makes resources easier to discover, manage, and support
- Simplifies automation, monitoring, and cost allocation
- Reduces confusion across teams and environments
- Helps ensure compliance and auditability

These standards apply to all new cloud resources and should be followed whenever existing resources are renamed or refactored.

## 2. Scope

The naming standards in this document apply to:

- All cloud infrastructure resources (e.g. compute, networking, storage, databases, messaging, security)
- All environments (e.g. development, test, staging, production)
- All Infrastructure as Code (IaC) definitions (e.g. AWS CDK, Terraform, CloudFormation)

Where a specific platform has stricter requirements (e.g. allowed characters or maximum length), those requirements MUST be followed in addition to this standard.

## 3. General Naming Principles

### 3.1 Core Principles

All resource names should be:

- **Predictable** – a person can infer purpose and environment from the name
- **Consistent** – follows the same pattern within and across teams
- **Unambiguous** – clearly distinguishes environments, applications, and components
- **Automation-friendly** – uses characters and patterns that work reliably in scripts, APIs, and IaC

### 3.2 Common Rules

Unless the platform requires otherwise:

- Use lowercase letters, numbers, and hyphens (`-`)
- Avoid spaces, underscores (`_`), and special characters
- Use short, widely understood abbreviations
- Do not encode confidential information in names (e.g. credentials, client secrets)

## 4. Naming Pattern

### 4.1 Standard Pattern

The standard pattern for cloud resource names is:

```
<org>-<system>-<component>-<env>-<region>-<suffix>
```

Where:

- **org** – Organisation identifier
- **system** – Business system or application
- **component** – Logical component or resource type
- **env** – Environment
- **region** – Cloud region (where relevant)
- **suffix** – Optional differentiator (e.g. numeric index, AZ, purpose)

Not all segments are required for every resource; when constrained by length or platform rules, segments may be omitted or abbreviated, but the relative order of segments must be preserved.

### 4.2 Standard Abbreviations

**Organisation**
- `is` – Integrity Systems

**Environments**
- `dev` – Development
- `test` – System / integration testing
- `stg` – Staging / pre-production
- `prod` – Production
- `sandbox` – Temporary or experimental

**Example Systems** (illustrative only)
- `nlis` – NLIS system
- `mlis` – MLIS system
- `idp` – Identity platform
- `data` – Shared data platform

**Regions** (AWS examples)
- `apse2` – ap-southeast-2
- `apse4` – ap-southeast-4

If multiple regions per system are used, the short codes must be documented and applied consistently.

## 5. Examples by Resource Type

**Note:** These are examples; actual implementations should be adapted to each platform's constraints.

### 5.1 Compute

*EC2 instances, ECS services, Lambda functions*

**Pattern:**
- EC2 instances: `is-<system>-<role>-<env>-<region>-i<nn>`
- ECS services: `is-<system>-svc-<component>-<env>-<region>`
- Lambda functions: `is-<system>-fn-<component>-<env>`

**Examples:**
- `is-nlis-api-dev-apse2-i01` – NLIS API EC2 instance in dev
- `is-nlis-svc-gateway-prod-apse2` – NLIS gateway ECS service in prod
- `is-data-fn-etl-loader-stg` – Data platform ETL loader Lambda in staging

### 5.2 Storage

*S3 buckets, blobs*

**Pattern:**
- S3 buckets: `is-<system>-<purpose>-<env>-<region>-bucket`

**Examples:**
- `is-nlis-logs-prod-apse2-bucket` – Production logs bucket for NLIS
- `is-data-raw-dev-apse2-bucket` – Raw data bucket for data platform in dev

**Additional Rules:**
- Bucket names must be globally unique; if a collision occurs, append a short numeric or hash suffix, e.g. `-01`
- Do not include PII or secrets in bucket names

### 5.3 Databases

*RDS, DynamoDB, SQL*

**Pattern:**
- RDS instances: `is-<system>-db-<component>-<env>-<region>`
- DynamoDB tables: `is-<system>-tbl-<entity>-<env>`

**Examples:**
- `is-nlis-db-core-prod-apse2` – Core NLIS production RDS instance
- `is-nlis-tbl-devices-prod` – Devices table for NLIS in prod

### 5.4 Networking

*VPC, subnets, security groups, load balancers*

**Pattern:**
- VPC: `is-<system>-vpc-<env>-<region>`
- Subnet: `is-<system>-subnet-<env>-<az>`
- Security group: `is-<system>-sg-<purpose>-<env>`
- Load balancer: `is-<system>-alb-<component>-<env>-<region>`

**Examples:**
- `is-nlis-vpc-prod-apse2`
- `is-nlis-subnet-prod-apse2a`
- `is-nlis-sg-api-prod`
- `is-nlis-alb-public-prod-apse2`

### 5.5 Messaging

*SQS, SNS, EventBridge, Kafka*

**Pattern:**
- Queue: `is-<system>-q-<purpose>-<env>`
- Topic: `is-<system>-topic-<purpose>-<env>`
- Event bus / streams: `is-<system>-bus-<purpose>-<env>`

**Examples:**
- `is-nlis-q-ingest-dev` – Ingestion queue for NLIS dev
- `is-nlis-topic-notify-prod` – Notification topic for NLIS prod

### 5.6 Security and IAM

**Pattern:**
- IAM roles: `is-<system>-role-<workload>-<env>`
- IAM policies: `is-<system>-policy-<purpose>-<env>`
- KMS keys: `is-<system>-kms-<purpose>-<env>-<region>`

**Examples:**
- `is-nlis-role-ecs-task-prod`
- `is-nlis-policy-s3-access-prod`
- `is-data-kms-encrypt-prod-apse2`

### 5.7 Monitoring and Logging

**Pattern:**
- Log groups: `is-<system>-logs-<component>-<env>`
- Metrics / alarms: `is-<system>-alarm-<component>-<env>`

**Examples:**
- `is-nlis-logs-api-prod`
- `is-nlis-alarm-api-latency-prod`

## 6. Tags and Labels

Where the platform supports tags/labels (e.g. AWS tags, Kubernetes labels), names do not replace tags. Both must be used.

### 6.1 Mandatory Tags

At minimum, all taggable resources must include:

- **Name** – Human-readable resource name (following the standards in this document)
- **System** – Business system or application (e.g. nlis)
- **Environment** – dev, test, stg, prod, etc.
- **Owner** – Owning team or cost centre
- **CostCentre** – Code used for cost allocation (where applicable)

**Example:**
```
Name = is-nlis-api-prod-apse2-i01
System = nlis
Environment = prod
Owner = n2r-team
CostCentre = 1234
```

## 7. Handling Constraints and Exceptions

### 7.1 Length and Character Constraints

If a platform restricts length or characters:

- Preserve the leftmost segments first: `is-<system>-<component>-<env>`
- Shorten component or system using clear abbreviations
- Drop region or suffix only if necessary

**Example shortening:**
- Full: `is-nlis-api-gateway-prod-apse2`
- Shortened: `is-nlis-apigw-prod`

### 7.2 Temporary and Experimental Resources

Temporary or experimental resources should still follow the pattern, but may include an additional `tmp` or `exp` segment, and should have a clear lifecycle policy.

**Example:**
- `is-nlis-api-exp-dev-apse2`

These resources should be cleaned up according to environment cleanup practices.

### 7.3 Documenting Approved Exceptions

Any deviation from these standards must:

- Have a documented reason (e.g. vendor-managed naming, legacy system limitations)
- Be recorded in an agreed location (e.g. an "Exceptions" section below or related page)
- Be reviewed periodically to determine if it can be aligned with the standard

## 8. Implementation in Infrastructure as Code

All IaC repositories (e.g. AWS CDK, Terraform) must:

- Implement naming conventions via shared constructs/modules/components
- Avoid hard-coded names scattered throughout code
- Use variables or configuration for env, region, and system segments
- Include automated checks (where feasible) to prevent non-compliant names

**Example approach:**
- Central "naming" utility that takes inputs (system, component, env, region) and generates compliant names
- Reuse the utility across stacks and repositories

## 9. Governance and Compliance

- New projects must adopt these naming standards from inception
- Existing projects should migrate names to this standard when:
  - Significant refactoring is undertaken, or
  - Resources are being recreated for other reasons
- Automated or manual reviews (e.g. architecture reviews, platform audits) should include verification of naming compliance
- Non-compliance should be tracked and remediated as part of normal operational work

## 10. Change Management

- Changes to this standard must be approved via the relevant architecture or platform governance forum
- Material changes should be communicated to impacted teams via agreed channels (e.g. Confluence updates, release notes, Slack/Teams announcements)
- The "Revision History" below must be updated for all changes
