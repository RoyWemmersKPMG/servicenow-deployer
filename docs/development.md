# Development and repository maintenance

## Repository layout

```text
.
├── 394148f9db1c2d906a03dad1f396197a/  # generated ServiceNow application export
│   ├── author_elective_update/
│   ├── dictionary/
│   ├── update/
│   ├── checksum.txt
│   ├── README.md
│   └── sys_app_394148f9db1c2d906a03dad1f396197a.xml
├── docs/                                 # hand-maintained knowledge base
├── README.md                             # repository landing page
└── sn_source_control.properties
```

## Generated-file rule

Do not manually edit files inside the application export directory. ServiceNow generated these files and checks them against `checksum.txt`. Hand edits, conflict resolution that combines different snapshots, or commits containing files from inconsistent revisions can make Studio import fail.

Make application changes in ServiceNow Studio, commit them through the supported source-control workflow, and let ServiceNow regenerate the export.

The root `README.md` and `docs/` are deliberately outside the application export and can be maintained as normal Markdown.

## Working with branches

- Use a development branch such as `dev` for ongoing application work.
- Baseline release branches using exact three-part semantic versions such as `1.26.0`.
- Do not add prefixes such as `v` if the branch must be deployable by Deployer.
- Keep one coherent ServiceNow-generated snapshot per commit.
- Avoid merging generated XML by hand. Regenerate from the intended ServiceNow state instead.

## Recovering an invalid generated snapshot

The generated README describes two recovery patterns:

1. Revert the commits that introduced incompatible generated files, working backward through history.
2. Start from a known-good generated snapshot, then create a new commit containing that coherent snapshot.

Do not use destructive Git commands on an unreviewed working tree. Preserve unrelated user changes and verify the exact commit and paths before recovery.

## Documentation contribution style

- Lead with the task and expected outcome.
- Use ServiceNow labels exactly as shown in the application.
- Put internal names in backticks.
- Separate normal procedures from exceptions.
- Include security impact when documenting roles, credentials, deletions, skipped tests, or downgrade behavior.
- Update [Reference](reference.md) when metadata choices, roles, properties, APIs, tables, or UI actions change.
- Update [Release and upgrade](release-and-upgrade.md) when compatibility or known limitations change.
- Verify every relative link before committing.

## Review checklist

- Application export directory was not manually changed.
- No credentials, tokens, instance secrets, or private URLs are present.
- Version and dependency requirements match application metadata.
- Procedures were tested on a non-production instance where behavior changed.
- REST examples use placeholders and URL encoding.
- New pages are linked from `docs/README.md` and `_Sidebar.md`.
- Markdown links resolve on a case-sensitive host.

## Application testing

The repository contains ServiceNow ATF records rather than a local executable test harness. Run the **Deployer** ATF suite in a non-production ServiceNow instance after application changes. Also exercise the specific end-to-end path affected by the change, including controller, target, repository provider, and MID Server when relevant.
