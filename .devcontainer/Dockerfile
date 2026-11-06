# Base image for the T3 Code dev container. Kept as a Dockerfile (rather than a
# bare `image` reference) so we can seed a global npm registry before the
# devcontainer features run: the node feature installs pnpm with
# `npm install -g pnpm`, which happens at image-build time, where containerEnv
# and remoteEnv do not apply yet. A global npmrc is the only lever that reaches
# that step.
FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

# This network SNI-filters registry.npmjs.org, so any build-time npm fetch
# fails with ECONNRESET. Point npm at the Verdaccio pull-through proxy on the
# Mac mini (reachable from containers over Tailscale). The node feature's
# `npm install -g pnpm` then succeeds. Runtime pnpm/npm traffic is covered by
# the registry env vars in devcontainer.json; this file only fixes build time.
#
# The env var, not a global npmrc: the node feature installs Node via nvm, and
# nvm's npm reads its globalconfig from ${NVM_DIR}/versions/node/*/etc/npmrc,
# so a file at /usr/local/etc/npmrc is ignored. NPM_CONFIG_REGISTRY is read by
# whichever npm binary runs.
#
# Set T3_NPM_REGISTRY to "" (or override it) to build against a different
# registry, e.g. when the proxy is unreachable.
ARG T3_NPM_REGISTRY=http://100.67.158.78:4873
ENV NPM_CONFIG_REGISTRY=${T3_NPM_REGISTRY}
ENV COREPACK_NPM_REGISTRY=${T3_NPM_REGISTRY}
