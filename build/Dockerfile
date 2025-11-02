ARG NODE_VERSION=25
FROM node:${NODE_VERSION}-bookworm AS base

ENV DEBIAN_FRONTEND=noninteractive

# Install tooling commonly needed in development containers.
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        build-essential \
        ca-certificates \
        git \
        openssh-client \
        sudo \
    && rm -rf /var/lib/apt/lists/*

ARG USER_NAME=node
RUN usermod -aG sudo "${USER_NAME}" \
    && echo "${USER_NAME} ALL=(ALL) NOPASSWD:ALL" > "/etc/sudoers.d/${USER_NAME}" \
    && chmod 0440 "/etc/sudoers.d/${USER_NAME}"

ENV NODE_ENV=development \
    npm_config_cache=/tmp/npm-cache

WORKDIR /workspace

RUN install -d -o "${USER_NAME}" -g "${USER_NAME}" /tmp/npm-cache /workspace

COPY --chown=${USER_NAME}:${USER_NAME} package*.json ./

USER ${USER_NAME}
RUN npm ci

USER root

COPY --chown=${USER_NAME}:${USER_NAME} . .

USER ${USER_NAME}

EXPOSE 8787
