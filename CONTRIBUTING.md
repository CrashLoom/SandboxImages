# Contributing to Sandbox Images

Thanks for contributing. This repo contains the Docker base images used by
CrashLoom's fixer agent sandbox environments.

## Ways to contribute

- Add or update toolchains in existing images
- Optimize image layers and build times
- Report broken tools or version issues
- Improve CI/release automation

## Development setup

Build images locally:

```bash
docker build -t sandbox-lite:dev ./sandbox-lite
docker build --build-arg BASE_IMAGE=sandbox-lite:dev -t sandbox-base:dev ./sandbox-base
docker build --build-arg BASE_IMAGE=sandbox-base:dev -t sandbox-base-mobile:dev ./sandbox-base-mobile
```

## Image hierarchy

```
sandbox-lite             <- Ubuntu 24.04 + minimal tools
  sandbox-base           <- multi-language toolchains
    sandbox-base-mobile  <- mobile SDKs
```

Changes to `sandbox-lite` affect all downstream images.

## Pull requests

- Use conventional commit style in PR title (`feat:`, `fix:`, `docs:`, etc.).
- Keep PRs focused and small when possible.
- Fill the PR template checklist.
- Test builds pass in CI before merge.

## Issues and labels

Use issue forms for:

- Tool update requests
- Build failures
- New toolchain requests

## Code of conduct

Be respectful and constructive in issues and pull requests.
