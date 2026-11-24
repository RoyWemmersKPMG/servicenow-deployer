# Headless REST API

Deployer exposes authenticated Scripted REST APIs for starting deployments and performing target-side operations. The APIs are enabled only while `x_snc_deployer.active` is `true`.

## Authentication

- Give external pipeline users `x_snc_deployer.api_controller` for the Controller API.
- Give controller-to-target users `x_snc_deployer.api_target` for the Target API.
- API roles are explicit. An `admin` user does not automatically receive them.
- Use dedicated technical users and encrypted ServiceNow credential records.
- Use OAuth 2.0 for inter-instance authentication where your environment supports it.
- Do not place tokens or passwords in query strings, scripts, logs, or this repository.

All current operations use `GET`, including operations that start work. Call them only from systems that will not prefetch, cache, or replay URLs. Never expose these endpoints through unauthenticated links.

## Controller API

Base path:

```text
/api/x_snc_deployer/controller
```

### Start Deployment

Starts a headless deployment for an exact repository branch and configured target name.

```http
GET /api/x_snc_deployer/controller/start_deployment?repo_url=https%3A%2F%2Fgithub.com%2Facme%2Fmy-app.git&branch=1.2.0&instance=ACME-TEST
Accept: application/json
Authorization: <configured authentication>
```

| Parameter | Required | Description |
| --- | --- | --- |
| `repo_url` | Yes | Repository clone URL covered by an active Repository Credential mapping. |
| `branch` | Yes | Exact deployable branch, normally a three-part semantic version. |
| `instance` | Yes | Name of a configured Target Instance record. |

The call starts work asynchronously. The pipeline should retain the returned identifiers and poll or query ServiceNow records according to its integration design. Treat an HTTP success as acceptance of the request, not proof that the deployment completed.

## Target API

Base path:

```text
/api/x_snc_deployer/target
```

These endpoints are normally called by a controller rather than directly by a human.

### Get Status

Returns instance information and selected installed custom applications, plugins, and Store apps.

```http
GET /api/x_snc_deployer/target/get_status?apps=x_snc_deployer,DevTools&plugins=com.snc.notify&store_apps=sn_library_intent
```

| Parameter | Required | Description |
| --- | --- | --- |
| `apps` | No | Comma-separated custom app sys_ids, scopes, or names. Use `none` to omit custom apps. |
| `plugins` | No | Comma-separated plugin sys_ids, IDs, or names. |
| `store_apps` | No | Comma-separated Store app sys_ids, scopes, or names. |

### Get Credential

Returns the sys_ids of the mapped credential and MID Server for a repository URL. It does not return the credential secret.

```http
GET /api/x_snc_deployer/target/get_credential?repo_url=https%3A%2F%2Fgithub.com%2Facme%2Fmy-app.git
```

| Parameter | Required | Description |
| --- | --- | --- |
| `repo_url` | Yes | Repository URL to match against target-side Repository Credential records. |

### Get Transactions

Returns active transactions on the target and accepts no documented query parameters.

```http
GET /api/x_snc_deployer/target/get_transactions
```

### Run Installation Scripts

Runs `AppInstall()` for an application and its dependencies.

```http
GET /api/x_snc_deployer/target/run_install_scripts?application_sys_id=x_acme_app&stop_on_error=true&async=false
```

| Parameter | Required | Description |
| --- | --- | --- |
| `application_sys_id` | Yes | Application sys_id or scope. The parameter name is historical and misleading because scopes are accepted. |
| `stop_on_error` | No | Stop when an installation script fails. Example: `true`. |
| `async` | No | Run asynchronously. Example: `false`. |

## Integration pattern

1. Validate inputs in the external pipeline.
2. URL-encode every query parameter.
3. Call the Controller API using a dedicated credential.
4. Store the response and ServiceNow record identifiers in the pipeline run.
5. Monitor the resulting RAS, Deployment, and Deployment Task records.
6. Apply a finite timeout and idempotency strategy in the caller.
7. On an ambiguous timeout, look up existing work before sending the request again.
8. Fail the pipeline when Deployer fails, is cancelled, or requires unresolved manual work.

## Error checklist

- `401`: authentication failed or the user is inactive.
- `403`: the explicit API role or an endpoint ACL is missing.
- `404`: wrong instance, base path, or inactive/missing API definition.
- Parameter error: verify names, URL encoding, repository mapping, semantic version, and target name.
- Accepted but no progress: inspect controller logs and background work, then check target reachability.

The exact JSON response schema is not formally versioned in the repository. Capture representative responses in integration tests and validate required fields defensively.
