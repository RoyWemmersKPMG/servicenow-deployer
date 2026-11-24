# Troubleshooting

Start with the symptom, then check the controller and target sides separately. Most failures are caused by one of four boundaries: repository access, controller-to-target access, target execution, or version/dependency consistency.

## Quick diagnostic order

1. Read the error on the RAS, Deployment, or Deployment Task.
2. Open **Deployer > Log** on the controller.
3. Validate the Target Instance.
4. Check the same timestamp in the target application log and system log.
5. Verify credentials, roles, URLs, and MID Server status.
6. Verify exact Deployer, DevTools, and ServiceNow versions.
7. Check whether the requested operation actually completed remotely before retrying.

## Installation and upgrade

### Import fails with checksum or mixed revision errors

The application directory is a generated ServiceNow export. Do not hand-edit it or merge incompatible generated snapshots. Revert offending commits or replace the snapshot with one exported consistently from ServiceNow. See [Development](development.md).

### Application installed before its dependencies

ServiceNow source control does not resolve dependencies. Delete the incorrectly imported application, install/activate required dependencies, then re-import it. Validate this procedure in a non-production instance before removing an installed application with data.

### Install script fails

Confirm it ran in scope `x_snc_deployer`, DevTools is installed at the required version, and the script is exactly:

```javascript
x_snc_devtools.InstallApp("x_snc_deployer");
```

Review scope logs and the DevTools installation documentation.

## Repository access

### Repository type cannot be determined

- Repository Credential record is active.
- Type matches GitHub, GitLab, or AzureDevOps.
- URL prefix matches protocol, host, organization/group, and encoding.
- GitHub Enterprise Server has **GitHub On-Premise** enabled and the correct API URL.
- The selected MID Server is up and can reach the provider.

### 401 or 403 from source control

Rotate or correct the token, confirm repository read permission, and ensure the ServiceNow credential type matches the provider integration. Test the same mapping with a Preview RAS.

### Branch missing although it exists

Check exact spelling, case, URL encoding, and provider project path. Deployment mode requires an exact three-part semantic-version branch. Confirm the branch is visible to the configured credential, not just to your personal account.

### Azure DevOps branch is locked but still writable

This is a documented known defect in branch protection. Do not treat Deployer's Azure DevOps lock as an enforcement control; implement protection in Azure DevOps policy until the limitation is resolved.

## Target connectivity and API

### Target validation fails

- Target base URL is correct and uses HTTPS.
- Credential belongs to an active technical user on that target.
- User has `x_snc_deployer.api_target` explicitly.
- `x_snc_deployer.active` is `true` on the target.
- Network route or MID Server is healthy.
- Target Deployer version meets the controller's required minimum.
- TLS trust and proxy configuration permit the connection.

### 401, 403, or 404

`401` usually indicates authentication. `403` indicates an explicit API role or ACL. `404` indicates the wrong instance/path or an inactive/missing API. Verify the base paths in [Headless REST API](headless-api.md).

### Target version is rejected

Upgrade the target. Do not lower `x_snc_deployer.required_target_version` to hide a protocol incompatibility unless you are testing a deliberate, documented exception.

## RAS and dependencies

### RAS stays in Processing

Look for a long provider request, MID Server queue issue, background worker error, or provider rate limit. Temporarily increase logging detail and create a fresh RAS rather than modifying historical evidence.

### Required dependency version unavailable

Publish the exact dependency branch or correct the application's dependency declaration. Deployment mode does not fall back to another compatible version.

### Unexpected dependency appears

Inspect dependency metadata in the source application and recursively inspect its dependencies. Confirm the RAS is reading the intended branch and repository, not a similarly named fork.

## Deployment execution

### Prepare never reaches Ready

Preparation is asynchronous. Review the deployment log, target validation, target inventory, and schema-deletion calculation. Confirm the assigned user and mandatory fields are present.

### Task timed out

Before retrying, inspect the target application/branch, active transactions, ATF runs, scan runs, or installation logs. A remote request can finish after the controller times out.

### Source-control operation collides with the previous one

Review `x_snc_devtools.sourcecontrol.cooldown.seconds` on the controller. Increase it cautiously if the instance reports completion before the source-control operation is actually settled.

### Deployment cancelled but target continues changing

Cancellation suppresses further task progression; it cannot guarantee termination of work already running on the target. Wait for the remote transaction to finish, refresh the local inventory, assess target state, and create a fresh deployment if needed.

### Plugin version does not match the requested version

The current platform plugin-activation endpoint cannot request a specific plugin version. This is a documented unfinished capability. Verify plugin version outside the automatic task and apply an approved manual process when exact version enforcement is required.

## ATF and Instance Scan

### ATF does not run

- `sn_atf.runner.enabled` is `true` on the target.
- The application has a default ATF suite.
- The target technical user has roles required by the tests.
- The deployment did not skip default ATF.
- No other ATF run or transaction is blocking execution.

### CodeSanity fails

Follow the Result Details link and fix findings in the application. Skip or ignore only under an explicit exception. Re-run the task after remediation where possible.

### Result link is inaccessible

The result lives on the target. Confirm your interactive account can access that instance and record; the controller's technical credential does not grant your browser session access.

## Escalation bundle

When asking for help, include:

- Deployer, DevTools, and ServiceNow versions;
- controller or target role of each affected instance;
- RAS, Deployment, and Task identifiers;
- provider type and sanitized repository path;
- timestamps and correlation details;
- controller and target log excerpts with secrets removed;
- whether a MID Server is involved;
- what completed remotely before the failure.
