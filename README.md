# -Jenkins-Ansible-Continuous-Delivery-Pipeline
[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)]() [![License](https://img.shields.io/badge/license-MIT-blue)]() [![Ansible](https://img.shields.io/badge/ansible-2.9+-red)]() [![Jenkins](https://img.shields.io/badge/jenkins-2.x-orange)]()

> Automated continuous delivery pipeline integrating Jenkins and Ansible for seamless application deployment across multiple environments.

## Overview

This project implements a production-ready CI/CD pipeline that automates the entire software delivery lifecycle from code commit to production deployment. The solution eliminates manual intervention, reduces deployment time, and ensures consistent deployments across staging and production environments.

### Key Features

- **Automated Build & Test**: Maven-based builds with integrated unit testing
- **Code Quality Gates**: SonarQube integration with configurable quality thresholds
- **Artifact Management**: Nexus Repository for versioned artifact storage
- **Multi-Environment Deployment**: Separate staging and production pipelines
- **Intelligent Rollback**: Automated rollback on deployment failures
- **Zero-Downtime Deployments**: Rolling deployment strategy with health checks
- **Notification System**: Slack integration for team alerts
- **Infrastructure as Code**: Ansible playbooks for consistent server configurations

## Architecture

Developer → GitHub → Jenkins → Maven → SonarQube → Nexus
↓
Ansible
↓
Staging ← → Production


### Technology Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| CI/CD Orchestration | Jenkins 2.x | Pipeline automation and coordination |
| Configuration Management | Ansible 2.9+ | Application deployment and server configuration |
| Build Tool | Maven 3.x | Dependency management and artifact building |
| Code Quality | SonarQube 8.x | Static code analysis and quality gates |
| Artifact Repository | Nexus Repository | Artifact storage and dependency management |
| Version Control | Git/GitHub | Source code management |
| Application Server | Apache Tomcat 9 | Java application hosting |
| Cloud Platform | AWS EC2 | Infrastructure hosting |
| Notifications | Slack | Team communication and alerts |

## Prerequisites

- **Jenkins Server**: Version 2.300+ with plugins:
  - Ansible Plugin
  - Pipeline Plugin
  - GitHub Integration Plugin
  - Slack Notification Plugin
- **Ansible**: Version 2.9 or higher
- **AWS Account**: For EC2 instance provisioning
- **GitHub Account**: Repository with webhook support
- **Nexus Repository**: Version 3.x
- **SonarQube Server**: Version 8.x or higher


## Pipeline Stages

### Stage 1: Code Checkout
Jenkins detects GitHub webhook and fetches latest code changes.

### Stage 2: Unit Testing
Maven executes test suite. Pipeline fails if tests don't pass.

### Stage 3: Code Analysis
- **Checkstyle**: Code style validation
- **SonarQube**: Deep static analysis with quality gate enforcement

### Stage 4: Build Artifact
Maven packages application with timestamp-based versioning: `app-1.0-20251030-143022.war`

### Stage 5: Upload to Nexus
Versioned artifact stored in Nexus Repository for traceability.

### Stage 6: Deploy to Staging
Ansible downloads artifact and deploys to staging environment with automatic rollback on failure.

### Stage 7: Automated Testing
Test suites execute against staging deployment. QA team receives notification.

### Stage 8: Production Deployment
Upon approval, same Ansible playbook deploys to production with zero downtime.


### Slack Notifications

Teams receive alerts for:
- ✅ Successful deployments
- ❌ Pipeline failures
- ⚠️ Rollback events
- 🔄 Pending approvals

## Security

### Credential Management

- Credentials stored in Jenkins Credential Store
- Ansible Vault encrypts sensitive variables
- SSH keys managed with restricted permissions (600)
- Security groups follow least privilege principle

### Access Control

- Staging: Developer and QA team access
- Production: DevOps and SRE team only
- Approval gates for production deployments

## Best Practices

### Code Quality
- Minimum 80% code coverage required
- Zero critical/blocker issues from SonarQube
- Automated code style validation with Checkstyle

### Deployment Safety
- Always deploy to staging before production
- Mandatory health checks after deployment
- Automated rollback on any failure
- Approval gates for production changes

### Artifact Management
- Timestamp-based versioning for traceability
- Nexus cleanup policy removes artifacts older than 90 days
- Keep last 10 versions of each artifact




