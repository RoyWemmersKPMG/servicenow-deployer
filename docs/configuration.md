# Configuration

## Repository Credentials

Repository Credential records map a repository URL prefix to a ServiceNow credential and provider. Deployer chooses a mapping from the requested repository URL, so use the narrowest practical URL prefixes and avoid ambiguous overlaps.

Open **Deployer > Repository Credentials** and configure:

| Field | Guidance |
| --- | --- |
| Active | Only active mappings are used to identify a repository provider. |
| Type | GitHub, GitLab, or AzureDevOps. |
| Repository URLs start with | URL prefix covered by this mapping. A host prefix covers many repositories; a full repository URL limits the mapping to one. |
| Credential | ServiceNow credential containing the provider token or authentication details. |
| MID Server | Optional route for private source-control systems. |
| API URL | Provider API base URL when it cannot be derived from the repository URL. |
| GitHub On-Premise | Enable for GitHub Enterprise Server rather than github.com. |

Test with a Preview-mode Repository Application Status. If Deployer cannot determine the provider, first verify that the mapping is active and the stored URL prefix matches the repository URL exactly, including protocol and host.

## Target Instances

Open **Deployer > Target Instances** and create one record per deployment target.

| Field | Guidance |
| --- | --- |
| Name | Stable unique name used by users and the Controller API, for example `ACME-TEST`. |
| URL | Base instance URL, for example `https://acme-test.service-now.com`. |
| Type | DEV, TEST, or PROD. |
| Credential | ServiceNow credential for the target technical user. OAuth 2.0 is supported. |
| MID Server | Optional route from the controller to the target. |
| Data Deletion Policy | Deny, Confirm, or Warn. Start with Deny. See [Security](security.md). |
| Run CodeSanity checks | Add Instance Scan work for each deployed application. |
| Run default ATF Test Suite | Add the application's default ATF suite to the deployment. |
| Pre-Deployment Activities | Add a manual task before automatic deployment work. |
| Smoke Test | Add a manual smoke-test task. |
| Post-Deployment Activities | Add a manual task after automated work. |
| Instructions fields | Operator-facing instructions copied into the corresponding manual task. |
| Platform Application Scope | Scope used for platform-related application handling in this environment. |

After saving, use **Validate**, then **Update Local CMDB**. Resolve validation failures before creating a production deployment.

## Pipelines

A Pipeline is an ordered group of Target Instances used to view where application versions are installed.

1. Create a Pipeline under **Deployer > Pipelines**.
2. Add instances through the related **Pipelines to Instances** records.
3. Set `Order` to match the delivery path, for example DEV 100, TEST 200, PROD 300.
4. Use **View Status** to compare application versions and target readiness.

Pipelines visualize state; they do not automatically promote a deployment from one instance to the next.

## System properties

| Property | Default | Purpose |
| --- | --- | --- |
| `x_snc_deployer.active` | `true` | Enables Deployer REST APIs. Disable only when intentionally taking the API surface out of service. |
| `x_snc_deployer.logging.verbosity` | `info` | Controls application log detail. Increase temporarily for diagnosis, then restore it. |
| `x_snc_deployer.required_target_version` | `1.25.0` | Minimum Deployer version accepted for remote target operations. Do not lower this merely to bypass compatibility errors. |

The controller also observes DevTools source-control cooldown behavior. If source-control operations appear to overlap, review `x_snc_devtools.sourcecontrol.cooldown.seconds` on the controller.

## Recommended environment policy

| Setting | DEV | TEST | PROD |
| --- | --- | --- | --- |
| Data deletion | Confirm or Deny | Deny | Deny |
| CodeSanity | On | On | On unless validated earlier under an approved exception |
| Default ATF | On | On | Based on production test policy; warnings appear when enabled for PROD |
| Manual smoke test | Optional | On | On |
| Pre/post activities | As needed | As needed | Use for change controls and operational checks |

Document intentional exceptions directly in deployment/change records.
