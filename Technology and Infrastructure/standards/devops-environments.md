# DevOps Environments

## Purpose

This document defines the standard AWS account structure and environment strategy for the enterprise SDLC (Software Development Life Cycle). It establishes clear guidelines for environment usage, access controls, and deployment practices to ensure consistency, security, and compliance across all solution teams.

## Environment Overview

The enterprise uses a multi-account AWS strategy with five distinct environments, each serving a specific purpose in the software development and deployment lifecycle. This approach provides:

- **Account-level isolation** between environments
- **Clear separation of concerns** for development, testing, and production workloads
- **Progressive deployment path** from experimentation to production
- **Consistent security and governance** controls across all environments
- **Audit trail and traceability** for all changes

## AWS Account Structure

| Environment | AWS Account | Purpose | Console Write Access | Deployment Type |
|------------|-------------|---------|---------------------|----------------|
| Sandbox | Sandbox Account | Individual testing & experimentation | Yes (DevOps teams) | Manual |
| Dev | Dev Account | Integration testing | No (Bitbucket pipeline only) | Automated |
| QA | QA Account | Quality assurance & bug fixes | No (Bitbucket pipeline only) | Automated |
| UAT | UAT Account | User acceptance testing | No (Bitbucket pipeline only) | Automated with approval |
| Production | Prod Account | Live production workloads | No (Bitbucket pipeline only) | Automated with approval |

## Environment Descriptions

### Sandbox

**Purpose**: Individual developer experimentation and testing environment

**Characteristics**:
- Isolated environment for testing infrastructure-as-code (IaC) and CDK code
- Allows rapid iteration and experimentation without impacting other environments
- Used for validating code changes before formal integration
- No service level agreements or uptime guarantees

**Access**:
- Console write access enabled for DevOps teams
- Manual deployments allowed (console or CLI)
- No branch protection or approval gates

**Use Cases**:
- Testing new IaC/CDK code on feature branches
- Experimenting with AWS services and configurations
- Validating infrastructure changes before Pull Requests
- Learning and skill development

**Deployment Method**: Manual deployment from sandbox/* or feature/* branches

**Data**: Non-production data only; no sensitive or production data allowed

### Dev

**Purpose**: Integration environment for ongoing development work

**Characteristics**:
- First automated deployment environment in the pipeline
- Integration point for all feature branches
- Continuous deployment from develop branch
- Shared environment for all development teams

**Access**:
- No console write access (Bitbucket pipeline only)
- All changes deployed via automated pipelines
- Read-only console access for troubleshooting

**Use Cases**:
- Integration testing of merged feature branches
- Automated testing (unit, integration, API tests)
- Early detection of integration issues
- Continuous validation of develop branch

**Deployment Method**: Automated deployment triggered by merges to develop branch

**Data**: Non-production test data; refreshed regularly from sanitized production data

### QA

**Purpose**: Quality assurance and systematic testing environment

**Characteristics**:
- Dedicated environment for QA team testing
- Stable environment for test execution
- Used for bug fix validation
- Mirrors production configuration

**Access**:
- No console write access (Bitbucket pipeline only)
- All changes deployed via automated pipelines
- Read-only console access for QA teams

**Use Cases**:
- Functional testing of release candidates
- Regression testing
- Bug fix verification
- Performance and load testing
- Security testing

**Deployment Method**: Automated deployment from release/* branches

**Data**: Non-production test data; representative of production scenarios

### UAT

**Purpose**: User acceptance testing and pre-production validation

**Characteristics**:
- Production-like environment for final validation
- Used by business users for acceptance testing
- Last validation gate before production
- Identical configuration to production

**Access**:
- No console write access (Bitbucket pipeline only)
- All changes deployed via automated pipelines
- Read-only console access for support teams

**Use Cases**:
- Business user acceptance testing
- End-to-end workflow validation
- Production readiness verification
- Final stakeholder sign-off

**Deployment Method**: Automated deployment from release/* branches after QA approval

**Data**: Non-production data; closely mirrors production data structure and volume

### Production

**Purpose**: Live production environment serving end users

**Characteristics**:
- Production workloads and live customer data
- Highest security and compliance controls
- 24/7 monitoring and support
- Strict change control and approval gates

**Access**:
- No console write access (Bitbucket pipeline only)
- All changes deployed via automated pipelines with approval
- Read-only console access for incident response

**Use Cases**:
- Live customer-facing applications
- Production workloads and services
- Business-critical operations

**Deployment Method**: Automated deployment from release/* branches after UAT approval and manual approval gate

**Data**: Live production data with full security and compliance controls

## Access Control Policy

### Least Privilege Principle

Console write access is restricted on Dev, QA, UAT, and Production accounts to maintain environment consistency and enforce infrastructure-as-code practices.

### Rationale

- **Prevents configuration drift** from manual changes
- **Ensures all changes are tracked and auditable** through version control
- **Maintains environment consistency and reproducibility**
- **Enforces code review and approval processes**
- **Provides complete audit trail** for compliance
- **Reduces security risks** from unauthorized changes

### Access Levels by Environment

**Sandbox Account**:
- Console write access: Yes (DevOps teams)
- Purpose: Enable testing and experimentation
- Justification: Allows rapid iteration without impacting controlled environments

**Dev, QA, UAT, Production Accounts**:
- Console write access: No (Bitbucket pipeline only)
- Purpose: Enforce infrastructure-as-code and maintain consistency
- Justification: All changes must be code-based, reviewed, and auditable

### Read-Only Access

All environments provide read-only console access for:
- Troubleshooting and debugging
- Monitoring and observability
- Incident response
- Audit and compliance verification

## Compliance & Governance Requirements

### All Teams Must

1. **Use Sandbox for experimentation**: All IaC/CDK code testing must occur in Sandbox before Pull Requests
2. **Deploy via Bitbucket pipelines**: Dev, QA, UAT, and Production accept only pipeline deployments
3. **Follow branch strategy**: Use defined branch naming conventions and merge processes
4. **Obtain required approvals**: Production deployments require explicit approval from Technical Lead and Solution Architect
5. **Document all changes**: Commit messages and Pull Requests must clearly describe changes
6. **Maintain environment parity**: Infrastructure code must work consistently across all environments
7. **No manual changes**: Manual console changes prohibited in Dev, QA, UAT, and Production
8. **Implement rollback plans**: All production changes must have documented rollback procedures

### Security Requirements

- All environments must implement security controls defined in enterprise security standards
- Secrets and credentials managed via AWS Secrets Manager or Parameter Store
- Network isolation enforced via VPCs and security groups
- Encryption at rest and in transit for all sensitive data
- IAM roles follow least privilege principle
- MFA required for all human access

### Audit and Compliance

- All deployments logged and tracked via Bitbucket pipeline execution logs
- CloudTrail enabled on all accounts for API activity tracking
- Regular compliance audits of environment configurations
- Automated compliance checks in deployment pipelines
- Quarterly access reviews for all environments

## Support & Exceptions

For questions or exception requests, contact:
- **DevOps Team**: [Contact Details]
- **Enterprise Architecture Team**: [Contact Details]

Exception requests require:
- Documented business justification
- Risk assessment
- Approval from Enterprise Architecture team
- Time-bound remediation plan (if temporary exception)

## Related Documents

- [DevOps Release Process](devops-release-process.md) - Detailed CI/CD workflow, branch strategy, and deployment processes
