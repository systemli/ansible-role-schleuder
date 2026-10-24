# AGENTS.md

Ansible role that installs and configures Schleuder (mailing list manager, with optional schleuder-cli, web and GitLab ticket plugin) on Debian.

## Layout

- `defaults/main.yml`: all role variables with defaults
- `tasks/`: `main.yml` imports `schleuder.yml`, `schleuder_cli.yml`, `schleuder_web.yml`, `schleuder_lists.yml`, `schleuder_gitlab_ticket_plugin.yml` and `goss.yml`
- `templates/`, `files/`, `handlers/`, `meta/main.yml`: role content and metadata
- `molecule/default/`: Molecule scenario (Docker, Debian 13) with goss tests in `tests/`
- `requirements.yml`: collections needed by the role (`community.general`)

## Checks

- `ansible-lint .` must not report new findings (config in `.ansible-lint`, `molecule/` and `.github/` are excluded)
- `yamllint .` uses `.yamllint`
- `molecule test` runs the role in a Docker container; CI runs it on Debian 12 and 13 via the shared integration workflow
- `ansible-lint` creates `.ansible/`; it is git ignored, never commit it

## Conventions

- Use fully qualified module names, quoted octal `mode` values and `true`/`false` booleans
- Give every task a `name`; `# noqa` comments need a reason
- New or changed role variables need an entry in `defaults/main.yml` and the README

## Commits and PRs

- Write in English, use conventional commit titles (`feat:`, `fix:`, `chore:`, `ci:`; `!` for breaking changes)
- The PR title sets the release-drafter label (`enhancement`, `bug`, `chore`, `breaking`); a breaking change bumps the major version
- Open PRs as draft and fill in `.github/pull_request_template.md`: what changed and why, plus a breaking changes section if needed
- Do not list changed files or claim test results in the description; CI covers that
