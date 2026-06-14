---
paths:
  - "**/*.go"
  - "**/go.mod"
  - "**/go.sum"
---
# Go Coding Style

> This file extends [common/coding-style.md](../common/coding-style.md) with Go specific content.

## Formatting

- **gofmt** and **goimports** are mandatory — no style debates

## Modern Go (target 1.22+)

- Prefer stdlib: `slices`, `maps`, `cmp`, `errors.Join`, built-in `min`/`max`, `range` over int
- `any`, not `interface{}`
- Never start a goroutine without a clear stop (context cancel or closed channel)

## Reference

For idioms — accept interfaces/return structs, small interfaces, error wrapping with `%w` — see skill: `go`.
