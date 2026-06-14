---
paths:
  - "**/*.go"
  - "**/go.mod"
  - "**/go.sum"
---
# Go Testing

> This file extends [common/testing.md](../common/testing.md) with Go specific content.

## Commands

Always run with race detection; check coverage:

```bash
go test -race ./...
go test -cover ./...
```

## Reference

For testing patterns — table-driven tests, `t.Helper()`, fakes over heavy mocks, golden files in `testdata/`, `go-cmp` — see skill: `go`.
