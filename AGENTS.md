# AGENTS.md

Ansible role that adds third-party APT repositories (deb822 or one-line style) on Debian (trixie, bookworm) and Ubuntu (noble), including signing keys and package pinning.

## Layout

- `defaults/main.yml`: role variables (`apt_repositories`, `apt_repositories_absent`, ...)
- `tasks/`: `main.yml`, `repo.yml` (one repository per loop item), `key_path.yml`
- `vars/*.yml`: repository presets (one file per preset, e.g. `gitlab.yml`); `null.yml` is the empty preset
- `files/`: binary signing keys referenced via `key_path`
- `templates/`: `repo.sources.j2` (deb822), `repo.list.j2` (one-line), `pref.j2` (pinning)
- `molecule/default/`: converge playbook and tests; CI supplies its own Docker scenario

## Checks

- `ansible-lint .` must pass (it creates `.ansible/`; git ignored, never commit it)
- `yamllint .` uses the repo's `.yamllint`
- `molecule test` runs the role in a Docker container; CI runs it on Debian 12/13 and Ubuntu 24.04
- A new preset needs a `vars/<name>.yml`, its key in `files/` if binary, and an entry in the README list

## Conventions

- Use fully qualified module names, quoted octal `mode` values and `true`/`false` booleans
- Give every task a `name`
- Presets may be partially overridden by the user, keep their keys consistent with the documented variables

## Commits and PRs

- Write in English, use conventional commit titles (`feat:`, `fix:`, `chore:`, `ci:`; `!` for breaking changes)
- The PR title sets the release-drafter label (`enhancement`, `bug`, `chore`, `breaking`); a breaking change bumps the major version
- Open PRs as draft and fill in `.github/pull_request_template.md`: what changed and why, plus a breaking changes section if needed
- Do not list changed files or claim test results in the description; CI covers that
