# AI Agent Instructions

## General overview and information
- Stream/topic: `Payment (AIS/PSS)`  
- Repo: `swissfintechinnovations/ca-payment` · OpenAPI version: `3.0.0`  
- Bundled spec at repo root: `paymentAPI.yaml, accountAPI.yaml + notification APIs`  
- Repo with config files, reusable workflows, and wiki: `swissfintechinnovations/.github`  

## What you are allowed to edit
- **Edit only** the split source components under `src/components/{schemas,parameters,headers,responses,...}`.
- **Do not touch** the bundled root file — it is generated from `src/*` by the Redocly bundle workflow on PR. Editing it directly will be overwritten.
- **Do not touch** anything in `.github/` or the reusable workflows in `swissfintechinnovations/.github`.

## Editing rules and patterns
- Only do small, focused changes, not large refactors.
- Prefer optional additive changes over modifying existing semantics.
- When adding or renaming schemas/parameters: update the file in `src/components/...` and ensure any file-based `$ref:` (e.g. `./components/schemas/Foo.yaml`) matches the new path.
- Expect bundling to rewrite refs.
- Error responses follow RFC7807: `application/problem+json` and use `src/components/responses/standard400.yaml` and `standard500.yaml`; include headers like `X-Correlation-ID` and `Content-Language` where applicable.
- Header/param conventions: client/correlation/agent headers are defined under `src/components/parameters/header` (e.g. `client.yaml`, `correlation.yaml`, `agent.yaml`) and should be referenced consistently.

## Naming convention & style guide
The full rules are defined in the `swissfintechinnovations/.github` wiki. The following rules provide a comprehensive summary.

### Style guide
- Do not refactor established structures purely for stylistic reasons.
- Use camelCase for property names.
- Use PascalCase for schema/type names.
- Keep enum values readable, stable, consistently formatted and explicitly documented.
- Name boolean fields as affirmative statements (e.g. `isActive`, `hasConsent`).
- Reuse existing error structures and response patterns consistently.
- When uncertain, align with existing patterns already used in this repository before introducing new conventions.

### Naming conventions
- Preserve semantic compatibility even when improving documentation or naming.
- Prefer clear, business domain oriented resource names.
- Prefer singular names for schema objects and plural names for collections/endpoints.
- Keep names concise, explicit, and stable over time.
- Avoid abbreviations unless they are established domain terms.
- Use consistent terminology across endpoints, schemas, examples, and documentation.
- Reuse existing domain vocabulary already present in the repository.
- Add meaningful descriptions and examples for all public models and fields.

## Schema Design Guidelines
- Preserve backward compatibility whenever possible. Do not introduce breaking API changes without explicit versioning discussion.
- Keep schemas reusable and avoid duplication.

## PR review checklist for agents
- All new schemas/params have descriptive titles and follow the naming pattern where applicable (see .github wiki)
    - Style Guide: [wiki page](https://github.com/swissfintechinnovations/.github/wiki/Style-Guide-Common-APIs)
    - Naming Conventions: [wiki page](https://github.com/swissfintechinnovations/.github/wiki/Naming-Conventions)
- Responses still include standard `400/500` responses and headers as applicable.
- `$ref` paths are correct for the file layout and remain valid after bundling (run the bundle command locally or via CI workflow to confirm).
    - bundle command:  `npx @redocly/cli bundle --config .github/redocly.yaml`
    - workflow: `SFTI Bundle`
- perform linter checks locally or via CI workflow and fix all errors and warnings
    - lint commands: `npx @redocly/cli lint --config=github/.github/redocly.yaml <<topic>>API.yaml`, `yamllint -d "{extends: github/.github/.yamllint, rules: {line-length: {max: 170}}}" -f github "<<file>>"`, `yamllint -c github/.github/.yamllint -f github "<<file>>"`
    - workflow: `SFTI Lint PRs`
- No breaking semantic changes to tags, paths, or required fields without a clear changelog entry.
