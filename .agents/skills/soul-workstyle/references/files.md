# Where files go, how they are deleted

## Where

| File | Place | Reason |
|------|-------|--------|
| Material that must not be public: lists and names of studied subjects (third-party games, authors, sites), files downloaded from other people, accounts and personal data | A local folder that Git ignores, the project's own if it has one (such as `.local/`). This holds even for a script's output or an intermediate result | The repository may be public, and a file deleted after a push still exists in old commits |
| Intermediate files used only in this turn | The session's scratchpad, or the system temp folder when there is no scratchpad | You delete them after use, so they do not belong in the project |
| A task's intermediate results, downloads and backups inside a repository | `.tmp/<task>/`, with downloads in its `downloads/` | You and the user can still find them after the session ends or the context is compressed |
| Probes, repro projects and other generated work outside a repository | An intent folder, see "Generated work" below | The user looks for output beside the script that made it |
| One-off maintenance scripts | `scripts/`, with output in `scripts/out/<script>/` | They stay apart from the project code but are easy to find |
| Products to deliver or to put under version control | The matching folder in the project, even when a script generates them | They are results, not intermediate files |

- Committed docs, comments, tests and commit messages use only generic descriptions, and tests use placeholder names.
  Before you commit, run `git grep -i <name>` over the tracked files and read the commit message
- A finding from someone else's project or screenshots goes into the repository as the mechanism only, without
  project, object or variable names and without the story of how you found it. The concrete case stays in the
  conversation or in the ignored folder
- Material that you collect over a long time goes into `.tmp/<task>/` while you collect it, so that it is still there
  after an interrupted session or a compressed context
- Before you write into `.tmp/` or `.trash/`, confirm that Git ignores it with `git check-ignore -q .tmp/x`, which
  exits with 0 for an ignored path. If it is not ignored, add it to `.git/info/exclude` rather than to the project's
  `.gitignore`; in a repository that you created yourself, add it to `.gitignore`
- A generated file's name carries its source script and its purpose: `thumbnail-resize.py` makes
  `thumbnail-resize__batch01.png`
- Generated or converted media default to WebP for images and to WebM (VP9 or AV1 video, Opus audio) for video and
  audio, unless the target cannot play them or the user names another format. SVG icons and canvas textures made in
  code are exceptions
- Fix output that gets regenerated (a summary, a cache, an index, a generated file) in its generator or its rule,
  because the next run overwrites any edit to the output

## Generated work

Generated work outside a repository goes into the scratchpad, in one folder per intent that is named for what the
task is for (`pitfalls-revalidation/`). The folder has two parts:

- `final/` holds what the task delivers, one folder each, with its generator script beside it
  (`final/fill-poly-repeated-point/`: the project, its .zip, `mk_fillpoly_repro.py`)
- `process/` holds what you made on the way, each item with its own script beside it: probes, controls, preview plans,
  screenshots, results, sweeps, a project opened only to show the user

A generator writes beside itself by default (`Path(__file__).parent`) and never to a path passed by hand, such as
`%TEMP%\out`, because the user looks for output next to the script that made it and must tell the deliverables from
the by-products at once.

## Agent tooling: hooks, agents, skills, memory

First ask whether the tooling depends on the repository's files:

- If it does not, it belongs to the user and goes under `~/.claude/`: sub-agents in `agents/`, skills in `skills/`,
  cross-project memory in `memory/`, hooks in `settings.json`
- If it does, client-neutral scripts go in the repository's `.agents/` and only the Claude-only registration goes in
  `.claude/`, because Claude Code loads sub-agents only from `.claude/agents/` and skills only from `.claude/skills/`

A habit that the user keeps across projects does not belong in one repository, because there it misses the other
projects and forces the user's preferences on contributors.

Tooling that an agent carries into another project follows the Agent Skills layout (https://agentskills.io): `SKILL.md`
with a name and a description, `scripts/`, `references/`, `assets/`. Its scripts have `--help`, errors that say what to
do and no interactive prompts, and its `SKILL.md` stays under 500 lines. The entry documents tell an arriving agent to
install the tooling itself, so that the user does not copy files by hand. Before you claim that a client finds the
tooling, check that client's discovery folder: Claude Code `.claude/skills/`, TRAE `.trae/skills/`, most others
`.agents/skills/`.

A tool that you build writes only inside the project that it serves, never a dotfile, a config folder or an
environment variable in the user's home folder. When a later session must find something again (such as where a clone
is), the tool prints it and tells the agent to remember it. The agent's memory stores it, because the user's folders
belong to the user. A default location is a filled-in example that the user can replace, never a
requirement.

A rule that holds in every project and shapes how work is done belongs in these soul skills, while machine facts and
narrow procedures (one tracker, one tool) stay in memory.

## Deleting or replacing

Confirm the exact target first, then look at its Git state. When the user asks you to keep what is useful from a set
(logs, sessions, old folders), inspect every item before you delete any of them. Move an item that you did not inspect
to `.trash/` instead of deleting it.

| Target | Action |
|--------|--------|
| Build output, dependencies, caches and intermediate files that can be regenerated | Delete |
| Tracked, with the current version committed | Change or delete it, because Git can restore it |
| Tracked, with uncommitted changes | First run `git diff HEAD --output=.tmp/<task>/<file>.patch -- <file>` and say where the backup is. Use `git stash -u` only when the operation covers the whole working tree |
| Untracked and worth recovering, especially the user's own files | Move it to `.trash/<date>/<original relative path>`, which keeps the path so that files with the same name do not overwrite each other |
| Credentials, keys, personal data | Do not copy them into `.trash/`; ask the user before you delete or keep them |
