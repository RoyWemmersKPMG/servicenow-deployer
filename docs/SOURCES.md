# Sources and documentation policy

This knowledge base was rewritten for task-oriented use from two primary sources:

1. The ServiceNow application metadata in this repository, version 1.26.0. This is the authority for shipped tables, choices, properties, roles, API paths, UI actions, and application version.
2. The [upstream Deployer manual](https://www.wildgrube.com/servicenow-deployer), used for installation workflow, architecture narrative, behavior, release notes, and documented limitations.

Supporting links:

- [Deployer upstream repository](https://github.com/saschawildgrube/servicenow-deployer)
- [DevTools upstream repository](https://github.com/saschawildgrube/servicenow-devtools)
- [DevTools manual](https://www.wildgrube.com/servicenow-devtools/)
- [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0)

## Precedence

When sources differ:

1. shipped 1.26.0 metadata describes what this repository contains;
2. upstream release notes describe intended version-specific behavior;
3. narrative manual text explains operating intent;
4. behavior verified on a supported non-production ServiceNow instance is the final operational check.

The wiki explicitly calls out known ambiguity instead of inventing behavior. For example, metadata around the Deployment **Mark as Complete** description is inconsistent, and the REST response schema is not independently versioned.

## Scope

This wiki explains installing, configuring, securing, operating, automating, troubleshooting, upgrading, and maintaining Deployer. It does not reproduce every internal Script Include function or every historical release note. The generated application XML remains the definitive low-level artifact.

## Keeping the wiki current

For each Deployer release:

- update the documented version and compatibility baseline;
- compare tables, dictionary choices, properties, roles, APIs, modules, and UI actions;
- review new release notes and technical debt;
- retest installation and a representative deployment;
- keep historical advice out of current procedures unless it still applies;
- add newly discovered operational behavior only after non-production verification.
