# DevOps Release Process

## Purpose
The purpose of this document is to establish a multi-account DevOps strategy for releasing infrastructure and application code. All code deployments must follow this process to ensure consistency, quality, and compliance across AWS environments.

## Context
This multi-account DevOps release strategy addresses critical enterprise requirements:

1. **Keep Accounts in Consistent State**: Enforces code-based deployments through standardized pipelines, eliminating configuration drift from manual changes

2. **Ensure Auto Documentation and Tracking**: Provides automatic documentation and tracking of all changes introduced by new features and implementations through version control, pipeline logs, and audit trails

3. **Centralized Governance, Decentralized Delivery**: Centrally defined guardrails (branch protection, approval gates) with team autonomy for delivery within approved boundaries

4. **Standardized Deployment and Release Mechanism**: Consistent CI/CD pipeline patterns across all teams and projects for predictable, repeatable deployments

5. **Ensure Security by Design**: Built-in approval gates, automated security scanning in pipelines, and least privilege access controls

6. **Audit Trail and Visibility of Deployment**: Complete traceability from code commit to production deployment with pipeline execution logs and automated audit trails

### Alignment with Architecture Principles

This DevOps release process directly supports and achieves the following enterprise architecture principles:

- **Automation by Default**: All deployments automated through CI/CD pipelines; manual changes in managed environments prohibited
- **Account-Level Isolation**: Separate accounts for each SDLC environment (Sandbox, Dev, QA, UAT, Production) with controlled promotion paths
- **Environment Parity**: Consistent architecture patterns across all environments with promotion through pipelines, not redeployment
- **Centralised Governance, Decentralised Delivery**: Guardrails centrally defined and enforced; teams self-service within approved boundaries
- **Security by Design**: Security controls built into platform and enabled by default through pipeline automation
- **Observability is Mandatory**: Pipeline execution logs, audit trails, and deployment tracking provide full visibility

## Applicability
This standard applies to:
- All solution teams deploying code to AWS environments
- All projects using the enterprise SDLC environments (Sandbox, Dev, QA, UAT, Production)
- Both new projects and existing projects transitioning to the standard

## When to Use
- When deploying any code changes to AWS environments
- When setting up CI/CD pipelines for new projects
- When onboarding new team members to the deployment process
- When planning release schedules and deployment windows

## Scope
Applies to all code deployments including:
- Application code (Java, Python, Node.js, etc.)
- Infrastructure-as-code (Terraform, CloudFormation, CDK)
- Configuration management code
- Automation scripts
- Database scripts
- Operational automation code

## Overview
The release process follows a progressive deployment model across five standard SDLC environments (Sandbox, Dev, QA, UAT, and Production) with automated CI/CD pipelines and approval gates. This aligns with our enterprise SDLC standards and defines how to work with different environments and release code.

## AWS Account Structure

| Environment | AWS Account | Purpose | Deployment Type |
|------------|-------------|---------|----------------|
| Sandbox | Sandbox Account | Individual testing & experimentation | Manual |
| Dev | Dev Account | Integration testing | Automated |
| QA | QA Account | Quality assurance & bug fixes | Automated |
| UAT | UAT Account | User acceptance testing | Automated with approval |
| Production | Prod Account | Live production workloads | Automated with approval |

## Branch Strategy & Deployment Flow

### 1. sandbox/* Branch
- **Purpose**: Individual developer experimentation and testing
- **Target Environment**: Sandbox Account
- **When to Use**: Testing changes before formal integration
- **Deployment Process**: 
  1. Create sandbox/* branch
  2. Develop code
  3. Manually deploy to Sandbox for validation
- **Deployment Method**: Manual, developer-initiated
- **Approval Required**: No

### 2. feature/* Branch
- **Purpose**: New feature development
- **Target Environment**: Sandbox Account
- **When to Use**: Developing new features or capabilities
- **Deployment Process**:
  1. Branch from develop
  2. Develop code
  3. Test locally
  4. Manual deploy to Sandbox for validation
- **Deployment Method**: Manual
- **Approval Required**: No
- **Merge Target**: develop (via Merge Request)

### 3. develop Branch
- **Purpose**: Integration branch for ongoing development
- **Target Environment**: Dev Account
- **When to Use**: Main integration point for all feature work
- **Deployment Process**:
  1. Accept Merge Request from feature/* branches
  2. Automated pipeline triggers:
     - Code validation and linting
     - Security scanning
     - Build artifacts
  3. Automated deployment to Dev environment
- **Deployment Method**: Automated CI/CD
- **Approval Required**: Merge Request approval only
- **Branch Protection**: Yes

### 4. bugfix/* Branch
- **Purpose**: Fix issues identified in QA/UAT
- **Target Environment**: QA Account
- **When to Use**: Addressing defects found during testing phases
- **Deployment Process**:
  1. Branch from develop or release/*
  2. Implement fix
  3. Submit Merge Request
  4. Automated pipeline triggers build and validation
  5. Automated deployment to QA for verification
- **Deployment Method**: Automated
- **Approval Required**: Merge Request approval
- **Merge Target**: Source branch (develop or release/*)

### 5. release/* Branch
- **Purpose**: Release candidate preparation and production deployment
- **Target Environment**: UAT Account → Prod Account
- **When to Use**: Preparing changes for production release
- **Deployment Process**:
  1. Branch from develop
  2. Automated deployment to UAT environment
  3. UAT validation and testing
  4. **Manual approval gate** (required)
  5. Automated deployment to Production
  6. Merge to main branch
  7. Merge back to develop
- **Deployment Method**: Automated with manual approval gate
- **Approval Required**: Yes (before Production deployment)
- **Approvers**: Technical Lead, Solution Architect
- **Merge Targets**: main and develop

### 6. hotfix/* Branch
- **Purpose**: Critical production fixes
- **Target Environment**: All environments (expedited path)
- **When to Use**: Emergency fixes for production issues
- **Deployment Process**:
  1. Branch from main
  2. Implement critical fix
  3. Submit Merge Request for expedited review
  4. Automated build and validation
  5. Deploy through all environments (accelerated)
  6. Merge to main after validation
  7. Merge back to develop
- **Deployment Method**: Expedited automated pipeline
- **Approval Required**: Yes (expedited approval process)
- **Approvers**: Technical Lead (minimum)
- **SLA**: 4 hours maximum

### 7. main Branch
- **Purpose**: Production-ready code (source of truth)
- **Target Environment**: Production Account
- **When to Use**: Never directly - only receives merges
- **Deployment Process**: N/A - receives merges only
- **Deployment Method**: Only receives merges from release/* and hotfix/*
- **Approval Required**: N/A
- **Branch Protection**: Yes (no direct commits allowed)

## Environment Flow

```
Sandbox → Dev → QA → UAT → Production
```

## Standard Deployment Workflow

### Normal Release Cycle
```
1. feature/* → Manual deploy to Sandbox → Test
2. feature/* → Merge Request → develop
3. develop → Auto deploy to Dev → Validate
4. bugfix/* (if needed) → Auto deploy to QA → Test
5. release/* → Auto deploy to UAT → UAT Testing
6. release/* → Approval Gate → Auto deploy to Production
7. release/* → Merge to main and develop
```

### Hotfix Cycle
```
1. hotfix/* → Branch from main
2. hotfix/* → Expedited review and approval
3. hotfix/* → Deploy through all environments (accelerated)
4. hotfix/* → Merge to main and develop
```

## Compliance Requirements

### All Solution Teams Must:
1. **Follow branch naming conventions** exactly as specified
2. **Use Merge Requests** for all code integration (no direct commits to protected branches)
3. **Obtain approvals** before production deployments
4. **Document changes** in commit messages and Merge Requests
5. **Test in Sandbox** before submitting Merge Requests
6. **Validate deployments** at each environment stage
7. **Rollback plan** must be documented for production changes

### Pipeline Automation
- All pipelines include: validation, security scanning, linting, and deployment
- Failed pipeline checks block deployment progression
- Manual approval required for UAT → Production promotion

### Branch Protection Rules
- **main**: No direct commits, requires release/* or hotfix/* merge
- **develop**: No direct commits, requires Merge Request approval
- All other branches: Follow team code review standards

## Key Principles

- **Progressive Deployment**: Changes flow through SDLC environments sequentially
- **Automated CI/CD**: Deployments from develop onwards are automated
- **Approval Gates**: Production requires manual approval
- **Code-Based Changes**: All changes must be code-based (no manual changes)
- **Environment Alignment**: Follows enterprise SDLC environment standards
- **Traceability**: Every deployment tracked through pipeline with audit logs

## Support & Exceptions

For questions or exception requests, contact:
- **DevOps Team**: [Contact Details]
- **Enterprise Architecture Team**: [Contact Details]

Exception requests require approval from Enterprise Architecture team.
