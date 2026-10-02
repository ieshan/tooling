# Go code quality tools image.
# Pre-installs golangci-lint, gosec, govulncheck, and nilaway so they
# can be run via the `gocheck` wrapper against any project without
# installing anything on the host or adding tool dependencies to go.mod.
#
# Build:  docker build -t gocheck-image:latest .
# Run:    gocheck golangci-lint run ./...

FROM golang:1.27.1-alpine

# git + ca-certificates needed by govulncheck (vuln DB fetch) and gosec (VCS info)
RUN apk add --no-cache git ca-certificates

# All tools pinned to specific versions for reproducibility.
# shadow and nilness are `go vet` vettools (run via `go vet -vettool=$(which shadow)`).
RUN go install github.com/golangci/golangci-lint/v2/cmd/golangci-lint@v2.13.2 && \
    go install github.com/securego/gosec/v2/cmd/gosec@v2.29.0 && \
    go install golang.org/x/vuln/cmd/govulncheck@v1.7.0 && \
    go install go.uber.org/nilaway/cmd/nilaway@v0.0.0-20260808063849-8649a03c818a && \
    go install golang.org/x/tools/go/analysis/passes/shadow/cmd/shadow@v0.49.0 && \
    go install golang.org/x/tools/go/analysis/passes/nilness/cmd/nilness@v0.49.0

WORKDIR /work
