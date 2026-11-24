# Reference

This page summarizes the configuration and process metadata shipped in Deployer 1.26.0.

## Tables

| Label | Table | Purpose |
| --- | --- | --- |
| Deployment | `x_snc_deployer_deployment` | Task-derived parent record for one application/version deployment to one target. |
| Deployment Task | `x_snc_deployer_deployment_task` | Task-derived ordered unit of manual or automatic deployment work. |
| Instance | `x_snc_deployer_instance` | Target URL, credential, environment type, deletion policy, and execution preferences. |
| Pipeline | `x_snc_deployer_pipeline` | Named deployment path used to compare instance status. |
| Pipelines to Instances | `x_snc_deployer_m2m_pipline_instance` | Ordered many-to-many link between pipelines and targets. The spelling `pipline` is part of the shipped table name. |
| Repository Application Status | `x_snc_deployer_repository_application_status` | Preflight result and dependency tree for a repository branch/version. |
| Repository Credentials | `x_snc_deployer_repository_credentials` | Maps URL prefixes to source-control provider credentials and optional MID Servers. |

## Deployment states

| State | Value |
| --- | --- |
| Draft | `1000` |
| Preparing | `1500` |
| Ready | `2000` |
| In Progress | `3000` |
| Completed | `10000` |
| Cancelled | `11000` |
| Failed | `12000` |

Deployment execution choices are `manual` and `automatic`.

## Deployment Task states

| State | Value |
| --- | --- |
| Pending | `1000` |
| Ready | `2000` |
| In Progress | `3000` |
| Complete | `10000` |
| Cancelled | `11000` |
| Failed | `12000` |
| Skipped | `13000` |
| Failure Ignored | `14000` |

## Deployment Task actions

| Label | Value | Execution |
| --- | --- | --- |
| Pre-Deployment Activities | `pre_deployment_activities` | Manual |
| Confirm Data Deletion | `data_deletion_confirm` | Manual |
| Plugin Installation | `plugin_install` | Automatic |
| Plugin Update | `plugin_update` | Automatic |
| Store App Installation | `store_app_install` | Automatic |
| Store App Update | `store_app_update` | Automatic |
| Install Application through Import from Source Control | `install_import_from_source_control` | Automatic |
| Install new version from Source Control | `install_switch_branch` | Automatic |
| Update from Source Control | `install_apply_remote_changes` | Automatic |
| Run CodeSanity Instance Scan Suite | `run_codesanity` | Automatic |
| Run Installation Scripts | `run_install_scripts` | Automatic |
| Run default ATF Test Suite | `run_atf_test_suite_default` | Automatic |
| Smoke Test | `smoke_test` | Manual |
| Post-Deployment Activities | `post_deployment_activities` | Manual |

## RAS values

Modes:

- Deployment: `deployment`
- Preview: `preview`
- Checkout: `checkout`

Statuses:

- Draft: `draft`
- Processing: `processing`
- Ready for Deployment: `ok`
- NOT ready for Deployment: `error`
- Previewed Branch: `previewed`
- Reviewed Dev Branch: `dev`

## Instance and repository choices

- Instance types: DEV (`dev`), TEST (`test`), PROD (`prod`)
- Data-deletion policies: Deny (`deny`), Confirm (`confirm`), Warn (`warn`)
- Repository types: GitHub (`github`), GitLab (`gitlab`), AzureDevOps (`azuredevops`)

## System properties

| Name | Type | Default |
| --- | --- | --- |
| `x_snc_deployer.active` | Boolean | `true` |
| `x_snc_deployer.logging.verbosity` | Choice list | `info` |
| `x_snc_deployer.required_target_version` | String | `1.25.0` |

## UI actions

| Record | Actions |
| --- | --- |
| Deployment | Cancel, Go to Instance, Go to RAS, Mark as Complete, Mark as Failed, Prepare, Repeat, Retry, Show Task List, Start |
| Deployment Task | Cancel Task, Ignore Failure, Mark Task as Complete, Mark Task as Failed, Retry Task Execution, Skip Task |
| Target Instance | Create Deployment, Update Local CMDB, Validate |
| Pipeline | View Status |
| Repository Application Status | Create Deployment, Get New Preview For Same Branch, Get New Status For Same Branch, Get New Status For Same Version, Get Status For New Version, View |

Action visibility depends on record state and user role.

## Navigation modules

The application menu includes Repository App Status, Deployments, Repository Credentials, Target Instances, Pipelines, Credentials, Technical Users, Installed Apps, App Versions in Repos, Test Suite, and Log. Administrative modules require `x_snc_deployer.admin` where configured.

## Scripted REST APIs

| API | Method | Path |
| --- | --- | --- |
| Controller / Start Deployment | GET | `/api/x_snc_deployer/controller/start_deployment` |
| Target / Get Credential | GET | `/api/x_snc_deployer/target/get_credential` |
| Target / Get Status | GET | `/api/x_snc_deployer/target/get_status` |
| Target / Get Transactions | GET | `/api/x_snc_deployer/target/get_transactions` |
| Target / Run Installation Scripts | GET | `/api/x_snc_deployer/target/run_install_scripts` |

See [Headless REST API](headless-api.md) for parameters and usage.

## Important server-side interfaces

These are implementation interfaces, not a supported public API contract:

- `HeadlessDeployment()` starts a controller-driven deployment.
- `CreateDeployment()` creates a deployment record.
- `GetRepoDependencyTree()` and `GetRepoDependencyList()` resolve repository dependencies.
- `IdentifyDataDeletion()` finds target tables and columns missing from the selected application set.
- `GetPipelineStatus()` builds pipeline status.
- `SourceControlAPI` abstracts supported repository providers.
- `TargetInstance*` functions call target operations, including validation, status, source-control actions, plugin activation, App Repo installation, Instance Scan, ATF, install scripts, and inventory updates.

`ValidateInstance()` is deprecated; use `TargetInstanceValidate()` in application code.
