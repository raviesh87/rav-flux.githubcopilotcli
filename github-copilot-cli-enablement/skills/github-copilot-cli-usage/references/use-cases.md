# GitHub Copilot CLI Use Cases

## First-Time User Flow

1. Confirm access: the user has a Copilot license, and the organization or enterprise policy enables Copilot CLI.
2. Install the CLI for the user's OS.
3. Run `copilot login` and complete browser-based authentication.
4. Open a project directory: `cd path/to/repo`.
5. Start: `copilot`.
6. Trust the directory only if the user is allowed to share this code context with Copilot.
7. Ask: `Give me an overview of this project.`
8. Run `/help`, then `/skills list` if skills are installed.

## Daily Development Prompts

- Explore a repo: `Give me a high-level overview of this repository and the main execution path.`
- Explain a file: `Explain @src/app.ts and call out risky assumptions.`
- Plan a change: `In plan mode, design an implementation plan for adding SSO support. Ask questions before editing.`
- Fix a bug: `Investigate why the checkout total is wrong, write a minimal fix, and run the relevant tests.`
- Generate tests: `Add focused unit tests for @src/pricing/calculate.ts, including edge cases.`
- Refactor safely: `Refactor this module for readability without changing behavior. Run existing tests afterward.`
- Review changes: `/review` or `/review @src/payments`.
- Write docs: `Update the README usage section to match the current CLI behavior.`
- Summarize Git history: `copilot -p "Summarize the commits on this branch compared with main" --allow-tool='shell(git:*)'`

## GitHub Workflow Prompts

- Issue triage: `Find the likely files involved in issue #123 and propose an implementation plan.`
- Pull request preparation: `Review my current branch, summarize the change, and draft a PR description.`
- CI failure debugging: `Inspect recent test failures, identify the root cause, and suggest the smallest fix.`
- Release notes: `Summarize merged PRs since the last tag into user-facing release notes.`

## Enterprise Rollout Checklist

1. Verify licensing and Copilot CLI policy at organization or enterprise level.
2. Choose the supported install path: WinGet for managed Windows devices, npm where Node.js 22+ is already approved, or Homebrew for macOS/Linux.
3. Confirm corporate proxy, firewall, and certificate requirements with the desktop engineering team.
4. Publish a short internal usage policy covering source code, secrets, regulated data, tool approval, and production deployments.
5. Add repository instructions in `.github/copilot-instructions.md` for build, test, lint, style, branch, and PR rules.
6. Provide a starter prompt pack and a short live demo: explore, plan, edit, test, review, PR.
7. Pilot with a small group, collect examples, then publish the plugin or skills through a marketplace repository.

## Tool Approval Guidance

- Prefer one-time approval for unfamiliar commands.
- Prefer session approval for repeated low-risk commands like `git status`, `git diff`, `npm test`, or project-local lint commands.
- Avoid broad approval for destructive commands, package installation, credential changes, deploys, production writes, and `git push`.
- Use `/reset-allowed-tools` after experiments or when the session context changes.
- In scripted usage, use targeted options such as `--allow-tool='shell(git:*)'` instead of `--allow-all-tools`.

## Publishing This Skill For Users

Project-only skill:

1. Copy the skill directory to `.github/skills/github-copilot-cli-usage`.
2. Commit and push it with the repository.
3. In Copilot CLI, run `/skills reload`.
4. Verify with `/skills info github-copilot-cli-usage`.

Personal skill:

1. Copy the skill directory to `~/.copilot/skills/github-copilot-cli-usage`.
2. Start Copilot CLI or run `/skills reload`.
3. Verify with `/skills list`.

Plugin:

1. Keep this plugin's `plugin.json` at the plugin root.
2. Test locally with `copilot plugin install ./github-copilot-cli-enablement`.
3. Verify with `copilot plugin list` and `/skills list`.
4. Reinstall after edits because Copilot CLI caches installed plugin contents.

Marketplace:

1. Add `.github/plugin/marketplace.json` to the root of a GitHub repository.
2. Include the plugin directory at the source path referenced by `marketplace.json`.
3. Users register the marketplace with `copilot plugin marketplace add ORG/REPO`.
4. Users browse with `copilot plugin marketplace browse org-copilot-cli-enablement`.
5. Users install with `copilot plugin install github-copilot-cli-enablement@org-copilot-cli-enablement`.

## Troubleshooting

- `copilot` not found: restart the terminal, check PATH, or reinstall with the chosen package manager.
- npm install fails: verify Node.js 22 or later and npm registry access.
- Windows launch fails: verify PowerShell v6 or higher.
- Authentication loops: run `copilot login` again, check the browser account, and unset unintended `COPILOT_GITHUB_TOKEN`, `GH_TOKEN`, or `GITHUB_TOKEN` values.
- Access denied after login: ask the GitHub organization owner or enterprise admin to verify Copilot CLI policy.
- The CLI ignores a new skill: run `/skills reload`, verify the folder contains `SKILL.md`, and check `/skills info SKILL-NAME`.
- Plugin changes do not appear: rerun `copilot plugin install ./github-copilot-cli-enablement` because plugin content is cached.
- Users ask about `gh copilot`: explain that current GitHub docs center on standalone `copilot`; only use the old `gh copilot` extension if the organization explicitly maintains it.
