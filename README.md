# ServiceNow Deployer

ServiceNow Deployer coordinates source-control-based application deployments across ServiceNow instances. It resolves application dependencies, installs required plugins and Store apps, deploys custom applications in dependency order, and can run installation scripts, Instance Scan checks, and ATF suites.

This repository contains Deployer **1.26.0** in ServiceNow source-control export format. Do not manually edit files inside `394148f9db1c2d906a03dad1f396197a/`; ServiceNow generated those files and validates them using the included checksum.

## Documentation

Start with the [documentation home](docs/README.md), or jump directly to:

- [Getting started](docs/getting-started.md)
- [Installation](docs/installation.md)
- [Configuration](docs/configuration.md)
- [Run a deployment](docs/deployments.md)
- [Headless REST API](docs/headless-api.md)
- [Troubleshooting](docs/troubleshooting.md)
- [Reference](docs/reference.md)

## Important

Deployer is an open-source application, not an officially supported ServiceNow product. Test installation and deployment procedures in a non-production environment before using them in production.

## Project links

- [Upstream project](https://github.com/saschawildgrube/servicenow-deployer)
- [Original project manual](https://www.wildgrube.com/servicenow-deployer)
- [DevTools dependency](https://github.com/saschawildgrube/servicenow-devtools)
- [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0)
