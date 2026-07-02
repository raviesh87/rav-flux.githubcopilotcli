---
name: github-copilot-cli-usage
description: Help users install, authenticate, configure, learn, troubleshoot, and apply GitHub Copilot CLI in enterprise or organization-provided GitHub Copilot environments. Use when asked for Copilot CLI onboarding, rollout guidance, common command-line workflows, safe tool approval practices, prompt examples, skills/plugins publishing, marketplace sharing, or migration from the old gh copilot extension to the standalone copilot CLI.
license: MIT
---

# GitHub Copilot CLI Usage

Use this skill to help organization users adopt the current standalone GitHub Copilot CLI, invoked with the `copilot` command.

## Current Product Note

Prefer the standalone `copilot` CLI. Do not default to the older `gh copilot suggest` or `gh copilot explain` extension workflow unless the user explicitly says their organization still uses that legacy extension.

## Setup Workflow

1. Confirm the user has an active GitHub Copilot plan and that the organization or enterprise has enabled the Copilot CLI policy.
2. Choose one install method:
   - Windows: `winget install GitHub.Copilot`
   - Cross-platform npm, requiring Node.js 22 or later: `npm install -g @github/copilot`
   - macOS/Linux Homebrew: `brew install copilot-cli`
   - macOS/Linux install script, when approved: `curl -fsSL https://gh.io/copilot-install | bash`
3. Verify installation with `copilot --version` and `copilot help`.
4. Authenticate with `copilot login`, or start `copilot` and run `/login`.
5. Start in a trusted project directory with `copilot`, confirm the directory trust prompt only for code the user is allowed to share with an AI tool, and ask an initial question such as `Give me an overview of this project.`

## Safe Usage Rules

- Remind users that Copilot CLI can read, modify, and execute files in trusted directories after user approval.
- Encourage plan mode for complex tasks. In the interactive UI, press `Shift+Tab` to cycle into plan mode.
- Use `@relative/path` to add files to context.
- Treat tool approvals carefully. Allow read-only and known safe commands more freely; be cautious with install, delete, credential, network, publish, deploy, and push commands.
- Avoid suggesting `--allow-all-tools` for normal users. Prefer targeted approvals such as `--allow-tool='shell(git:*)'` or session-by-session approval.
- Use `/review` before committing significant changes.
- Use `/model` when the user needs to choose among available models.
- Use `/experimental` or `copilot --experimental` only when the user explicitly wants opt-in experimental behavior such as Autopilot mode.
- Use `/lsp` and a user-level `~/.copilot/lsp-config.json` or repo-level `.github/lsp.json` when code intelligence needs installed language servers.
- Use `copilot -p "PROMPT"` for one-off command-line prompts, and `copilot -sp "PROMPT"` when scripting and only the answer should be printed.

## Resource Map

- Read `references/platform-install.md` when the user asks for Windows, macOS, Linux, managed-device, package-manager, PATH, update, verification, or first-login instructions.
- Read `references/use-cases.md` when the user asks for prompt examples, team rollout playbooks, troubleshooting steps, publishing instructions, or common enterprise workflows.

## Deliverables This Skill Can Produce

- User-specific installation and authentication steps for Windows, macOS, Linux, GitHub.com, or GHE.com.
- Team rollout checklists for organizations that grant Copilot access centrally.
- Prompt packs for code exploration, bug fixing, testing, refactoring, review, pull requests, CI failures, documentation, and release notes.
- Safe approval guidance for interactive and non-interactive CLI usage.
- Instructions for creating and sharing skills through local skill folders or Copilot CLI plugins and marketplaces.
