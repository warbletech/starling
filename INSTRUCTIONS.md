# Starling — Setup & Usage

> **MCP server for Kubernetes ops** — scan manifests, read live clusters, and run agent-driven remediation.
>
> **Starling** is a WarbleCloud product (a Kubernetes Platform capability — see
> <https://warblecloud.com/capabilities>). It sits *above* the public
> [`warble-tech/starling`](https://github.com/warble-tech/starling) repo, which is only
> the **release channel** for the Starling CLI. Source lives in the private
> `warble-tech/warble-core` repo.
>
> ```text
> Starling  (WarbleCloud product / capability)
>    └── warble-tech/starling   ← public repo: release binaries only
>            └── source: warble-tech/warble-core  (private)
> ```

> ⚠️ **Verify against actual schema.** The tool names, parameters, and JSON payloads in the
> [Tool Call Examples](#tool-call-examples) section below are **inferred from the documented
> capabilities** — they are not copied from the real MCP server. Before relying on them,
> confirm the actual tool list:
>
> ```bash
> starling mcp --stdio <<< '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
> ```
>
> or check the tool registry in `warble-tech/warble-core`. Update names/params here to match.

---

## Install

**macOS (arm64):**
```bash
curl -L https://github.com/warble-tech/starling/releases/latest/download/starling_0.1.0_macOS_arm64.tar.gz | tar xz
sudo mv starling /usr/local/bin/
starling login
```

Pre-built binaries for **macOS** and **Linux** (`amd64` / `arm64`) are on the
[Releases page](https://github.com/warble-tech/starling/releases/latest). Pick the
archive matching your OS/arch.

**Setup guides:**
- Full install: <https://warblecloud.com/install>
- IDE / MCP client setup: <https://warblecloud.com/mcp-setup>
- Kubeconfig: <https://warblecloud.com/kubeconfig-setup>

## Authenticate

```bash
starling login
```

This opens a browser flow against Warble Cloud and stores credentials locally.
Starling reads your cluster through your existing `kubeconfig` (default
`~/.kube/config`, or `$KUBECONFIG`).

## Configure your MCP client

Add Starling as an MCP server. Example for a client that uses `mcpServers` JSON:

```json
{
  "mcpServers": {
    "starling": {
      "command": "starling",
      "args": ["mcp", "--stdio"]
    }
  }
}
```

Restart the client; Starling's tools then appear in `tools/list`.

---

## Tool Call Examples

> ⚠️ Inferred — verify names and parameters against the live server (see banner above).

For each capability we show **(a)** a natural-language prompt a user gives the agent
and **(b)** the MCP `tools/call` request that prompt would produce.

### 1. Scan a manifest

**Prompt:**
> "Scan `deployment.yaml` for misconfigurations."

**MCP tool call:**
```json
{
  "jsonrpc": "2.0",
  "id": 2,
  "method": "tools/call",
  "params": {
    "name": "scan_manifest",
    "arguments": {
      "path": "deployment.yaml"
    }
  }
}
```

### 2. Read live cluster resources

**Prompt:**
> "List all pods in the `payments` namespace and show which are not Ready."

**MCP tool call:**
```json
{
  "jsonrpc": "2.0",
  "id": 3,
  "method": "tools/call",
  "params": {
    "name": "get_cluster_resources",
    "arguments": {
      "kind": "Pod",
      "namespace": "payments"
    }
  }
}
```

### 3. Remediate an issue

**Prompt:**
> "The `api` deployment in `payments` has no resource limits — fix it."

**MCP tool call:**
```json
{
  "jsonrpc": "2.0",
  "id": 4,
  "method": "tools/call",
  "params": {
    "name": "remediate",
    "arguments": {
      "kind": "Deployment",
      "name": "api",
      "namespace": "payments",
      "issue": "missing-resource-limits",
      "dry_run": true
    }
  }
}
```

Run with `"dry_run": true` first to preview the patch; set it to `false` to apply.

---

## License

Apache-2.0 © Warble Cloud / ChirpStack LLP
