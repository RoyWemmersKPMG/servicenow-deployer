# Release and upgrade

## Compatibility for 1.26.0

- Supported ServiceNow releases: Zurich and Australia.
- Required DevTools version: 1.146.0.
- Controller application version: 1.26.0 in this repository.
- Minimum target version configured by the application: 1.25.0.

Keep controller and targets on the same Deployer version where practical. The minimum target property is a protocol floor, not a recommendation to keep targets behind.

## Upgrade procedure

1. Read release notes between the installed and desired versions.
2. Confirm ServiceNow and DevTools compatibility.
3. Pause new deployments and let running deployments finish.
4. Back up environment-specific configuration and record current property values.
5. Upgrade DevTools first when the new Deployer version requires it.
6. Upgrade Deployer on a non-production target.
7. Run the installation script in scope `x_snc_deployer`:

   ```javascript
   x_snc_devtools.InstallApp("x_snc_deployer");
   ```

8. Run the Deployer ATF suite.
9. Validate repository access and target API operations.
10. Upgrade remaining targets, then the controller, within a controlled compatibility window.
11. Validate every target and refresh local inventories.
12. Run a Preview RAS and a non-production deployment.
13. Resume automation.

Adjust the target/controller order if a release note explicitly requires a different sequence.

## Highlights in 1.26.0

- Requires DevTools 1.146.0.
- Adds automatic plugin installation/update and Store app installation/update tasks.
- Speeds RAS creation by using dictionary XML files as the primary source for dictionary-record identification.
- Adds custom application selection to Target API **Get Status**.
- Adds **Repeat** for successful deployments.
- Adds target functions for plugin activation and App Repo installation.

## Known limitations

### Azure DevOps branch protection

`SourceControlAPI::ProtectBranchReadOnly()` does not reliably prevent later commits on Azure DevOps even when the branch is locked. Use Azure DevOps repository policies for enforcement.

### Exact plugin version activation

The platform endpoint used to activate a plugin cannot request a specific plugin version. Deployer cannot currently guarantee that plugin activation reaches the exact requested version.

### Historical parameter name

Target API **Run Installation Scripts** accepts an application scope as well as a sys_id, but its required parameter is still called `application_sys_id`.

### Response contract

The Scripted REST API response schema is not separately versioned. Integration tests should validate the fields external automation depends on.

## Post-upgrade acceptance

- Application and dependency versions match the release requirements.
- Deployer ATF passes.
- Repository mappings work for all provider types in use.
- Target validation succeeds through every configured network path.
- Local inventory refresh succeeds.
- Preview and Deployment-mode RAS processing succeeds.
- Plugin/Store app status is returned when requested.
- A representative deployment completes in non-production.
- Pipeline views show current versions and target readiness.
