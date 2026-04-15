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

## HTML Generation

The generated HTML is the product. It needs to last. These are the rules the generator must follow without exception.

### Rules

- Output must be valid, semantic HTML5
- No inline styles -- all styling via the single root `style.css`
- No JavaScript in generated HTML -- enhancement only via an optional external script tag, never inline
- No external resources of any kind -- no CDN, no web fonts, no analytics, no tracking pixels
- Images must have descriptive alt attributes -- filename used as fallback if no metadata exists
- Heading structure must be logical and consistent across all generated pages
- All links must be relative -- never absolute paths, never protocol-relative
- The root `index.html` must link to all direct child folder indexes
- Each folder `index.html` must link back to its parent
- Thumbnail images link to their originals

### Template

- A single `index.html.tmpl` drives all generated pages
- Go's `html/template` package -- not a third party templating library
- Template is embedded in the binary at build time using `embed.FS`
- The default `style.css` is also embedded and written on first run or rebuild

## TUI

Bubble Tea drives the interface. The TUI is not decorative -- it communicates what the tool is doing and whether it succeeded. That's it.

### Principles

- Output should feel calm and confident, not developer-tool noisy
- No walls of log output
- Progress should be visible for anything that takes more than a second -- thumbnail generation especially
- Errors are clear and human. Not stack traces, not codes. Just what went wrong and what to do about it.

### States

- **Idle** -- active tin name shown, available commands hinted
- **Running** -- spinner with a plain description of what's happening, progress bar for batch operations (thumbnail generation, index writing)
- **Done** -- short summary. What was built, how many images, how long it took.
- **Error** -- what failed, which file or folder if relevant, what to try next

### Style

- Lipgloss for colour and layout
- Minimal colour -- muted, not loud
- No ASCII art, no banners, no version splashes on every run
