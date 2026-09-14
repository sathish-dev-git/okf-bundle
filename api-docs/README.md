# api-docs - API source documents (input)

The only place where API facts are authored. Everything here is read by `tools/build_okf.py`; nothing
here is published directly. This folder is the export of the API documentation system, kept in the
layout the exporter produces.

## Expected layout

```
api-docs/
├── MD/
│   └── <NN · Domain Title>/<GROUP_NAME>.md         narrative reference, one file per API group
├── ZENESIS_OAS/
│   └── <domain>-grouped-api.json                    OpenAPI 3, one file per domain
├── ZENESIS_OAS_SAMPLES/
│   └── <domain>-grouped-api-samples.json            SDK snippets keyed by path + method
└── zoho-analytics-api-common.json                   OAuth scopes, Error schema, common responses
```

The builder does **not** discover files: the `DOMAINS` table in `tools/build_okf.py` lists every
domain folder, markdown file stem and OpenAPI file. A new file that is not in `DOMAINS` is ignored
(see the "Add a new API group or domain" playbook).

## Rules when replacing or adding content

- **Provide the complete set, not a delta.** The build deletes and regenerates the whole bundle from
  what is present here. If only the changed documents are present, the bundle will contain only those.
- A markdown endpoint section (`## N. Title`) and its OpenAPI operation are joined **by title**
  (`x-zenesis-title`, else `summary`). If they differ, the builder prints a `WARN`; fix the title or add
  a `TITLE_MAP` entry.
- Keep the OpenAPI and samples files valid JSON.
- Do not add anything here that is not an API source document.

Format details: [../agent-guide/03-source-document-format.md](../agent-guide/03-source-document-format.md).
Procedures per kind of change: [../agent-guide/04-change-playbooks.md](../agent-guide/04-change-playbooks.md).
