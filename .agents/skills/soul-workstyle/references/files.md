# Where files go, how to delete them

## Where

| File | Place | Reason |
|------|-------|--------|
| Material that must not be public: lists and names of studied subjects (third-party games, authors, sites), files downloaded from other people, accounts and personal data | A local folder that Git ignores. If the project has one, use it (such as `.local/`). This applies also to the output of a script and to intermediate results | The repository can be public. A file that is deleted after a push stays in old commits |
| Intermediate files for this turn only | The scratchpad of the session, or the system temp folder if there is no scratchpad | You discard them after use. They do not belong in the project |
| Intermediate results, downloads and backups of a task inside a repository | `.tmp/<task>/`, with downloads in its `downloads/` | You and the user can find them after the session ends or the context is compressed |
| Probes, repro projects and other generated work outside a repository | An intent folder, see "Generated work" below | The user looks for output beside the script that made it |
| One-off maintenance scripts | `scripts/`, with output in `scripts/out/<script>/` | They stay apart from the project code and are easy to find |
| Products to deliver or to put under version control | The matching folder in the project, also when a script generates them | They are results, not intermediate files |

- In committed docs, comments, tests and commit messages, use only generic descriptions. In tests, use placeholder
  names. Before you commit, run `git grep -i <name>` over the tracked files and read the commit message
- Write a finding from another person's project or screenshots into the repository as the mechanism only. Leave out
  the names of the project, its objects and its variables, and the story of how you found it. Keep the concrete
  case in the conversation or in the ignored folder
- If you collect material over a long time, write it into `.tmp/<task>/` while you collect it. The material then
  survives an interrupted session or a compressed context
- Before you write into `.tmp/` or `.trash/`, confirm that Git ignores the folder: `git check-ignore -q .tmp/x` gives
  exit code 0 for an ignored path. If Git does not ignore it, add it to `.git/info/exclude`, not to the `.gitignore`
  of the project. In a repository that you created, add it to `.gitignore`
- Give a generated file a name that contains its source script and its purpose: `thumbnail-resize.py` makes
  `thumbnail-resize__batch01.png`
- By default, make generated or converted images WebP, and video and audio WebM (VP9 or AV1 video, Opus audio). If
  the target cannot play these formats or the user names another format, use that format. SVG icons and canvas
  textures made in code are exceptions
- If output gets regenerated (a summary, a cache, an index, a generated file), make the fix in its generator or its
  rule. The next run removes an edit to the output

## Generated work

Put generated work outside a repository in the scratchpad, in one folder for each intent. Name the folder for the
intent of the task (`pitfalls-revalidation/`). Split the folder in two:

- `final/`: what the task delivers. Each deliverable has its own folder, with its generator script beside it
  (`final/fill-poly-repeated-point/`: the project, its .zip, `mk_fillpoly_repro.py`)
- `process/`: what you made on the way, each item with its own script beside it: probes, controls, preview plans,
  screenshots, results, sweeps, a project opened only to show the user

By default, a generator writes beside itself (`Path(__file__).parent`). A generator never writes to a path given by
hand, such as `%TEMP%\out`. The user looks for output beside the script that made it. The user must see at once which
files are deliverables and which are by-products.

## Agent tooling: hooks, agents, skills, memory

First ask: does the tooling depend on the files of the repository?

- If not, it belongs to the user. Put it under `~/.claude/`: sub-agents in `agents/`, skills in `skills/`,
  cross-project memory in `memory/`, hooks in `settings.json`
- If it does, put client-neutral scripts in the `.agents/` folder of the repository. Put only the Claude-only
  registration in `.claude/`, because Claude Code loads sub-agents only from `.claude/agents/` and skills only from
  `.claude/skills/`

A habit that the user has in every project does not belong in one repository. In one repository, it misses the other
projects and forces the preferences of the user on contributors.

Tooling that an agent carries into another project follows the Agent Skills layout (https://agentskills.io):
`SKILL.md` with a name and a description, `scripts/`, `references/`, `assets/`. Give each script a `--help` option and
errors that say what to do. Do not add interactive prompts to a script. Keep `SKILL.md` under 500 lines. The entry
documents tell an arriving agent to install the tooling itself, so the user does not copy files by hand. Before you
claim that a client finds the tooling, check the discovery folder of that client: Claude Code `.claude/skills/`, TRAE
`.trae/skills/`, most others `.agents/skills/`.

A tool that you build writes only inside the project that it serves. It creates no dotfile, config folder or
environment variable in the home folder of the user. If a later session must find something again (such as the
location of a clone), the tool prints it and tells the agent to remember it. The memory of the agent keeps it; the
folders of the user belong to the user. A default location is a filled-in example that the user can replace, not a
requirement.

A rule that holds in every project and shapes how work is done belongs in these soul skills. Machine facts and narrow
procedures (one tracker, one tool) stay in memory.

## Deleting or replacing

First confirm the exact target. Then look at its Git state. If the user asks you to keep the useful items of a set
(logs, sessions, old folders), inspect every item before you delete any item. Move an item that you did not inspect to
`.trash/`.

| Target | Action |
|--------|--------|
| Build output, dependencies, caches and intermediate files that can be regenerated | Delete |
| Tracked, with the current version committed | Change or delete it; Git can restore it |
| Tracked, with uncommitted changes | First run `git diff HEAD --output=.tmp/<task>/<file>.patch -- <file>` and say where the backup is. Use `git stash -u` only when the operation covers the whole working tree |
| Untracked and worth recovering, especially the user's own files | Move to `.trash/<date>/<original relative path>`. The path stays, so files with the same name do not overwrite each other |
| Credentials, keys, personal data | Do not copy them into `.trash/`. Ask the user before you delete or keep them |
