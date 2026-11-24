# Glossary

**Application baseline**  
The DevTools procedure that creates a versioned application branch. Deployer can hook into completion and protect the new branch on supported providers.

**ATF**  
ServiceNow Automated Test Framework. Deployer can run the default ATF suite of each installed custom application.

**Checkout mode**  
RAS mode that reads the same branch across an application and its dependencies without strict dependency-version compatibility. Intended for development instance initialization or reset.

**CodeSanity**  
The project's name for Instance Scan checks run against an application during deployment.

**Controller instance**  
The ServiceNow instance that stores configuration, evaluates repositories and dependencies, creates deployment records, and coordinates target operations.

**Data Deletion Policy**  
Target setting—Deny, Confirm, or Warn—that controls how detected removal of application tables or columns affects deployment.

**Deployment**  
Task-derived record representing the rollout of one repository application/version and its dependencies to one target.

**Deployment Task**  
One ordered automatic or manual unit of work within a Deployment.

**Development instance**  
Instance used for application development and baselining.

**DevTools**  
Required companion application that supplies dependency, installation, source-control, CMDB, documentation, and extension functionality used by Deployer.

**Headless deployment**  
A deployment started through the Controller API rather than through the ServiceNow UI.

**Local CMDB**  
Controller-side inventory of applications and versions discovered on target instances and in repositories.

**MID Server**  
ServiceNow component that can route controller-to-target and source-control traffic when direct network access is unavailable.

**Pipeline**  
Ordered group of target instances used to visualize application version progression and instance readiness.

**Preview mode**  
RAS mode used to evaluate a branch before release. Its result cannot directly create a deployment.

**Repository Application Status (RAS)**  
Preflight record containing repository, branch/version, dependency tree, processing state, and errors.

**Repository Credential**  
Mapping from a repository URL prefix to provider type, ServiceNow credential, optional API URL, and optional MID Server.

**Reverse dependency order**  
Deepest dependencies first and requested application last. Used for installation, install scripts, and testing.

**Target instance**  
Instance receiving applications. It exposes the Deployer Target API and executes local source-control, installation, scan, and test work.

**Technical user**  
Non-human account dedicated to an API integration, with explicit API and operation roles.
