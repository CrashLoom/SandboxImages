# sandbox-base

Full multi-language sandbox image for CrashLoom's fixer agent. Supports most backend and web project types.

## Base

`sandbox-lite` (Ubuntu 24.04 + git, Python, tree-sitter, build tools)

## Included Tools

Everything from `sandbox-lite`, plus:

| Category | Tools |
|----------|-------|
| Python | python3-venv, python3-dev, poetry, uv, pyenv |
| Node.js | nvm (LTS), npm, yarn, pnpm, bun |
| Ruby | rbenv, Ruby 3.1, 3.2, 3.3 |
| Java | JDK, Maven, Gradle |
| Go | Latest stable |
| .NET | Latest SDK |
| PHP | php, composer |
| Rust | rustc, cargo |
| C/C++ | gcc, g++, clang |
| DB clients | psql (PostgreSQL 16), redis-cli |
| Utilities | jq, ssh, zip/unzip, tar, gzip, xz |

## Usage

```bash
docker pull ghcr.io/crashloom/sandbox-base:latest
docker run --rm -it ghcr.io/crashloom/sandbox-base:latest bash
```

## Image Hierarchy

```
sandbox-lite             <- Ubuntu 24.04 + minimal tools
  sandbox-base           <- this image (multi-language toolchains)
    sandbox-base-mobile  <- adds Flutter, Android SDK, React Native deps
```
