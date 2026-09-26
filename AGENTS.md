# Ferret

## Map and setup

`pkg/compiler/`, `pkg/parser/`, and `pkg/runtime/` implement FQL compilation, parsing, and execution. `pkg/drivers/` provides browser/data drivers; `pkg/stdlib/` provides query functions. `e2e/` contains the test CLI, pages, and query cases; `examples/` demonstrates embedding.

Use Go with the declared 1.24.2 toolchain (`go.mod` minimum 1.23). `go mod download` resolves dependencies. `make compile` builds `bin/ferret` from `e2e/cli.go`; this is the test harness, not evidence that the separately distributed CLI is installed. Preserve FQL behavior and driver contracts with focused tests.

## Verification and prerequisites

`make test` runs `go test ./pkg/...`; use an affected package for quick iteration. `make vet` and `make lint` provide static checks; lint requires staticcheck and revive, while `make fmt` requires goimports and rewrites files. `make install-tools` installs unpinned remote tools, so inspect prerequisites before using it.

Parser generation (`make generate`) requires Java and ANTLR 4.13.2 configured as in `.github/workflows/build.yml`. Edit grammar/generator sources rather than hand-patching generated parser code. `make e2e` requires the compiled harness, the Lab executable (`LAB_BIN`), and disposable Chromium exposing localhost:9222; it exercises fixture pages and needs visual/output inspection when driver behavior changes.

The default `make build` also generates code and runs tests. `make cover` uploads results via a remote script, so use `make test` for local-only unit verification. Don't copy CI's blanket Docker stop command into a shared host. Report local checks separately from browser integration and coverage uploads.

## Finishing work

Follow the nearby implementation and keep changes within the requested scope. Carry authorized changes through the relevant checks, fixing failures caused by the change. For a bug, reproduce the affected behavior and add a focused regression check when useful. Make routine reversible choices without another approval; ask only when missing information materially changes correctness, scope, or authorization, and name the exact source of any blocking rule. Report what changed, checks actually run, and concrete unverified behavior; repeat checks when new edits or evidence warrant it.
