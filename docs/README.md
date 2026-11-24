# ServiceNow Deployer knowledge base

Deployer is a ServiceNow application for repeatable, source-control-based deployments. A controller instance plans and coordinates work; target instances expose a restricted API that performs deployment operations.

This knowledge base is organized around tasks. If this is your first time using Deployer, follow the pages in the order below.

## Start here

1. [Learn the core concepts](getting-started.md).
2. [Install Deployer](installation.md) on the required instances.
3. [Configure credentials, targets, and pipelines](configuration.md).
4. [Check an application with Repository Application Status](repository-application-status.md).
5. [Run and monitor a deployment](deployments.md).

For automation, see the [headless REST API](headless-api.md). For failures, start with [troubleshooting](troubleshooting.md).

## Browse by topic

| Topic | Use it when you need to... |
| --- | --- |
| [Getting started](getting-started.md) | Understand prerequisites, terminology, and the shortest path to a first deployment |
| [Architecture](architecture.md) | Understand instance roles, trust boundaries, dependency order, and data flow |
| [Installation](installation.md) | Install or validate Deployer on development, controller, and target instances |
| [Configuration](configuration.md) | Configure source-control access, target instances, pipelines, and properties |
| [Repository Application Status](repository-application-status.md) | Validate a repository and its dependency tree before deployment |
| [Deployments](deployments.md) | Prepare, start, monitor, retry, and complete deployments |
| [Headless REST API](headless-api.md) | Integrate Deployer with an external pipeline or call target operations |
| [Security and data deletion](security.md) | Assign roles, manage technical users, and choose deletion safeguards |
| [Operations](operations.md) | Run routine checks, interpret logs, and maintain the local application inventory |
| [Troubleshooting](troubleshooting.md) | Diagnose configuration, source-control, deployment, API, ATF, and scan failures |
| [Reference](reference.md) | Look up tables, states, task types, properties, roles, and UI actions |
| [Release and upgrade](release-and-upgrade.md) | Upgrade safely and review version compatibility |
| [Development](development.md) | Work with the generated repository without breaking its checksum |
| [Glossary](glossary.md) | Look up project-specific terms |

## Supported baseline

This wiki documents repository version **1.26.0**. The application metadata and upstream manual identify these requirements:

- ServiceNow **Zurich or Australia**
- Deployer **1.26.0** on the controller
- Deployer **1.25.0 or later** on target instances, as configured by `x_snc_deployer.required_target_version`
- DevTools **1.146.0**
- Access from the relevant ServiceNow instance or MID Server to the source-control provider and other ServiceNow instances
- Supported repository providers: GitHub/GitHub Enterprise, GitLab, and Azure DevOps

See [Sources and documentation policy](SOURCES.md) for how this wiki was assembled.
