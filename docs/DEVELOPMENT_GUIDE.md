# Development Guide

This project uses Mise (https://mise.jdx.dev/). At the moment, there is no dedicated development container, thus you need to configure your local development environment following the steps described below.

## Pre-requisites

- [Mise](https://mise.jdx.dev/) >= 2026.2.4

## Preparing your Build Environment

| Action                                                                       |                                                                                                                                                                                                                    |
| :--------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Open the [repository](https://github.com/awslabs/agent-plugins).             | As you are reading this file from the repo, you are probably already there.                                                                                                                                        |
| Using the "fork" button in the upper right, fork the repo into your account. | Some git/GitHub expertise is assumed.                                                                                                                                                                              |
| Clone forked repo to your local development environment.                     | If you wish to work off a branch in your repository, create and clone that branch. You will create a PR back to `main` in the agent-plugins repository eventually, you can do that from fork/main or fork/_branch_ |
| `cd agent-plugins`                                                           | This is the home directory of the repo and where you will open your text editor, run builds, etc.                                                                                                                  |
| `code .`                                                                     | Opens the project in VSCode. You can use the editor of your choice, just adapt this step to your specific use case.                                                                                                |
| `mise install`                                                               | This command will install the tools required for the project and environmental variables                                                                                                                           |

## Claude Code Setup

This project uses Claude Code plugins for development workflows. The project-level `.claude/settings.json` pre-configures enabled plugins via `enabledPlugins`, but you must first register the marketplace(s) it references.

### One-time marketplace setup

Run this once inside a Claude Code session:

```bash
/plugin marketplace add awslabs/agent-plugins
```

After adding the marketplace, the plugins listed in `.claude/settings.json` will activate automatically for this project.

### PR contributor statement

The project-level `attribution.pr` setting automatically appends the required contributor statement to pull request descriptions created by Claude Code, so the `contributorStatement` CI check passes without manual copy-paste.

## GitHub Copilot Setup

When using GitHub Copilot coding agents, repository-level guidance is provided in `.github/copilot-instructions.md`.

### Recommended setup flow

1. Open the repository in your Copilot-supported environment (for example, GitHub or VS Code with Copilot enabled).
2. Start a Copilot coding agent session in this repository.
3. Verify `.github/copilot-instructions.md` is available to the session context before asking for changes.
4. Prompt with AWS-focused intents (for example, deployment planning, cost estimation, or IaC generation) so the agent can apply the matching plugin skills under `plugins/`.
5. Validate generated changes with this repository's standard checks (`mise build`) before committing.

日本語で案内する場合の推奨フロー:

1. GitHub や VS Code など、Copilot が有効な環境でリポジトリを開く。
2. このリポジトリを対象に Copilot コーディングエージェントのセッションを開始する。
3. 変更依頼の前に `.github/copilot-instructions.md` がセッションに読み込まれていることを確認する。
4. デプロイ計画・コスト見積もり・IaC 生成など AWS 指向の依頼を日本語で行い、`plugins/` 配下のスキル適用を促す。
5. コミット前に `mise build` で標準チェックを実行し、生成内容を検証する。

Keep `.github/copilot-instructions.md` aligned with current plugin support and workflows so Copilot sessions follow the same project conventions as Claude Code and Cursor.

## Working on Your Contribution

| Action                                            | Explanation                                                                                                                                                                                                                                                                                     |
| :------------------------------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (optional)<br/>`git checkout -b your-branch-name` | If you're working in a different branch than main in your forked repo and haven't changed your local branch, now is a good time to do so.                                                                                                                                                       |
| _Do all your code editing_                        | Work with your AI assistant to edit the code, validate and verify.                                                                                                                                                                                                                              |
| `mise build`                                      | This is the build command for the project. It will compile and run all the quality gates, and run the unit and integration tests. If you make any substantive changes to the code, you will almost certainly see some or all of the tests fail. Before you commit, you should run a full build. |

## Security Scanning

### Gitleaks - Secret Detection

This repository uses [gitleaks](https://github.com/gitleaks/gitleaks) to detect secrets and sensitive information in the codebase.

#### Handling False Positives

If gitleaks reports a false positive (e.g., example API keys in documentation, test fixtures), you can add it to the baseline file to suppress future warnings.

1. Run gitleaks locally to generate the baseline:

   ```bash
   gitleaks git --config=.gitleaks.toml --report-format=json . > .gitleaks-baseline.json
   ```

2. Review the generated file to ensure only legitimate false positives are included.

3. Commit the updated `.gitleaks-baseline.json` file.

#### Configuration

Custom rules and allowlists are defined in `.gitleaks.toml`. Common customizations include:

- Excluding paths (vendor directories, generated files)
- Allowlisting specific patterns or files
- Adding custom secret detection rules
