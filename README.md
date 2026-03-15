# CrashLoom Sandbox Images

Pre-built Docker images for CrashLoom's fixer agent sandbox environments.

## Images

| Image | Description | Base |
|-------|-------------|------|
| `ghcr.io/crashloom/sandbox-lite` | Minimal: git, Python, tree-sitter, build tools | `ubuntu:24.04` |
| `ghcr.io/crashloom/sandbox-base` | Full: Node.js, Ruby, Java, Go, .NET, PHP, Rust, DB clients | `sandbox-lite` |
| `ghcr.io/crashloom/sandbox-base-mobile` | Mobile: Flutter, Android SDK, React Native | `sandbox-base` |

## Image Hierarchy

```
sandbox-lite  (Ubuntu 24.04 + minimal tools)
  └── sandbox-base  (multi-language toolchains)
        └── sandbox-base-mobile  (mobile SDKs)
```

## Usage

```bash
docker pull ghcr.io/crashloom/sandbox-base:latest
```

See each image's directory for detailed tool listings.
