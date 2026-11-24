# Installation

Install DevTools before Deployer. The repository's generated README notes that ServiceNow source control does not resolve dependencies for you; importing an application before its dependencies may require deleting and re-importing the application after the dependencies are installed.

## Shared installation procedure

Perform these steps on every instance that will act as a development, controller, or target instance.

1. Install the required ServiceNow platform dependencies, including the Task table schema where applicable.
2. Install [DevTools 1.146.0](https://www.wildgrube.com/servicenow-devtools/) and complete its installation procedure.
3. Fork [the Deployer repository](https://github.com/saschawildgrube/servicenow-deployer).
4. In ServiceNow Studio, import the Deployer application from your fork.
5. Run this background script in application scope `x_snc_deployer`:

   ```javascript
   x_snc_devtools.InstallApp("x_snc_deployer");
   ```

6. On a non-production instance, run the **Deployer** ATF test suite.
7. Review the Deployer application log for installation errors.

Use a source-control credential appropriate to your provider. The upstream GitHub procedure uses a classic personal access token with repository access stored in a ServiceNow Basic Auth credential. Apply your organization's credential and token policies, and do not put secrets in this repository.

## Target instance setup

After the shared procedure:

1. Set `sn_atf.runner.enabled` to `true` if Deployer will run ATF on this target.
2. Elevate to `security_admin` while performing the security configuration.
3. Create a dedicated technical user for controller-to-target calls.
4. Assign `x_snc_deployer.api_target`.
5. Assign only the additional roles required for intended operations. ATF execution evaluates the roles of the user that starts the tests; the upstream configuration uses `admin` and `security_admin` for full target functionality.
6. Configure Repository Credential mappings on the target. This is required when the target must import an application from source control for the first time.
7. Confirm `x_snc_deployer.active` is `true`.

Do not reuse a human administrator account for controller-to-target communication.

## Controller instance setup

After the shared procedure:

1. Configure Repository Credential mappings for every repository host, group, or repository the controller must inspect.
2. Create a ServiceNow credential for each target technical user.
3. Create Target Instance records.
4. Use **Validate** on every target.
5. Use **Update Local CMDB** on every target.
6. Create Pipelines if you want a cross-instance version view.
7. If external automation will start deployments, create a dedicated controller API user and assign `x_snc_deployer.api_controller`.

## Development instance setup

After the shared procedure, configure Repository Credential mappings for the repositories whose version branches should be protected during DevTools baselining.

Deployer integrates with the DevTools application-baseline extension point. New version branches are protected for supported providers. Azure DevOps branch protection is a known limitation: setting the branch to locked does not reliably prevent further commits.

## Verification checklist

- The installed Deployer version is 1.26.0.
- DevTools is 1.146.0.
- The Deployer ATF suite passes on non-production instances.
- `x_snc_deployer.active` is `true`.
- Target API technical users are active and correctly scoped.
- Repository Credential mappings match real repository URLs.
- Controller Target Instance records pass **Validate**.
- The controller can refresh the local inventory.
- A Preview-mode Repository Application Status can read a known repository.

Continue with [Configuration](configuration.md).
