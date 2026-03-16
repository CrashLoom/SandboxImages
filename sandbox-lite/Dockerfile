FROM ubuntu:24.04

ARG DEBIAN_FRONTEND=noninteractive

# =============================================================================
# Layer 1: System essentials + git
# =============================================================================
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates curl wget git \
    && rm -rf /var/lib/apt/lists/*

# =============================================================================
# Layer 2: Build tools
# =============================================================================
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential gcc g++ make cmake pkg-config \
    libssl-dev libpq-dev libyaml-dev \
    && rm -rf /var/lib/apt/lists/*

# =============================================================================
# Layer 3: Python 3 + pip + tree-sitter Python bindings + language grammars
# =============================================================================
RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 python3-pip \
    && rm -rf /var/lib/apt/lists/* \
    && rm -f /usr/lib/python3.*/EXTERNALLY-MANAGED \
    && pip3 install --no-cache-dir tree-sitter tree-sitter-language-pack

# =============================================================================
# Layer 5: Hardening — strip setuid/setgid, clear caches
# =============================================================================
RUN find / -perm /4000 -type f -exec chmod u-s {} + 2>/dev/null; \
    find / -perm /2000 -type f -exec chmod g-s {} + 2>/dev/null; \
    rm -rf /tmp/* /var/tmp/* /var/log/* /var/cache/apt/*

# =============================================================================
# Layer 6: Workspace
# =============================================================================
RUN mkdir -p /workspace

WORKDIR /workspace
