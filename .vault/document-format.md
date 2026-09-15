# Document Format

See the [README](../README.md) for the repository overview and index, the [contribution guide](../CONTRIBUTING.md) for workflows, and the [glossary](../CONTEXT.md) for shared vocabulary.

Use UTF-8 Markdown with YAML front matter for metadata and a body organized by document type.
Use the [repository path convention](../CONTRIBUTING.md#path-and-filename-convention): a descriptive lowercase `kebab-case.md` filename that names the subject or purpose without repeating document type, version, or status.
The path stays stable as the document evolves.
This format applies to visions, standards, designs, guidance, checklists, and catalogs.
Repository instructions, the README, playbook landing page, this format reference, the glossary, and review records have their own purposes and do not need the full header.

## Required Fields

Visions, standards, designs, guidance, checklists, and catalogs MUST include the fields below.

| Field | Meaning |
| --- | --- |
| `id` | Stable document identifier, unique in this repository; retain it through renames and revisions. |
| `type` | `vision`, `standard`, `design`, `guidance`, `checklist`, or `catalog`. |
| `status` | `draft`, `proposed`, `approved`, or `retired`. Use this field instead of a separate `state`. |
| `version` | Quoted release label, such as `"0.9"`; identifies a content version, independently of approval status. |
| `owner` | One accountable team or role responsible for keeping the document current. |
| `authors` | Named authors of the document; authorship does not confer ownership or approval authority. |
| `created` | Original document creation date, in `YYYY-MM-DD` format. |
| `updated` | Date the content was last changed, in `YYYY-MM-DD` format. |
| `scope` | Short statement of the systems or practices covered; expand boundaries in the body when needed. |
| Body | Purpose, applicability, and the content appropriate to the document type. |

Use lowercase values for `type` and `status` in YAML. Prose and index labels may use title case. Use separate creation and update dates instead of an ambiguous `date` field. Record real values; placeholders are acceptable only while drafting.
In Draft documents, use `null` for unknown scalar metadata and `[]` for unknown authors. Resolve missing metadata before Proposed status; never infer original authorship or creation dates from an import commit.
Git retains the detailed contributor and edit history, so do not maintain a second commit-by-commit changelog in every document.
Choose a new version label for each approved content revision; do not reuse an approved label for different body content. Git commits distinguish intermediate edits to the same candidate version.

## Conditional Fields

| Field | When needed |
| --- | --- |
| `approval` | Required for Approved status: `by`, `date`, and `reference` identifying the exact reviewed revision and its approval evidence. |
| `effective_date` | When requirements start applying after approval. The transition section identifies the applicable previous revision until then. |
| `supersedes` | When this document replaces a different document; identify and link the previous document. |
| `replaced_by` | When retired with a replacement; identify and link the replacement. |
| `retirement` | Required for Retired status: `by`, `date`, `reason`, and `reference` to the authorized retirement decision. Retain earlier approval evidence if present. |
| `legacy_status` | For migrated documents: preserves the original status text as provenance, without granting approval. |

Use `YYYY-MM-DD` for approval, effective, and retirement dates. Omit unused conditional fields instead of leaving empty placeholders.
The approval reference points to an already reviewed commit or review record; do not try to embed the final publishing commit's own hash in its contents.
Starting a revision of an approved document changes the candidate status to Draft and removes its inherited approval block. The applicable revision retains its approval on the default branch.

## Standard Template

```markdown
---
id: "<stable-document-id>"
type: standard
status: draft
version: "0.1"
owner: "<accountable team or role>"
authors:
  - "<author name>"
created: "YYYY-MM-DD"
updated: "YYYY-MM-DD"
scope: "<systems or practices covered>"
---

## Purpose

<Problem addressed and intended outcome.>

## Applicability

<Boundaries, exclusions, and conditions beyond the short scope statement.>

## Requirements

### REQ-001 — <Requirement title>

<Observable rule using MUST, SHOULD, or MAY.>

**Rationale:** <Why this rule exists.>

**Verification:** <How a reviewer checks the rule and what evidence is needed.>

## References

<Links to governing standards, related documents, and source specifications.>
```

Requirement IDs are stable within a standard. Reference them together with the document ID and target revision; do not renumber or reuse retired IDs.

Add examples when they clarify a rule, and a transition section when a revision changes what existing systems must do. Keep examples visibly separate from normative requirements.

### Grouped requirements and child IDs

A parent such as `REQ-003` may group related controls. Give independently assessable obligations stable child IDs such as `REQ-003-01`, using level-four headings that can be linked directly.
A child identifies one control decision or outcome; required fields of one record or parameters of one validation may remain together. Separate obligations that can be independently satisfied or excepted.
Keep the existing parent ID/title for navigation and grouped guidance. Shared rationale and verification scenarios may sit under the parent; each child's evidence maps its own checks and result.
Allocate new child IDs without renumbering existing children. Retire obsolete IDs without reusing them; a further split records its successor IDs and requires an explicit evidence/exception mapping.
Control references contain the document ID, exact adopted revision, and child ID where defined. For example: `security-baseline / REQ-003-03 / <adopted-revision>`; the revision placeholder is replaced in an actual record.
Evidence and exception scope follow [Technology Governance REQ-003-02](../governance/technology.md#req-003-02--control-references) and [REQ-004-01](../governance/technology.md#req-004-01--exception-scope-and-record).

## Bodies for Other Document Types

Keep the shared header, purpose, and applicability. Replace the Requirements section with content appropriate to the type:

| Type | Body content |
| --- | --- |
| Vision | Problem, desired outcomes, scope, proposed flow with examples, open questions, and success criteria; link resulting Designs or Standards when available. |
| Design | Solution scope, structure, behaviour, responsibilities, constraints, acceptance criteria, and links to governing standards. |
| Guidance | Steps or explanations, worked examples when useful, and links to the requirements being explained. |
| Checklist | Checks, expected evidence, and a source requirement reference for each check. |
| Catalog | Entries with stable identity, owner, lifecycle, and links to canonical artifacts or governing standards; add fields required by that catalog's subject. |

A catalog's document status describes approval of the document; each entry's lifecycle describes the listed technology or contract. They are separate concepts.

## Practical Guidance and Usability Review

For Guidance that helps select or implement an engineering pattern, use the following content. Keep small explanations small; a Checklist can link to the pattern and verify its evidence without repeating the design.

| Content | What the reader can determine |
| --- | --- |
| Scenario and prerequisites | Which callers, operations, platforms and constraints the recommendation fits, using facts that can be checked. |
| Recommendation and reason | The preferred approach for that scenario and the governing Standard/Design that supports it. A list of options alone is insufficient. |
| Alternatives and trade-offs | When another approach fits better, and its effect on security, complexity, credentials, latency, availability and operating ownership as applicable. |
| Complete flow | Which component performs each decision/action, what evidence crosses each boundary, and how success, denial, partial completion and dependency failure behave. |
| Worked example | A coherent illustrative record/configuration and expected results. Mark fictional values and distinguish examples from approved corporate profiles. |
| Remaining inputs | Separate facts engineers can verify or measure from decisions requiring an accountable owner. Identify their sources/authority and the consequence of leaving them unknown; do not invent approvals, retention, identities or policy values. |
| Verification | Representative allowed, denied and failure scenarios mapped to source controls, with observable results. |

Keep mandatory rules in the Standard and shared solution choices in the canonical Design. Guidance links to and explains them; examples do not become a second source of requirements.
A recommendation such as a credential preference must not silently decide a different concern such as caller attribution, resource permission or evidence durability.

During review, apply the guidance to contrasting scenarios with explicit inputs. Check that a reader can select and justify a complete pattern, identify the cost of an alternative, and isolate only remaining facts.
Include a scenario where a prerequisite fails and one where a dependency fails after work starts. The expected result may be a documented unresolved decision rather than a fabricated default.
An engineer or agent should not need to ask the user to repeat existing rules or choose between unexplained mechanisms.
Use the [service design review workflow](../PLAYBOOKS/workflows/service-design-review.md) to demonstrate which decisions a playbook resolves from evidence and which owner inputs remain.
Review combined patterns across the same request, data and recovery flow; separately plausible recommendations may have incompatible prerequisites.

Record which scenarios were exercised and any unresolved outcomes in the review evidence. Metadata, Markdown lint and working links validate document structure; scenario review validates the recommendation's usability.
Document approval and production verification retain their separate meanings.

## Review Record Format

Use a title and a link to the reviewed document, followed by one heading per comment. Each comment carries its own target revision because a review file can span several document versions.

```markdown
# <Document title> — Review

Document: <relative link to the document>

## C-001 — <Concern title>

Author: <name>
Created: YYYY-MM-DD
Target: <document ID>, <Git commit>, <requirement ID or section heading>
State: Open

Concern: <Problem, evidence, and impact.>
Suggested change: <Proposed resolution, if known.>
```

On assessment, record the decision, decision-maker, date, and reason. Deferred comments add a revisit date or condition; Applied comments link the resulting commit. Preserve earlier decisions when reassessing a comment.

## Markdown Validation

Run markdownlint from the repository root with the shared [configuration](.markdownlint-cli2.jsonc):

```sh
markdownlint-cli2 --config .docs/.markdownlint-cli2.jsonc README.md CONTEXT.md .docs/document-format.md
```

Replace the file arguments with the documents being checked. Always pass `--config .docs/.markdownlint-cli2.jsonc`; a plain invocation from the root does not automatically use this nested configuration for root or subject-area documents.
