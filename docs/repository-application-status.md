# Repository Application Status

A Repository Application Status (RAS) is the preflight check for a repository branch and its dependencies. It answers: “Can Deployer build a coherent deployment plan from this source-control state?”

## Choose the correct mode

| Mode | Input | Version compatibility | Can create deployment? | Typical use |
| --- | --- | --- | --- | --- |
| Deployment | Repository URL + exact version branch | Strict | Yes | Deploy a released application version |
| Preview | Repository URL + branch | Strict dependency availability check | No | Check `dev` or a candidate branch before baselining |
| Checkout | Repository URL + branch shared by dependencies | Not strict | Yes | Initialize or reset a development instance from a branch such as `dev` |

## Create a deployment RAS

1. Open **Deployer > Repository App Status**.
2. Select **New**.
3. Set **Mode** to **Deployment**.
4. Enter the canonical repository clone URL used by your Repository Credential mapping.
5. Enter an exact semantic version such as `2.3.1`.
6. Save or submit the record.
7. Wait for background processing to finish.
8. Confirm the status is **Ready for Deployment**.
9. Select **View** and inspect the dependency tree before creating the deployment.

Processing updates the controller's record of application versions available in source control.

## Read the result

| Status | Meaning | Next action |
| --- | --- | --- |
| Draft | Processing has not begun. | Save/submit and check background processing. |
| Processing | Repositories and dependencies are being read. | Wait; investigate only if it remains stuck. |
| Ready for Deployment | Exact versions and dependencies are available. | Review the tree, then create a deployment. |
| NOT ready for Deployment | One or more checks failed. | Review `Errors`, display details, and logs. |
| Previewed Branch | Preview completed. | Review results; baseline or fix the branch. |
| Reviewed Dev Branch | Checkout review completed. | Review results; create a checkout deployment if intended. |

The `Errors` field is a count. The display and dependency tree show which repository or dependency failed.

## Dependency rules

- Deployer recursively reads direct and indirect dependencies.
- A deployment requires exact version branches; it does not silently substitute a merely compatible branch.
- A version branch must be named as a three-part semantic version, such as `1.10.0`.
- Checkout mode is the exception: it follows the same named branch across dependencies and does not enforce version compatibility.
- Plugins and Store apps declared as dependencies are included in the plan and checked against target state.

## Refresh actions

Use the action that matches your intent:

- **Get New Status For Same Version** repeats a Deployment-mode check for the same version.
- **Get Status For New Version** creates a status check for another release version.
- **Get New Preview For Same Branch** repeats a Preview-mode check.
- **Get New Status For Same Branch** repeats a Checkout-mode check.

These actions create a new record so previous evidence remains available.

## Common failures

### Provider cannot be determined

The Repository Credential mapping is inactive, the URL prefix does not match, or the provider type is wrong. Compare the RAS URL with **Repository URLs start with** character for character.

### Repository or branch not found

Confirm the credential can read the repository and that the exact version branch exists. Check case, URL encoding, and provider-specific project paths.

### Dependency version missing

Open the dependency tree to identify the dependency and required version. Publish the missing version branch or correct the dependency metadata in the source application; do not change the target to conceal an inconsistent release.

### Processing does not finish

Check **Deployer > Log**, outbound HTTP access, MID Server status, and provider rate limits. Temporarily increase `x_snc_deployer.logging.verbosity` if the normal log does not explain the stall.

## Before creating a deployment

- Status is **Ready for Deployment**.
- Repository and exact version are correct.
- Every application and non-custom dependency in the tree is expected.
- The selected target is validated and its local inventory is current.
- Any apparent table or column deletion is understood.
- The change owner has reviewed skipped scan or ATF settings.

Continue with [Deployments](deployments.md).
