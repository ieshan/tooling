# gocheck

Run Go code quality tools (golangci-lint, gosec, govulncheck, nilaway) against any project via Docker, without installing them on the host or adding tool dependencies to `go.mod`.

## Prerequisites

- Docker must be installed and running.
- Build the image before first use (see below).

## Build docker image

```bash
docker build -t gocheck-image:latest .
```

## Install the wrapper

```bash
chmod +x ./gocheck
cp ./gocheck ~/.local/bin
```

## Installed tools

| Tool | Version |
|---|---|
| golangci-lint | v2.13.2 |
| gosec | v2.29.0 |
| govulncheck | v1.7.0 |
| nilaway | v0.0.0-20260808063849-8649a03c818a |
| shadow (`go vet` vettool) | golang.org/x/tools v0.49.0 |
| nilness (`go vet` vettool) | golang.org/x/tools v0.49.0 |

All tools run on Go 1.27.1 (Alpine).

## Usage

Run Go code quality tools against any project from its directory:

```bash
gocheck golangci-lint run ./...
gocheck gosec ./...
gocheck govulncheck ./...
gocheck nilaway ./...

# `go vet` vettools (singlechecker binaries, run directly)
gocheck shadow ./...
gocheck nilness ./...
```

You can also run any command available in the image:

```bash
gocheck go vet ./...
gocheck go test -race ./...
```

## Rebuilding the image

After bumping tool versions in the Dockerfile, rebuild with:

```bash
gocheck rebuild                       # uses the script's own directory
gocheck rebuild /path/to/gocheck-dir  # explicit build context
```

Or manually:

```bash
docker build --no-cache -t gocheck-image:latest /path/to/gocheck-dir
```

## Notes

- `govulncheck` requires network access to fetch the vulnerability database.
- Go build and module caches are persisted in Docker named volumes
  (`gocheck-gocache`, `gocheck-gomodcache`) for faster repeated runs.
- Tools are NOT added to your project's `go.mod` — they live only inside the image.
