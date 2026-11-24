# Operations

## Routine health check

Run this check after installation, upgrades, credential rotation, clones, and network changes:

1. Confirm Deployer and DevTools versions.
2. Confirm `x_snc_deployer.active` is `true`.
3. Validate each Target Instance.
4. Update each target's local inventory.
5. Open each Pipeline with **View Status** and investigate instances shown as not ready.
6. Create a Preview RAS for a known repository.
7. Review **Deployer > Log** for new errors or repeated warnings.
8. Run the Deployer ATF suite on non-production instances.

## Before a release window

- Validate the target and controller-to-target credential.
- Confirm source-control credentials are not near expiration.
- Refresh the target inventory.
- Create a new RAS after the release branch is final.
- Review the full dependency tree, including plugins and Store apps.
- Check target Deployer version against `x_snc_deployer.required_target_version`.
- Review data deletion, downgrade, refresh, CodeSanity, and ATF options.
- Verify manual-task assignees and instructions.
- Confirm the rollback decision and recovery artifacts.

## Monitor a deployment

Watch the parent Deployment and its ordered Deployment Tasks. For the active task, check:

- state and retry count;
- task action and execution type;
- repository URL, branch, and version;
- **Start Here** link for manual or remote work;
- **Result Details** link for scan, ATF, or installation evidence;
- deployment and application logs on both controller and target;
- active transactions when the target seems busy.

Do not assume cancellation stops remote work already in progress. Reconcile the target before starting a replacement deployment.

## Logs

Use **Deployer > Log** for application-scope log entries. Common categories include:

- repository parsing and provider API calls;
- dependency discovery;
- target requests and responses;
- deployment preparation;
- source-control operations;
- installation scripts;
- Instance Scan and ATF progress.

The default logging verbosity is `info`. Raise `x_snc_deployer.logging.verbosity` only long enough to reproduce a problem, because detailed logs may be noisy and may include repository or instance identifiers.

## Inventory maintenance

Use **Update Local CMDB** when:

- a target was cloned or rebuilt;
- applications changed outside Deployer;
- a deployment timed out or was cancelled;
- a source-control operation was completed manually;
- the Pipeline view disagrees with the target;
- a new target is added.

RAS creation updates versions known to exist in repositories. Deployment execution and explicit target refreshes update what is known to be installed.

## Credential rotation

1. Create or update the ServiceNow credential record without exposing the secret.
2. Confirm every Repository Credential or Target Instance reference uses the intended record.
3. Test repository access with Preview mode.
4. Test target access with **Validate**.
5. Retire the old secret after all mappings pass.
6. Review logs for systems still attempting the old credential.

## Clone recovery

After a ServiceNow clone, assume environment-specific credentials, URLs, technical users, properties, and inventories may be wrong.

1. Disable unintended outbound deployment activity.
2. Restore environment-specific credentials and target definitions.
3. Verify source and target URLs.
4. Verify technical users and API roles.
5. Validate targets and refresh inventories.
6. Run non-production ATF and a Preview RAS.
7. Re-enable deployment automation only after verification.

## Evidence to retain

- RAS result and dependency tree;
- generated task list;
- deletion review;
- scan and ATF result URLs;
- manual task completion notes;
- deployment final state and logs;
- target inventory after deployment;
- incident or exception record for skipped or ignored controls.
