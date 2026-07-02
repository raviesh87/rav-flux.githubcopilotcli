# GitHub Copilot CLI Organization User Guide

Last reviewed: 2026-07-02

This guide is for users whose organization has granted GitHub Copilot access and enabled GitHub Copilot CLI. The current CLI is the standalone `copilot` command. Older material may mention the previous `gh copilot suggest` or `gh copilot explain` extension; use the standalone CLI unless your organization explicitly tells you otherwise.

## 1. Confirm Access

Before installing, confirm:

- Your GitHub account has an active Copilot plan through your organization or enterprise.
- The organization or enterprise Copilot CLI policy is enabled.
- Your machine can reach GitHub services through corporate proxy, VPN, firewall, or certificate inspection controls.
- On Windows, PowerShell v6 or higher is available.

If you can use Copilot in an IDE but `copilot` says access is disabled, ask a GitHub organization owner or enterprise admin to verify the Copilot CLI policy.

## 2. Install Copilot CLI

Choose the path your organization supports. Managed Windows devices usually use WinGet. Managed macOS devices usually use Homebrew, Self Service, Jamf, Intune, or another endpoint software catalog. The npm path works on macOS, Linux, and Windows when Node.js 22 or later is approved.

### Windows

Open PowerShell. GitHub documents PowerShell v6 or higher as the Windows prerequisite:

```powershell
$PSVersionTable.PSVersion
```

Verify WinGet is available:

```powershell
winget --version
```

Install:

```powershell
winget install GitHub.Copilot
```

Install the prerelease channel only when your organization explicitly wants it:

```powershell
winget install GitHub.Copilot.Prerelease
```

Update later:

```powershell
winget upgrade GitHub.Copilot
```

If your organization does not allow WinGet but allows npm, verify Node.js 22 or later and install:

```powershell
node --version
npm --version
npm install -g @github/copilot
```

If npm is configured with `ignore-scripts=true`, use:

```powershell
npm_config_ignore_scripts=false npm install -g @github/copilot
```

Close and reopen PowerShell, then verify:

```powershell
copilot --version
copilot help
```

If `copilot` is not found, restart the terminal, check whether your endpoint management tool completed installation, and confirm the install location is on `PATH`.

### macOS

Open Terminal. Verify Homebrew is available:

```bash
brew --version
```

Install:

```bash
brew install copilot-cli
```

Install the prerelease channel only when your organization explicitly wants it:

```bash
brew install copilot-cli@prerelease
```

Update later:

```bash
brew upgrade copilot-cli
```

If your organization does not allow Homebrew but allows npm, verify Node.js 22 or later and install:

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

If `copilot` is not found, open a new Terminal window and verify your shell profile includes the Homebrew or npm binary path. On Apple Silicon Macs, Homebrew commonly uses `/opt/homebrew/bin`; on Intel Macs, it commonly uses `/usr/local/bin`.

Alternative install script for macOS when approved by your organization:

```bash
curl -fsSL https://gh.io/copilot-install | bash
```

or:

```bash
wget -qO- https://gh.io/copilot-install | bash
```

Use `| sudo bash` only when your organization allows installing to `/usr/local/bin` as root. Set `PREFIX` to install to a custom directory, and set `VERSION` to install a specific release:

```bash
curl -fsSL https://gh.io/copilot-install | VERSION="v0.0.369" PREFIX="$HOME/custom" bash
```

### Linux

Use Homebrew, npm, or the install script, depending on your organization's approved package manager:

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

## 3. Authenticate

For normal interactive use:

```powershell
copilot login
```

For GitHub Enterprise Cloud with data residency:

```powershell
copilot login --host HOSTNAME
```

You can also start the CLI with `copilot`, then type `/login`. The device flow will show a one-time code and open your browser.

Personal access token option: create a fine-grained personal access token with the `Copilot Requests` permission, then set it through `GH_TOKEN` or `GITHUB_TOKEN`. The quickstart lists `GH_TOKEN` before `GITHUB_TOKEN` for precedence.

Environment token note: token variables can override stored login credentials. If authentication behaves oddly, check whether `GH_TOKEN` or `GITHUB_TOKEN` is set for another tool.

## 4. First Session

Open a repository or project folder.

Windows PowerShell:

```powershell
cd C:\path\to\repo
copilot
```

macOS/Linux Terminal:

```bash
cd ~/path/to/repo
copilot
```

When asked whether you trust the folder, proceed only for code you are allowed to share with an AI coding tool. During a trusted session, Copilot CLI may ask to read, edit, or execute files in that directory, but it should ask before actions that modify or execute code.

Try:

```text
Give me an overview of this project.
```

On first launch, Copilot CLI displays its startup banner. To show the banner again later:

```powershell
copilot --banner
```

Useful interactive controls:

- `Shift+Tab`: cycle into or out of plan mode.
- `@relative/path`: include a specific file in context.
- `/help`: show available slash commands.
- `/login`: authenticate if not already logged in.
- `/model`: choose from available models.
- `/experimental`: enable or manage experimental mode.
- `/lsp`: check configured Language Server Protocol servers.
- `/feedback`: submit feedback from the CLI.
- `?`: show tabbed help.
- `Esc`: cancel the current operation.
- `Ctrl+C`: cancel, clear input, or exit depending on state.
- `Ctrl+L`: clear the screen.

The quickstart says Copilot CLI uses Claude Sonnet 4.5 by default and allows users to choose other available models, including Claude Sonnet 4 and GPT-5, with `/model`.

## 5. Recommended Daily Workflow

1. Start in the repo: `copilot`.
2. Explore first: `Give me an overview of this repo and the test strategy.`
3. For non-trivial work, enter plan mode with `Shift+Tab`.
4. Ask for a plan: `Plan the smallest safe change to add this feature. Ask questions before editing.`
5. Approve file edits and commands deliberately.
6. Run tests and lint through Copilot or manually.
7. Use `/review` before committing.
8. Ask for a PR summary once the branch is ready.

## 6. Common Use Cases

Code exploration:

```text
Explain the main request flow and where authentication is enforced.
```

File-specific explanation:

```text
Explain @src/app.ts and call out risky assumptions.
```

Bug investigation:

```text
Investigate why the checkout total is wrong, propose a fix, and run the smallest relevant test set.
```

Testing:

```text
Add focused tests for @src/pricing/calculate.ts, including boundary cases.
```

Refactoring:

```text
Refactor this module for readability without changing behavior. Run existing tests afterward.
```

Code review:

```text
/review
```

Pull request preparation:

```text
Review my current branch, summarize the changes, and draft a PR description.
```

Docs:

```text
Update the README setup section to match the current commands.
```

Git help:

```powershell
copilot -p "In Git, how can I apply one commit from another branch?"
```

Branch summary:

```powershell
copilot -p "Summarize my branch compared with main" --allow-tool='shell(git:*)'
```

Scripting with only the response text:

```powershell
copilot -sp "Generate release notes from the commits since the last tag"
```

## 7. Tool Approval Guidance

Copilot CLI can request permission to run tools such as shell commands, file writes, web access, or subagents.

Good defaults:

- Approve unfamiliar commands once.
- Approve repeated low-risk commands for the session, such as `git status`, `git diff`, `npm test`, or project-local lint commands.
- Be cautious with package installs, credential updates, file deletion, network calls, deployments, production writes, and `git push`.
- Avoid `--allow-all-tools` for normal development.
- Reset saved approvals with `/reset-allowed-tools` when needed.

Example targeted startup:

```powershell
copilot --allow-tool='shell(git:*)' --deny-tool='shell(git push)'
```

## 8. Skills And Plugins

Skills are task-specific instruction folders. Use them when Copilot should load detailed instructions only for relevant work.

Project skill locations:

- `.github/skills`
- `.claude/skills`
- `.agents/skills`

Personal skill locations:

- `~/.copilot/skills`
- `~/.agents/skills`

Useful commands inside Copilot CLI:

```text
/skills list
/skills info github-copilot-cli-usage
/skills reload
```

Useful terminal commands:

```powershell
copilot skill list
copilot skill add <FILE-URL-OR-DIRECTORY>
```

Plugins package skills, agents, hooks, and MCP server configs so users can install them more easily.

Test this bundle locally from the repository root:

```powershell
copilot plugin install ./github-copilot-cli-enablement
copilot plugin list
```

Then start `copilot` and verify:

```text
/skills list
/skills info github-copilot-cli-usage
```

After editing a local plugin, reinstall it because Copilot CLI caches installed plugin contents:

```powershell
copilot plugin install ./github-copilot-cli-enablement
```

## 9. Models, Experimental Mode, And LSP

Model selection:

```text
/model
```

Use this when a task benefits from a different available model, or when your organization directs users to a specific model.

Experimental mode:

```powershell
copilot --experimental
```

Inside the CLI:

```text
/experimental
```

The quickstart says this setting persists in your config once enabled. One experimental feature listed there is Autopilot mode, which users can reach by cycling modes with `Shift+Tab`. Treat experimental behavior as opt-in and potentially changing.

Language Server Protocol support gives Copilot CLI more code intelligence, but the CLI does not bundle language servers. Install language servers separately.

TypeScript example:

```powershell
npm install -g typescript-language-server
```

User-level LSP configuration:

```text
~/.copilot/lsp-config.json
```

Repository-level LSP configuration:

```text
.github/lsp.json
```

Example `.github/lsp.json`:

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

Check LSP status in an interactive session:

```text
/lsp
```

## 10. Publishing For All Users

To share this as an internal plugin marketplace:

1. Put this repository in a GitHub org repo that users can read.
2. Keep `.github/plugin/marketplace.json` at the repository root.
3. Keep `github-copilot-cli-enablement/plugin.json` and its `skills/` folder at the path referenced by the marketplace file.
4. Ask users to register the marketplace:

```powershell
copilot plugin marketplace add ORG/REPO
```

5. Ask users to browse and install:

```powershell
copilot plugin marketplace browse org-copilot-cli-enablement
copilot plugin install github-copilot-cli-enablement@org-copilot-cli-enablement
```

For a local/shared filesystem marketplace, users can add the local path that contains `.github/plugin/marketplace.json`:

```powershell
copilot plugin marketplace add C:\path\to\marketplace-repo
```

## 11. Admin Rollout Checklist

- Confirm Copilot licenses are assigned.
- Enable Copilot CLI policy at org or enterprise level.
- Choose standard install method by platform.
- Confirm proxy, firewall, and certificate requirements.
- Publish guidance on source code, secrets, regulated data, and production deployments.
- Add `.github/copilot-instructions.md` to important repos with build/test/lint commands and coding standards.
- Decide whether prerelease, experimental mode, PAT auth, and user-installed LSP servers are allowed.
- Remind users that each prompt may consume premium request quota under their Copilot plan.
- Publish this plugin marketplace or copy the skill to `.github/skills`.
- Run a pilot with a small group and capture examples that work well for your codebase.

## 12. Troubleshooting

`copilot` command not found:

- Restart the terminal.
- Check PATH.
- Reinstall with WinGet, npm, or Homebrew.

npm install fails:

- Verify Node.js 22 or later.
- Check corporate npm registry/proxy access.

Authentication loops:

- Rerun `copilot login`.
- Confirm the browser is signed into the expected GitHub account.
- Unset unintended `COPILOT_GITHUB_TOKEN`, `GH_TOKEN`, or `GITHUB_TOKEN` values.

Access denied:

- Ask an organization owner or enterprise admin to verify that Copilot CLI is enabled.

Skill not found:

- Confirm the folder contains `SKILL.md`.
- Confirm the skill name in frontmatter.
- Run `/skills reload`.
- Run `/skills info SKILL-NAME`.

Plugin changes not showing:

- Reinstall the plugin because installed plugin contents are cached.

Old `gh copilot` examples do not work:

- Use the standalone `copilot` command. Only use `gh copilot` if your organization still documents and supports that older extension.

Missing LSP intelligence:

- Install the correct language server separately.
- Add user-level config in `~/.copilot/lsp-config.json` or repo-level config in `.github/lsp.json`.
- Run `/lsp` inside Copilot CLI.

## Sources

- GitHub Docs, Getting started with GitHub Copilot CLI: https://docs.github.com/en/copilot/how-tos/copilot-cli/cli-getting-started
- GitHub Docs, Installing GitHub Copilot CLI: https://docs.github.com/en/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
- GitHub Docs, Authenticating GitHub Copilot CLI: https://docs.github.com/en/copilot/how-tos/copilot-cli/set-up-copilot-cli/authenticate-copilot-cli
- GitHub Docs, Using GitHub Copilot CLI: https://docs.github.com/en/copilot/how-tos/copilot-cli/use-copilot-cli/overview
- GitHub Docs, Allowing and denying tool use: https://docs.github.com/en/copilot/how-tos/copilot-cli/use-copilot-cli/allowing-tools
- GitHub Docs, Adding agent skills for GitHub Copilot CLI: https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-skills
- GitHub Docs, Creating a plugin for GitHub Copilot CLI: https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-creating
- GitHub Docs, Creating a plugin marketplace for GitHub Copilot CLI: https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-marketplace
- GitHub Docs, GitHub Copilot CLI plugin reference: https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-plugin-reference
- GitHub Copilot CLI quickstart repository: https://gh.io/copilot-cli-quickstart
