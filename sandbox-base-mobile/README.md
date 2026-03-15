# sandbox-base-mobile

Mobile development sandbox image for CrashLoom's fixer agent. Extends `sandbox-base` with Flutter, Android SDK, and React Native tooling.

## Base

`sandbox-base` (all backend/web language toolchains)

## Included Tools

Everything from `sandbox-base`, plus:

| Category | Tools |
|----------|-------|
| Flutter | Latest stable SDK, Dart SDK |
| Android | SDK cmdline-tools, platform-tools, build-tools 34, Android 34 |
| React Native | Node.js (inherited), watchman |

## Usage

```bash
docker pull ghcr.io/crashloom/sandbox-base-mobile:latest
docker run --rm -it ghcr.io/crashloom/sandbox-base-mobile:latest bash
```

## Image Hierarchy

```
sandbox-lite             <- Ubuntu 24.04 + minimal tools
  sandbox-base           <- multi-language toolchains
    sandbox-base-mobile  <- this image (Flutter, Android SDK, React Native)
```
