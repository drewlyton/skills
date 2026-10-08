# skills

Drew's personal [pi](https://pi.dev) skills, kept in one repo so every machine
loads the same set. Private: this bundles third-party skills, so it is not
licensed for redistribution (see [CREDITS.md](CREDITS.md)).

## Layout

```
skills/
  <skill-name>/
    SKILL.md          # frontmatter + instructions
    ...               # any supporting files the skill references
package.json          # pi manifest declaring ./skills
```

A skill is a directory with a `SKILL.md`. Pi discovers it from the `skills`
directory configured below.

## Install on a new machine

Clone to a stable path:

```sh
git clone git@github.com:drewlyton/skills.git ~/projects/drewlyton-skills
```

Point pi at the skills directory by adding this to
`~/.pi/agent/settings.json` (`~` is expanded by pi):

```json
{
  "skills": ["~/projects/drewlyton-skills/skills"]
}
```

Restart pi. All skills in the repo are now available as `/skill:<name>`
commands, and the model-invoked ones appear in the system prompt as usual.

> Use the same clone path on every machine so the setting stays consistent.
> If you clone somewhere else, set that absolute path instead.

## Update

```sh
git -C ~/projects/drewlyton-skills pull
```

## Add or edit a skill

Edit in place in the clone and commit. For a new skill, follow pi's
[skill authoring](https://github.com/earendil-works/pi-mono/blob/main/packages/coding-agent/docs/skills.md) rules (a `name` and
`description` in frontmatter; use `disable-model-invocation: true` for
user-invoked skills). Commit and push; `git pull` on the other machine.

## Alternative: install as a pi package

The repo also carries a `pi` manifest, so a machine can manage it as a package
instead:

```sh
pi install git:github.com/drewlyton/skills
```

Pi clones it under `~/.pi/agent/git/` and loads `./skills`. Updates then come
from `pi update --extensions`. Prefer the clone-and-setting route above when you
want to edit skills in place, since pi resets and cleans its managed clone when
the pinned ref changes.

## Credits and licensing

Not all skills here are original; see [CREDITS.md](CREDITS.md). Treat this repo
as private.
