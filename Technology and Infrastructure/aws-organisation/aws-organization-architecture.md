# AWS Organization Architecture

## 1. Purpose

This document defines the **AWS Organization structure** and **multi-account strategy** for **Integritysystems**. It aims to provide a comprehensive blueprint for establishing a secure, scalable, and well-governed foundation for our cloud infrastructure. 

This document serves as a blueprint for stakeholders, IT teams, and decision-makers, detailing the structure, components, and standards of our AWS environment. By following this architecture, we intend to:

* Standardize our AWS adoption
* Improve operational efficiency
* Enhance security posture
* Ensure alignment with business objectives and compliance requirements

This document is a living reference, updated as our cloud strategy evolves, to maintain its relevance and effectiveness in guiding our AWS journey.

---

## 2. Background

ISC has embarked on a cloud transformation journey to leverage the benefits of AWS cloud services. As ISC expands its cloud footprint, it introduces the challenges of managing multiple AWS accounts, enforcing security policies, and maintaining compliance across diverse teams and projects.

There is a need for a standardized and secure foundation to support the growing AWS environment. A well-designed AWS account architecture is essential to provide the necessary cohesion and coherence, facilitating sustainable and continuous growth.

---

## 3. Scope

This architecture covers:

* AWS Organization structure and account hierarchy
* Organizational Units (OUs) design and purpose
* Service Control Policies (SCPs) and governance
* Cross-account access patterns and security
* Billing and cost management structure
* Compliance and audit requirements

---

## 4. Architecture Overview

![AWS Landing Zone Architecture](isc-aws-landingzone-arch.drawio.png)

The AWS Organization architecture is built on **AWS Control Tower** foundation, implementing a multi-account strategy that separates workloads, environments, and security functions into distinct accounts organized within a hierarchical structure.

This architecture addresses five key areas:

1. **Account Vending**: Automated account provisioning through AWS Control Tower Account Factory
2. **Account Structure**: Hierarchical multi-account organization with defined OUs
3. **Security**: Centralized identity management and access controls
4. **Governance & Compliance**: Service Control Policies and compliance monitoring
5. **Centralized Service Management**: Shared services for networking, identity, and logging

### 4.1 Account Vending

Automated account provisioning through AWS Control Tower Account Factory with baseline configurations, approval workflows, and standardized deployment processes for consistent account creation across the organization.

**Implementation**: AWS Control Tower Account Factory

**Integration**: ServiceNow, Jira, or internal portal

**Approval Workflow**: Automated for sandbox, approval required for production

**Account Baseline Configuration**:
- Security: Baseline security controls and monitoring
- Networking: VPC setup and Transit Gateway attachment
- Identity: SSO integration and role provisioning
- Compliance: Config rules and Security Hub enablement
- Cost Management: Budget alerts and cost allocation tags

**Vending Process**:
1. Request Submission via self-service portal or API
2. Validation of business justification and technical requirements
3. Approval (automated or manual based on account type)
4. Provisioning (account creation and baseline deployment)
5. Handover (credentials and documentation delivery)
6. Monitoring (ongoing compliance and cost tracking)

### 4.2 Account Structure

Hierarchical multi-account organization with Management Account at root, Core OU (Network Hub, Security, DevOps, Logging, Log Archive, Audit), and environment-specific OUs (Production, Dev, QA, UAT, Sandbox) segregating functions and environments.

**Management Account (Root)**:
- Purpose: AWS Organization management and billing consolidation
- Responsibilities: Organization-wide policies and SCPs, consolidated billing, root-level security controls
- Access: Highly restricted, break-glass only

**Core OU**:
- Network Hub Account: Centralized networking (Transit Gateway, Direct Connect, VPN, DNS)
- Security Account: Security services and tools
- DevOps Account: CI/CD pipelines and deployment automation
- Logging Account: Centralized logging and monitoring
- Log Archive Account: Long-term log storage (CloudTrail, Config, VPC Flow Logs, GuardDuty)
- Audit Account: Security monitoring and compliance validation (Security Hub, Inspector, Trusted Advisor)
- Access: Shared Services team

**Production OU**:
- Purpose: Live production workloads
- Controls: Strict SCPs, change control, monitoring
- Access: Production support teams only

**Dev OU**:
- Purpose: Development and feature testing environments
- Controls: Moderate SCPs, automated provisioning, cost controls
- Access: Development teams and DevOps

**QA OU**:
- Purpose: Quality assurance and integration testing
- Controls: Moderate SCPs, test data management, automated testing
- Access: QA teams and test automation

**UAT OU**:
- Purpose: User acceptance testing and pre-production validation
- Controls: Production-like SCPs, change control, monitoring
- Access: Business users, UAT teams, and release managers

**Sandbox OU**:
- Purpose: Experimentation and learning with minimal controls
- Controls: Minimal SCPs, cost controls
- Access: Developers and architects

### 4.3 Security

Centralized identity management through AWS SSO, cross-account access patterns using IAM roles, network isolation with hub-and-spoke model, and comprehensive security controls including MFA enforcement and audit logging.

**Identity and Authentication**:
- Primary Method: AWS SSO (Identity Center) for all human access
- Identity Source: Corporate Active Directory or external IdP (SAML/OIDC)
- MFA Enforcement: Required for all console and CLI access
- Session Duration: Time-limited sessions with automatic expiry

**Access Patterns**:
- Human Access: AWS SSO with federated identity
- Service Access: Cross-account IAM roles with trust relationships
- Emergency Access: Break-glass procedures with audit logging
- API Access: Temporary credentials via STS assume role

**Network Integration Principles**:
- Hub-and-Spoke Model: Network account as central hub via Transit Gateway
- Account Isolation: No direct network peering between workload accounts
- Shared Services Exception: Only Shared Services Account accessible for specific services
- Zero Trust: No implicit trust between accounts or environments
- Centralized Connectivity: All external connections through Network account

**Security Controls**:
- Least privilege access principles
- Time-bound access for elevated permissions
- Audit logging for all cross-account access
- Regular access reviews and certification

### 4.4 Governance & Compliance

Service Control Policies (SCPs) enforcing security and operational guardrails at organization, OU, and account levels, with continuous compliance monitoring, automated remediation, and adherence to ISO 27001, SOC 2, and PCI DSS standards.

**Organization-Wide Policies**:
- Deny root user access (except break-glass)
- Enforce encryption in transit and at rest
- Require MFA for console access
- Prevent deletion of CloudTrail logs
- Restrict region usage to approved regions

**Environment-Specific Policies**:
- Production: Deny instance termination without approval, require resource tagging, prevent public S3 buckets, enforce backup policies
- Dev/QA/UAT: Cost controls, resource limits, automated cleanup, test data management
- Sandbox: Spending limits, prevent access to production networks, auto-shutdown of resources

**Compliance Framework**:
- Standards: ISO 27001, SOC 2, PCI DSS (as applicable)
- Monitoring: AWS Config rules and Security Hub
- Reporting: Automated compliance dashboards
- Remediation: Automated where possible, manual escalation

**Account Lifecycle**:
- Provisioning: Automated through Control Tower Account Factory
- Configuration: Baseline security and compliance settings
- Monitoring: Continuous compliance validation
- Decommissioning: Secure account closure process

### 4.5 Centralized Service Management

Core OU providing shared services including centralized networking (Transit Gateway, Direct Connect, VPN), identity federation (AWS SSO), shared infrastructure (Active Directory, DNS), and centralized logging and monitoring capabilities.

**Centralized Networking**:
- Transit Gateway: Hub for all account connectivity
- Direct Connect: On-premises connectivity
- VPN: Secure remote access
- DNS: Centralized DNS with conditional forwarding
- Internet Access: Controlled egress through centralized NAT/proxy

**Identity Federation**:
- AWS SSO (Identity Center): Single sign-on for all accounts
- Identity Providers: Integration with corporate Active Directory or external IdP
- Cross-Account Roles: Service-to-service access patterns

**Shared Infrastructure**:
- Active Directory: Centralized directory services
- DNS: Domain name resolution
- Monitoring Tools: Organization-wide observability

**Centralized Logging & Monitoring**:
- Log Archive Account: Centralized storage for CloudTrail, Config, VPC Flow Logs, GuardDuty findings
- Audit Account: Security Hub for compliance validation and security monitoring
- Retention: Long-term compliance and audit requirements

**Cost Management**:
- Consolidated Billing: All accounts under management account
- Cost Allocation: Tags and account-based tracking
- Budgets: Account and service-level budget alerts
- Optimization: Regular cost reviews, Reserved Instances, Savings Plans

---

## 5. Document Control

| Field | Value |
|-------|-------|
| **Owner** | <Your Name>, Enterprise Architect |
| **Team** | <Your Team Name> |
| **Contact** | <your.email@company.com> |
| **Current Version** | 1.0 |
| **Applies To** | All AWS accounts and services |

## 6. Change Log

| Version | Date | Author | Description |
|---------|------|--------|--------------|
| 1.0 | 2024-01-15 | <Your Name> | Initial version - AWS Organization architecture design |

---

## 7. References

- [Enterprise Architecture Principles](../../README.md)
- [AWS Organizations Best Practices](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_best-practices.html)
- [AWS Control Tower](https://docs.aws.amazon.com/controltower/)
- [AWS Landing Zone](https://aws.amazon.com/solutions/implementations/aws-landing-zone/)