> [!Note]
> W.I.P.
> This is a spin-off of https://github.com/lkretschmer/deepl-go/pull/7

# go-openapi-coverage

`go-openapi-coverage` is a tool to check whether a Go client library fully implements
all endpoints defined in an [OpenAPI specification](https://spec.openapis.org/oas/latest.html).

It analyzes an OpenAPI spec (YAML/JSON) and Go source code, then reports which API
operations are implemented and which are missing.

---

## Motivation

OpenAPI generators can create Go client stubs from a specification, but **they do not
guarantee that a hand-written or evolving client implementation stays in sync with
the official spec**.

In practice:

- OpenAPI specs evolve
- Client libraries lag behind
- Missing endpoints are often unnoticed until users report them

This tool addresses that gap by answering a simple question:

> **Does this Go client actually cover the OpenAPI spec?**

---

## What This Tool Does

- Parses an OpenAPI specification (v3, YAML or JSON)
- Extracts all defined API operations
- Parses Go source code using AST analysis
- Detects exported client methods
- Compares spec operations with Go implementations
- Reports:
  - Implemented endpoints
  - Missing endpoints
  - Coverage summary

This is **not** a spec-to-spec diff tool.
It is a **spec-to-implementation coverage checker**.

---

## Features

- [ ] OpenAPI v3 support (YAML / JSON)
- [ ] Static analysis using Go AST (no reflection, no execution)
- [ ] Language-agnostic OpenAPI parsing
- [ ] Clear coverage reports
- [ ] CI-friendly (non-zero exit code on missing endpoints)

---

## Example Output

```text
OpenAPI coverage report

Total operations:     25
Implemented:          18
Missing:              7
Coverage:             72%
```

Optionally, a detailed list of missing operations can be generated.

## Installation

```sh
go install github.com/yourname/go-openapi-coverage/cmd/openapi-coverage@latest
```

## Usage

```sh
openapi-coverage \
  --spec openapi.yaml \
  --pkg ./client
```

Typical use cases:

- Validate third-party Go clients against official OpenAPI specs
- Prevent missing endpoints in CI
- Track client coverage as APIs evolve

## How Matching Works

The tool uses **static analysis**:

- OpenAPI operations are identified from the spec
- Go source code is parsed using `go/parser` and AST inspection
- Exported methods are collected from the target package
- Operations and methods are matched using configurable rules (e.g. operationId, naming conventions)

No code execution is required.

## Non-Goals

- Will not generate client code
- Will not compare OpenAPI specs with each other
- Will not runtime API testing

This tool focuses exclusively on **coverage verification**.

## Use Cases/Target Users

- API client maintainers
- OSS reviewers
- CI pipelines
- Spec-driven development workflows

## Status

> [!Note]
> This project is in early development. The API and matching rules may evolve.

Feedback and contributions are welcome.

## License

- MIT
- Authors: KEINOS and the Contributors

