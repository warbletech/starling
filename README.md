# Starling

**MCP server for Kubernetes ops** — scan manifests, read live clusters, remediate from your agent.

This repository is the **public release channel** for the Starling CLI.
Source code lives in [`warbletech/warble-core`](https://github.com/warbletech/warble-core/tree/main/ops/starling).

## Download

See [Releases](https://github.com/warbletech/starling/releases) for prebuilt binaries (macOS / Linux / Windows × amd64 / arm64).

## Install

```bash
# macOS (Apple Silicon example)
curl -L https://github.com/warbletech/starling/releases/latest/download/starling_0.1.0_macOS_arm64.tar.gz | tar xz
sudo mv starling /usr/local/bin/

starling login
```

Full install guide: https://warblecloud.com/install
IDE setup:          https://warblecloud.com/mcp-setup
Kubeconfig guide:   https://warblecloud.com/kubeconfig-setup

## License

Apache-2.0. © Warble Cloud / ChirpStack LLP.
