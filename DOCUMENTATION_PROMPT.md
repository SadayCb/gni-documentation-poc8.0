# PL/SQL API Documentation Generation Standard

Generate professional GitHub/MkDocs-compatible documentation from a supplied Oracle package **Specification and Body**.

## Mandatory validation

1. Inventory every active public procedure/function **from the Specification**, including overloads as separate declarations. Ignore commented-out declarations.
2. Match each method to its active body implementation. Skip commented-out bodies. Never assign a private procedure's error to a public overload without tracing a verified call path.
3. Preserve exact source signatures, including default expressions, parameter order, return types and spelling.
4. Use source-supported facts only. Label unknown semantics as `Not determined from source` rather than inventing conditions.
5. Build unique explicit anchors and test every relative Markdown file link and anchor.
6. Maintain one consistent verified inventory total in README, overview, and validation report.

## Required structure

```text
README.md
mkdocs.yml
docs/
  index.md
  api.md
  workflows.md
  data-model.md
  dependencies.md
  error-handling.md
```

## API structure

Group public methods by verified functional domains; under each domain list **Procedures** and **Functions** separately. Each domain needs a one-sentence responsibility description.

For each method include **Purpose**, exact **Signature**, **Parameters** (business meaning where evidenced), **Business Logic**, **Business Rules**, **Dependencies**, and **Errors**. Functions additionally need **Returns**. Keep descriptions concise yet meaningful; neither `WHEN OTHERS` nor PL/SQL `IF` syntax is a business rule. Do not generate filler such as “Supplied p_x input” or “Performs method x”. Avoid copied SQL blocks, generic exception boilerplate and meaningless usage templates. Describe relevant COMMIT and rollback effects where source confirms them.

## Error reference

Only document meaningful validated errors, grouped into business validation, configuration/data lookup and operation/processing. Each row must identify the **correct method or overload**, triggering condition, business impact and observed handling or corrective action. Do not substitute incomplete message fragments for error descriptions. Explicitly distinguish direct errors from those arising in private called routines. Do not infer an Oracle error number from `R(...)` without its implementation.

## Workflows, model, dependencies

Provide 2–5 source-confirmed workflow diagrams, each with entry-point links, meaningful business conditions and outcomes. Never invent EOD/SOD execution ordering.

Data Model must report verified read/write access and relevant method links; observed joins are not proof of foreign keys. Dependencies list significant direct packages and external integrations without repeating every table.

## Deliverable checks

Report public inventory count, overload coverage, broken-link count, source limitations and any unverified mappings. Documentation must be readable in GitHub and MkDocs. Maintain relative links from `docs/` and keep `docs/index.md` as the MkDocs landing page.
