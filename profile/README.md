<p align="center">
  <img src="images/tessera-banner.png" alt="Tessera BI" width="800">
</p>

<h2 align="center">Tessera BI</h2>

<p align="center">
  <strong>Native business intelligence, built inside NetSuite.</strong><br>
  No middleware. No external database. No additional authentication.
</p>

<p align="center">
  <a href="https://github.com/TesseraBI/tessera-docs/actions/workflows/docs-ci.yml">
    <img src="https://github.com/TesseraBI/tessera-docs/actions/workflows/docs-ci.yml/badge.svg" alt="Docs CI">
  </a>
  <img src="https://img.shields.io/badge/SuiteScript-2.1-red" alt="SuiteScript 2.1">
  <img src="https://img.shields.io/badge/Deployment-SDF-red" alt="SDF">
  <img src="https://img.shields.io/badge/Language-TypeScript-3178c6" alt="TypeScript">
  <img src="https://img.shields.io/badge/Tests-240%20passing-brightgreen" alt="240 passing tests">
  <img src="https://img.shields.io/badge/License-Commercial-blue" alt="Commercial">
</p>

---

## What "native" means

Tessera runs entirely within your NetSuite account. Every query, every computation, and every rendering decision happens inside the platform using your existing data and your existing session. There is no ETL pipeline, no external database, no data warehouse, and no additional login to manage.

| Layer | Technology |
|-------|-----------|
| Runtime | SuiteScript 2.1 |
| Deployment | SuiteCloud Development Framework (SDF) |
| Data access | SuiteQL (`N/query`) + Saved Searches (`N/search`) |
| Visualization | Apache ECharts — bundled, served from File Cabinet |
| Math & statistics | math.js — bundled, served from File Cabinet |
| Tabular data | AG Grid Community Edition |
| Language | TypeScript (strict mode) |
| Authentication | NetSuite native session — no configuration required |
| External runtime dependencies | None |

## Architecture

The dominant pattern is **CQRS** — the read path and write path are fully separated. Dashboard portlets read pre-computed Metric Result records. They never touch the computation engine at render time. The computation engine — the Computed Result Layer (CRL) — runs as scheduled Map/Reduce scripts and writes results once. Portlets read those results.

This separation is what makes accounting-grade correctness achievable. A portlet that live-queries NetSuite at render time cannot guarantee the result is the same number the Controller approved. A portlet reading a pre-computed, version-linked, timestamped result record can make that guarantee unconditionally.

The semantic layer — the Metric Definition Layer (MDL) — comprises 17 custom record types governing metric definitions, query components, filter scope, dimension scope, versioning, governance, and benchmarks. Every metric definition is versioned and immutable once approved. Every result record is permanently associated with the definition version that produced it.

## Development

**Type-safe.** TypeScript strict mode throughout. Two tsconfigs: AMD for SDF deployment, CommonJS for the Node.js test runner.

**Tested.** 240 passing unit tests using `@oracle/suitecloud-unit-testing` — Oracle's published Jest-based SuiteScript testing framework. Tests run in Node.js with full NS module mocking. No sandbox required to run the test suite.

**Design-first.** All 17 custom record types, all query templates, the six-step Metric Wizard UX, the portlet rendering model, the security role model, and the CRL Map/Reduce architecture were fully designed and documented before production coding began. Approximately 75–88 hours of pre-production design work is version-controlled in the repository from day one.

**CI on every commit.** TypeScript compilation (both configs) and the full test suite run on every push via GitHub Actions.

**SDF-native deployment.** Every NetSuite object is defined as SDF XML. Installation is deterministic — no manual configuration steps.

## SuiteCloud compliance

Tessera uses only APIs published by Oracle for SuiteCloud partners. No restricted APIs. No undocumented internal namespaces. Planned for the BFN — AI Application badge (due to `N/llm` use for metric narration).

## Repositories

| Repository | Description | Access |
|-----------|-------------|--------|
| [tessera-docs](https://github.com/TesseraBI/tessera-docs) | Architecture, security model, SuiteCloud compliance, data layer reference, integrator guide | Public |
| tessera-core | Product source | Private — SDN review on request |

## Get in touch

**[tesserabi.com](https://www.tesserabi.com)** · Early access · SDN and integration inquiries

*NetSuite is a registered trademark of Oracle Corporation. Tessera is an independent product and is not affiliated with, endorsed by, or sponsored by Oracle.*
