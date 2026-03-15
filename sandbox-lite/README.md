# sandbox-lite

Lightweight sandbox base image for CrashLoom's fixer agent. Provides minimal tooling for code analysis and simple builds.

## Base

`ubuntu:24.04`

## Included Tools

| Tool | Purpose |
|------|---------|
| git | Repository cloning and history analysis |
| python3, pip | Script execution, tree-sitter bindings |
| tree-sitter (CLI + Python) | AST parsing for code analysis |
| gcc, g++, make, cmake | C/C++ compilation |
| curl, wget | Dependency fetching |
| libssl-dev, libpq-dev | Build dependencies |

## Usage

```bash
docker pull ghcr.io/crashloom/sandbox-lite:latest
docker run --rm -it ghcr.io/crashloom/sandbox-lite:latest bash
```

## Image Hierarchy

```
sandbox-lite        <- this image (Ubuntu 24.04 + minimal tools)
  sandbox-base      <- adds Node.js, Ruby, Python extras, DB clients, Go, Java, .NET, PHP, Rust
    sandbox-base-mobile  <- adds Flutter, Android SDK, React Native deps
```
