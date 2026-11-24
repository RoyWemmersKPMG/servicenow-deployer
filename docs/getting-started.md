# Getting started

## What Deployer does

Deployer turns a version branch in source control into an ordered deployment plan for a ServiceNow target instance. It can:

- discover direct and indirect application dependencies;
- verify that the requested versions exist;
- install or update required plugins and Store apps;
- import a new custom application from source control;
- switch an installed application to a version branch;
- apply remote changes when an installed branch needs refreshing;
- run application installation scripts;
- run CodeSanity checks through Instance Scan;
- run each application's default ATF test suite;
- pause for optional manual activities and data-deletion confirmation;
- show application versions across a pipeline of instances.

## The three instance roles

An instance can serve one or more roles, but keeping the responsibilities clear makes configuration easier.

| Role | Purpose |
| --- | --- |
| Development instance | Developers change applications and create version branches. Deployer can protect new version branches when DevTools baselines an application. |
| Controller instance | Stores repository mappings and target definitions, evaluates dependencies, creates deployment tasks, and coordinates deployments. |
| Target instance | Receives applications. Its Deployer Target API performs source-control operations, install scripts, status checks, Instance Scan, and ATF work. |

## Before you begin

You need:

- a ServiceNow instance on a supported release;
- administrative access for installation;
- DevTools 1.146.0 installed first;
- a fork of the Deployer repository;
- source-control credentials that can read the required repositories;
- a ServiceNow credential for each controller-to-target connection;
- network access directly from the instance or through a MID Server;
- a non-production target for the first end-to-end test.

If ATF is part of your deployment policy, set `sn_atf.runner.enabled` to `true` on each target that will execute ATF.

## Fastest path to a first deployment

1. Install DevTools and Deployer on one controller and one non-production target. See [Installation](installation.md).
2. On the target, create a technical user with `x_snc_deployer.api_target`; add the additional roles required by the target operations you intend to run.
3. On the controller, create a ServiceNow credential for that technical user.
4. Configure a Repository Credential mapping for your provider and repository URL prefix.
5. Create a Target Instance record and select a conservative data-deletion policy, preferably **Deny** for the first run.
6. Use **Validate** on the Target Instance record.
7. Use **Update Local CMDB** to load the target's installed-application inventory.
8. Create a Repository Application Status in **Deployment** mode for a semantic version branch such as `1.2.0`.
9. Confirm that its status becomes **Ready for Deployment** and inspect the dependency tree with **View**.
10. Select **Create Deployment**, review the generated tasks, then **Start**.

## Conventions that matter

- Deployable version branches must use exactly three semantic-version components, for example `1.4.2`.
- Repository URLs must match an active Repository Credential mapping.
- Dependencies install deepest-first; the requested application installs last.
- A deployment cannot start from a failed Repository Application Status.
- The target's data-deletion policy determines whether destructive schema differences stop, pause, or only warn during deployment.
- Production deserves separate credentials, strict role assignment, and an explicit review of any skipped checks.

## Next step

Read [Architecture](architecture.md) if you want the mental model, or go directly to [Installation](installation.md).
