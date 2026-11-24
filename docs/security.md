# Security and data deletion

## Role model

| Role | Intended use |
| --- | --- |
| `x_snc_deployer.viewer` | Read configuration assets and source-control-derived dependency trees. |
| `x_snc_deployer.developer` | Viewer access plus creation of Repository Application Status records. |
| `x_snc_deployer.manager` | Trigger deployments and read configuration and dependency information. |
| `x_snc_deployer.admin` | Full Deployer configuration and process access; contains the manager role. |
| `x_snc_deployer.api_controller` | Call Controller API endpoints. |
| `x_snc_deployer.api_target` | Call Target API endpoints. |

Assign API roles explicitly. Since Deployer 1.22.0, `admin` does not automatically grant Controller or Target API access.

The full target API role description notes that ATF execution depends on the roles of the calling user. The upstream setup assigns `admin` and `security_admin` to a fully capable target technical user. Where that privilege is unacceptable, separate target integrations by capability and prove the minimum role set with ATF in a non-production instance.

## Technical-user policy

- Use separate controller API and target API accounts.
- Do not reuse personal accounts.
- Limit each account to the instances and endpoints it needs.
- Prefer OAuth 2.0 or a managed credential with rotation.
- Monitor authentication failures and unusual deployment activity.
- Disable accounts before deleting them so running integrations fail visibly.
- Never commit secrets or exported credential records.

## Data-deletion protection

Deployer compares tables and columns present on the target with the metadata in the applications selected for deployment. Each Target Instance requires one of these policies:

| Policy | Behavior | Recommended use |
| --- | --- | --- |
| Deny | Blocks a deployment that would remove detected tables or columns. | Default for TEST and PROD; safest starting point everywhere. |
| Confirm | Creates a manual confirmation before deletion is allowed. | Controlled development or test environments with qualified reviewers. |
| Warn | Allows deletion after warning the operator. | Exceptional disposable environments only. |

Before approving a deletion:

1. Identify each table and column.
2. Confirm that it belongs to the intended application and release.
3. Determine whether it contains business, audit, integration, or reference data.
4. Review downstream scripts, reports, ACLs, integrations, and dashboards.
5. Create a recoverable backup or export where required.
6. Link the approval evidence to the change and deployment.
7. Validate the result in a lower environment first.

## Network and MID Server controls

Repository and target calls can be routed through MID Servers. Use this when direct instance egress is not allowed or when services are private. Restrict MID Server access to required hosts and ports, and monitor its queue and status. A MID Server solves routing; it does not replace credential scoping or TLS validation.

## Production controls

- Require a successful RAS and reviewed dependency tree.
- Require current target validation and inventory.
- Keep data deletion on Deny unless a specific approved change requires Confirm.
- Treat skipped CodeSanity or ATF as a policy exception.
- Ensure manual tasks contain clear entry, success, and rollback instructions.
- Keep deployment and task records as the execution audit trail.
- Reconcile target state after failure, cancellation, timeout, or ignored task failure.

## API shutdown

Set `x_snc_deployer.active` to `false` to disable Deployer REST APIs during a security response or maintenance window. Record why and when it was changed; remember that controller-to-target operations will fail until it is restored.
