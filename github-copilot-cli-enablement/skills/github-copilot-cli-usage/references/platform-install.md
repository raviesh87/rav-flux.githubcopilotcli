# GitHub Copilot CLI Platform Install

## Preconditions For All Platforms

- Confirm the user has an active GitHub Copilot plan.
- Confirm the user's organization or enterprise enables the Copilot CLI policy.
- Confirm corporate VPN, proxy, firewall, and certificate rules allow GitHub and package-manager access.
- Prefer the package manager approved by desktop engineering.

## Windows

Use PowerShell v6 or higher:

```powershell
$PSVersionTable.PSVersion
```

Preferred install with WinGet:

```powershell
winget --version
winget install GitHub.Copilot
```

Prerelease channel, only when approved:

```powershell
winget install GitHub.Copilot.Prerelease
```

Update:

```powershell
winget upgrade GitHub.Copilot
```

Fallback install with npm, requiring Node.js 22 or later:

```powershell
node --version
npm --version
npm install -g @github/copilot
```

If npm has scripts disabled:

```powershell
npm_config_ignore_scripts=false npm install -g @github/copilot
```

Verify:

```powershell
copilot --version
copilot help
```

If `copilot` is not found, restart PowerShell, verify the endpoint install completed, and check that the install location is on `PATH`.

## macOS

Preferred install with Homebrew:

```bash
brew --version
brew install copilot-cli
```

Prerelease channel, only when approved:

```bash
brew install copilot-cli@prerelease
```

Update:

```bash
brew upgrade copilot-cli
```

Fallback install with npm, requiring Node.js 22 or later:

```bash
node --version
npm --version
npm install -g @github/copilot
```

Verify:

```bash
copilot --version
copilot help
which copilot
```

If `copilot` is not found, open a new Terminal window and verify the shell profile includes the Homebrew or npm binary path. Apple Silicon Homebrew commonly uses `/opt/homebrew/bin`; Intel Homebrew commonly uses `/usr/local/bin`.

Alternative install script, when approved:

```bash
curl -fsSL https://gh.io/copilot-install | bash
```

or:

```bash
wget -qO- https://gh.io/copilot-install | bash
```

Use `| sudo bash` only when root installation to `/usr/local/bin` is approved. Set `PREFIX` to install to a custom directory, and set `VERSION` to install a specific release:

```bash
curl -fsSL https://gh.io/copilot-install | VERSION="v0.0.369" PREFIX="$HOME/custom" bash
```

## Linux

Install with Homebrew or npm, depending on the approved package manager:

```bash
brew install copilot-cli
```

or:

```bash
node --version
npm --version
npm install -g @github/copilot
```

or:

```bash
curl -fsSL https://gh.io/copilot-install | bash
```

Verify:

```bash
copilot --version
copilot help
```

## Authentication

Interactive login:

```bash
copilot login
```

GitHub Enterprise Cloud with data residency:

```bash
copilot login --host HOSTNAME
```

Inside an interactive `copilot` session, users can also run:

```text
/login
```

If authentication behaves unexpectedly, check whether `COPILOT_GITHUB_TOKEN`, `GH_TOKEN`, or `GITHUB_TOKEN` is set and overriding stored credentials.

Personal access token option from the quickstart: create a fine-grained PAT with the `Copilot Requests` permission, then set `GH_TOKEN` or `GITHUB_TOKEN`.

## Models, Experimental Mode, And LSP

Choose available models:

```text
/model
```

The quickstart says Copilot CLI defaults to Claude Sonnet 4.5 and can choose other available models, including Claude Sonnet 4 and GPT-5.

Experimental mode:

```bash
copilot --experimental
```

or inside the CLI:

```text
/experimental
```

The setting persists after it is enabled. Autopilot mode is an experimental feature reached by cycling modes with `Shift+Tab`.

Install language servers separately. TypeScript example:

```bash
npm install -g typescript-language-server
```

Configure LSP at the user level with `~/.copilot/lsp-config.json` or at the repository level with `.github/lsp.json`.

Example repository config:

```json
{
  "lspServers": {
    "typescript": {
      "command": "typescript-language-server",
      "args": ["--stdio"],
      "fileExtensions": {
        ".ts": "typescript",
        ".tsx": "typescript"
      }
    }
  }
}
```

Check status:

```text
/lsp
```
