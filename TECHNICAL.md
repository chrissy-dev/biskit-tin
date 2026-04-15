# Technical 

This tool is built in Go. I'm not a Go developer, but I feel Go is the right tool for this job. Single binary, cross-platform, fast, and boring in the best possible way. I'm building this spec-first and AI-assisted. The spec is the source of truth. The Go code is just what the spec became.

## Project Structure

```
biskit-tin/
├── cmd/
│    ├── root.go          -- entry point, global flags
│    ├── tin.go           -- tin management commands
│    ├── rebuild.go       -- rebuild command
│    ├── refresh.go       -- refresh command
│    └── status.go        -- status command
├── internal/
│    ├── archive/
│    │    ├── archive.go
│    │    └── archive_test.go
│    ├── registry/
│    │    ├── registry.go
│    │    └── registry_test.go
│    ├── thumbs/
│    │    ├── thumbs.go
│    │    └── thumbs_test.go
│    ├── html/
│    │    ├── html.go
│    │    └── html_test.go
│    └── scanner/
│         ├── scanner.go
│         └── scanner_test.go
├── templates/
│    ├── index.html.tmpl
│    └── style.css
├── .github/
│    └── workflows/
│         ├── ci.yml
│         └── release.yml
├── .goreleaser.yml
├── Makefile
├── SPEC.md
├── TECHNICAL.md
└── README.md
```

## Makefile

```makefile
make build      # build the binary locally
make test       # run all tests
make lint       # run golangci-lint
make release    # goreleaser snapshot build, no publish
make clean      # remove build artefacts
```

## Language & Runtime

- Go
- Single binary output, no runtime dependencies
- Cross-platform -- Windows, macOS, Linux
- No CGO -- pure Go only, keeps the binary self-contained

## CLI

- Cobra for command structure, flags, and help text
- Bubble Tea (Charm) for TUI -- progress bars, spinners, status output
- Lipgloss for styling TUI output
- The TUI is the only interface. There is no GUI.

## Image Processing

- `golang.org/x/image` for thumbnail generation -- part of the official Go extended stdlib
- Pure Go, no CGO
- Thumbnails are generated at a fixed maximum dimension, preserving aspect ratio
- Originals are never touched

## Tin Registry

- Stored as a single JSON config file in the user's OS config directory
- `os.UserConfigDir()` to locate it -- respects platform conventions
- Format: named entries mapping to absolute paths
- One active tin at a time, stored in the same file

## Releases & CI

- goreleaser for cross-platform binary builds and GitHub Releases
- Two GitHub Actions workflows:
  - `ci.yml` -- runs on every push, executes lint and tests
  - `release.yml` -- runs on version tag (`v*`), triggers goreleaser
- golangci-lint for linting
- Releases produce binaries for Windows, macOS (Intel + Apple Silicon), and Linux

## Testing

- Standard Go `testing` package
- Table-driven tests throughout
- Real filesystem using `os.MkdirTemp` -- no mocking the filesystem
- Tests live alongside the code they test (`_test.go` convention)
- CI fails on any test failure or lint error