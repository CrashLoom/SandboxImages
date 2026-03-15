ARG BASE_IMAGE=sandbox-lite:latest
FROM ${BASE_IMAGE}

ARG DEBIAN_FRONTEND=noninteractive

# =============================================================================
# Layer 1: Additional system packages + Python/Ruby build deps
# (base tools like git, curl, gcc, make, cmake, libssl-dev already in sandbox-lite)
# =============================================================================
RUN apt-get update && apt-get install -y --no-install-recommends \
    jq gnupg2 software-properties-common apt-transport-https \
    openssh-client unzip zip tar gzip bzip2 xz-utils \
    lsb-release \
    clang \
    # Python build deps (pyenv)
    libffi-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev \
    libncurses5-dev libncursesw5-dev liblzma-dev tk-dev libgdbm-dev \
    libgdbm-compat-dev libdb-dev uuid-dev \
    && rm -rf /var/lib/apt/lists/*

# =============================================================================
# Layer 2: PostgreSQL 16 client (libpq-dev already in sandbox-lite)
# =============================================================================
RUN curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc \
    | gpg --dearmor -o /usr/share/keyrings/postgresql-archive-keyring.gpg \
    && echo "deb [signed-by=/usr/share/keyrings/postgresql-archive-keyring.gpg] https://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" \
        > /etc/apt/sources.list.d/pgdg.list \
    && apt-get update && apt-get install -y --no-install-recommends \
    postgresql-client-16 \
    && rm -rf /var/lib/apt/lists/*

# =============================================================================
# Layer 3: Redis 7 client tools
# =============================================================================
RUN curl -fsSL https://packages.redis.io/gpg \
    | gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg \
    && echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" \
        > /etc/apt/sources.list.d/redis.list \
    && apt-get update && apt-get install -y --no-install-recommends redis-tools \
    && rm -rf /var/lib/apt/lists/*

# =============================================================================
# Layer 4: Python extras (python3 + pip already in sandbox-lite) + poetry + uv
# =============================================================================
RUN apt-get update && apt-get install -y --no-install-recommends \
    python3-venv python3-dev \
    && rm -rf /var/lib/apt/lists/* \
    && pip3 install --no-cache-dir poetry uv

# =============================================================================
# Layer 5: pyenv
# =============================================================================
ENV PYENV_ROOT="/opt/pyenv"
RUN curl -fsSL https://pyenv.run | bash
RUN echo 'export PYENV_ROOT="/opt/pyenv"' > /etc/profile.d/pyenv.sh \
    && echo 'export PATH="$PYENV_ROOT/bin:$PYENV_ROOT/shims:$PATH"' >> /etc/profile.d/pyenv.sh \
    && echo 'eval "$(pyenv init -)"' >> /etc/profile.d/pyenv.sh

# =============================================================================
# Layer 6: Node.js LTS via nvm + yarn + pnpm + bun
# =============================================================================
ENV NVM_DIR="/opt/nvm"
RUN mkdir -p "$NVM_DIR" \
    && curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash \
    && . "$NVM_DIR/nvm.sh" \
    && nvm install --lts \
    && nvm alias default lts/* \
    && npm install -g yarn pnpm

RUN curl -fsSL https://bun.sh/install | BUN_INSTALL=/opt/bun bash

ENV PATH="/opt/bun/bin:$PATH"
RUN echo 'export NVM_DIR="/opt/nvm"' > /etc/profile.d/nvm.sh \
    && echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"' >> /etc/profile.d/nvm.sh

# Symlink default node/npm to /usr/local/bin for non-interactive shells
RUN . "$NVM_DIR/nvm.sh" \
    && ln -sf "$(which node)" /usr/local/bin/node \
    && ln -sf "$(which npm)" /usr/local/bin/npm \
    && ln -sf "$(which npx)" /usr/local/bin/npx \
    && ln -sf "$(which yarn)" /usr/local/bin/yarn \
    && ln -sf "$(which pnpm)" /usr/local/bin/pnpm

# =============================================================================
# Layer 7: rbenv + Ruby 3.1, 3.2, 3.3
# =============================================================================
ENV RBENV_ROOT="/opt/rbenv"
RUN git clone --depth 1 https://github.com/rbenv/rbenv.git "$RBENV_ROOT" \
    && git clone --depth 1 https://github.com/rbenv/ruby-build.git "$RBENV_ROOT/plugins/ruby-build" \
    && echo 'export RBENV_ROOT="/opt/rbenv"' > /etc/profile.d/rbenv.sh \
    && echo 'export PATH="$RBENV_ROOT/bin:$RBENV_ROOT/shims:$PATH"' >> /etc/profile.d/rbenv.sh \
    && echo 'eval "$(rbenv init -)"' >> /etc/profile.d/rbenv.sh

RUN export PATH="$RBENV_ROOT/bin:$PATH" && eval "$(rbenv init -)" \
    && rbenv install 3.1.6 && rbenv install 3.2.6 && rbenv install 3.3.6 \
    && rbenv global 3.3.6

# =============================================================================
# Layer 8: Hardening — strip setuid/setgid, clear caches
# (tree-sitter CLI + Python bindings already in sandbox-lite)
# =============================================================================
RUN find / -perm /4000 -type f -exec chmod u-s {} + 2>/dev/null; \
    find / -perm /2000 -type f -exec chmod g-s {} + 2>/dev/null; \
    rm -rf /tmp/* /var/tmp/* /var/log/* /var/cache/apt/*

# =============================================================================
# Layer 9: PATH consolidation (workspace already created in sandbox-lite)
# =============================================================================

ENV PATH="/opt/rbenv/bin:/opt/rbenv/shims:/opt/pyenv/bin:/opt/pyenv/shims:/opt/bun/bin:$NVM_DIR/versions/node/default/bin:$PATH"

WORKDIR /workspace
