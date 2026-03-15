ARG BASE_IMAGE=sandbox-base:latest
FROM ${BASE_IMAGE}

ARG DEBIAN_FRONTEND=noninteractive

# =============================================================================
# Layer 1: Java (Eclipse Temurin JDK 21) — required by Android SDK tools
# =============================================================================
RUN curl -fsSL https://packages.adoptium.net/artifactory/api/gpg/key/public \
    | gpg --dearmor -o /usr/share/keyrings/adoptium-archive-keyring.gpg \
    && echo "deb [signed-by=/usr/share/keyrings/adoptium-archive-keyring.gpg] https://packages.adoptium.net/artifactory/deb $(lsb_release -cs) main" \
        > /etc/apt/sources.list.d/adoptium.list \
    && apt-get update && apt-get install -y --no-install-recommends temurin-21-jdk \
    && rm -rf /var/lib/apt/lists/*

# =============================================================================
# Layer 2: Android SDK cmdline-tools
# =============================================================================
ENV ANDROID_HOME="/opt/android-sdk"
ARG ANDROID_CMDLINE_TOOLS_VERSION=11076708

RUN mkdir -p "$ANDROID_HOME/cmdline-tools" \
    && curl -fsSL "https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_CMDLINE_TOOLS_VERSION}_latest.zip" \
    -o /tmp/cmdline-tools.zip \
    && unzip -q -d "$ANDROID_HOME/cmdline-tools" /tmp/cmdline-tools.zip \
    && mv "$ANDROID_HOME/cmdline-tools/cmdline-tools" "$ANDROID_HOME/cmdline-tools/latest" \
    && rm /tmp/cmdline-tools.zip

ENV PATH="$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/platform-tools:$PATH"

RUN yes | sdkmanager --licenses > /dev/null 2>&1 \
    && sdkmanager \
    "platform-tools" \
    "build-tools;34.0.0" \
    "platforms;android-34"

# =============================================================================
# Layer 3: Flutter SDK
# =============================================================================
ENV FLUTTER_HOME="/opt/flutter"
RUN git clone --depth 1 --branch stable https://github.com/flutter/flutter.git "$FLUTTER_HOME" \
    && "$FLUTTER_HOME/bin/flutter" precache \
    && "$FLUTTER_HOME/bin/flutter" config --no-analytics

ENV PATH="$FLUTTER_HOME/bin:$FLUTTER_HOME/bin/cache/dart-sdk/bin:$PATH"

# =============================================================================
# Layer 4: React Native dependencies (watchman)
# =============================================================================
RUN apt-get update && apt-get install -y --no-install-recommends watchman \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /workspace
