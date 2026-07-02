# GitHub Copilot CLI Enablement

Reusable onboarding guidance, workflows, prompt examples, and guardrails for organization users of the standalone GitHub Copilot CLI.

Start with the full guide:

- `github-copilot-cli-enablement/docs/github-copilot-cli-guide.md`

## Local Plugin Test

From this repository root:

```powershell
copilot plugin install ./github-copilot-cli-enablement
copilot plugin list
```

Then start Copilot CLI and verify:

```text
/skills list
/skills info github-copilot-cli-usage
```

## Marketplace Install

After this repository is available on GitHub:

```powershell
copilot plugin marketplace add raviesh87/rav-flux.githubcopilotcli
copilot plugin marketplace browse org-copilot-cli-enablement
copilot plugin install github-copilot-cli-enablement@org-copilot-cli-enablement
```

## Contents

- `.github/plugin/marketplace.json`: Copilot CLI marketplace manifest.
- `github-copilot-cli-enablement/plugin.json`: Copilot CLI plugin manifest.
- `github-copilot-cli-enablement/skills/github-copilot-cli-usage/SKILL.md`: Agent skill entrypoint.
- `github-copilot-cli-enablement/skills/github-copilot-cli-usage/references/`: Detailed platform install and use-case references.
