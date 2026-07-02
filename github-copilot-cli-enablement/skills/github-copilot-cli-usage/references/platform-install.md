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
brew install --cask copilot-cli
```

Update:

```bash
brew upgrade --cask copilot-cli
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

## Linux

Install with Homebrew or npm, depending on the approved package manager:

```bash
brew install --cask copilot-cli
```

or:

```bash
node --version
npm --version
npm install -g @github/copilot
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
