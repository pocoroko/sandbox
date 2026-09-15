---
id: 'standards-faq'
type: 'guidance'
status: 'draft'
version: '0.1'
owner: 'Architect'
authors: []
created: '2026-09-11'
updated: '2026-09-11'
scope: 'Common application scenarios and answers for documents in this repository.'
---

# Frequently Asked Questions

## Architecture

### Service starts with one module

**Situation:** A new service has one business module, but additional modules may be added later.

**Use:** Follow the [Modular Monolith Service Design](architecture/modular-monolith.md) with compact composition and one module under `Modules/`.

```text
src/
├── TBIBank.<Service>/
│   └── Modules/
│       └── <Module>/
│           ├── Contracts/
│           ├── Domain/
│           ├── Application/
│           └── Infrastructure/
├── TBIBank.<Service>.<Adapter>/
└── TBIBank.<Service>.Host/
```

Create `Contracts/` only for contracts consumed outside the module.
Do not create empty future modules or separate projects in anticipation of growth.
When another business capability appears, add it as a sibling module and define its code, contract, dependency, and data boundaries.
The service's `docs/architecture.md` records that the service currently contains one module.
An ADR is unnecessary unless the service makes a significant architectural decision, such as selecting project-isolated composition.
