# Writing that stays in a project

The reader is whoever later uses or maintains the project, person or model. They were not in this session; they need
to know what is and why. Ask of every sentence: without it, would the reader's understanding, actions or maintenance
decisions change? If not, delete it.

## Only the present

History belongs to git, the changelog and decision records; everywhere else describes the current state.

- No words relative to a change, such as "now", "no longer", "new", "fixed", "updated": the reader never saw the old
  version, so they are noise
- No mention of the conversation or the task: "as requested", "raised in review", "the earlier approach", "the user said"
- After a correction, rewrite the affected titles, openings, labels, file names and descriptions from the final
  state; do not patch the old sentence with parentheses, synonyms, "note" or compliance disclaimers. Check the most
  visible places first: document title, opening, PR title, caption, file name, unpushed commit messages
- A rejected option is written only when it touches an architectural constraint, compatibility or security, or would
  stop a later mistaken change; write it as a constraint ("X cannot be used here because Y"), not as a history
- Unfinished work goes in the report, not as a `TODO` in the file

A reader who was not in this session cannot tell from anywhere that another option once existed.

## Comments

A comment gives a step or a piece of data a short name, so the reader sees at a glance what it does.

```js
// Screen touched
if (touch.isTouching) {
  // Round the angle
  angle = Math.round(angle);
  // Player turns left
  player.angle -= 90;
}

// Destroy a frame later
// Destroyed in the same frame, the collision event cannot read the object
await nextFrame();
obj.destroy();
```

Not this:

```js
// If the screen has been touched, round the angle so that no value like 89.99999999999999 is left
if (touch.isTouching) {
  // get x
  const x = pos.x;
}
```

- Comments follow the language the file already uses, a new file follows the project; the examples show the form only
- Write the shortest phrase ("Round the angle", "Player turns left"), without filler words or "so that ..." clauses
- Name the domain event, not the mechanism: "Player turns left", not "set angle to Self.Angle - 90"; "Skip downloaded
  files", not "continue if the path exists"
- Only when code looks redundant or odd because of a quirk, trap or constraint, and someone might remove it, add the
  reason on its own line, like "Destroy a frame later" above
- What a name already says needs no comment; above code that has to be read in full to know what it does, write a
  short phrase
- A branch names its case, `else` names the rest: "Cache expired: download the index again", "Otherwise stop sliding"
- A variable or setting says what it holds, with unit or range; a boolean says the fact it stands for: "Player turn
  time in seconds", "Screen touched"
- In a long block, a short phrase between steps works as a signpost; the block stays one block, not split into
  functions to make room for comments
- A function's description is also a phrase; parameters only add what the signature leaves unclear: unit, range, side
  effects, exceptions
- Do not translate code line by line: one name per step, not one comment per line. No self-praise ("standard",
  "clean", "robust"); no section rules in a simple file

## Docs and README

- A page introducing a tool or concept says in its first sentence what it is and what problem it solves
- Sections follow what the reader does (install, configure, use, build), not the internal module structure
- Steps are imperative, one action each; conditional behaviour is "If X, Y happens"
- Check commands, paths, settings and interface names against the implementation before writing them; run what can
  be run
- Facts that change with data or versions (version numbers, entry counts, test totals, the result of one count) stay
  out of the text: describe the behaviour and point to the file, command or manual that produces it
- Structure follows length: no headings in a short text, no bold everywhere, no table for two or three items; parallel
  items in lists, cause and reasoning in paragraphs
- Something that needs a long explanation to be used correctly: consider changing the thing itself

## UI copy

- Use the objects the user sees and the names on screen, never IDs, field names or internal flows
- Buttons are verbs saying what will happen; hints name the key or the entry point: "Press Space to start"
- Error messages say what happened and what the user can do, without blaming the user

## Commit messages and PRs

- Follow the repository's existing language and format; look at `git log` before writing
- The title says what changed, never "cleanup" or "misc fixes"
- The body holds only what the diff does not show: motivation, constraints, impact; no file list
- A PR description states the final behaviour and the trade-offs a reviewer cannot recover from the diff; no Summary,
  Changes, Test plan template, no intermediate states that were never merged
- Never write `#N` or `owner/repo#N` in a commit message, issue or PR text, however it is passed (`-m`, `--body`,
  `gh`): GitHub posts it to that issue's timeline for good. Name the issue in words or as the full URL in backticks.
  A document inside a repository may use the short form; only commits, issues and PRs are parsed

## Text models read

AGENTS.md, CLAUDE.md, prompts, skills and memory files are read by models, which follow them literally, so:

- Write them in English (see the language table in SKILL.md); a project's own rule wins
- Give rules their reasons, so the model can judge the cases the text does not cover
- Keep hard numbers, "must", "never" and capitals for real hard constraints; anything else gets over-applied
- Next to a prohibition, say what to do instead
- Examples get copied: vary the domain, or say which point the example shows
- A prompt for review, checking or verification gets plain instructions and permission to say "not sure", no expert
  persona: a persona makes the model sound certain rather than be right
- Leave out what the model does by default; what a script, check or test can guarantee goes there, not into text read
  every time
- Guidance meant for small models goes where they act: the good shape as the default in a helper, and a line in a
  tool's output that says what to write, not more prose they never open. The first line of a tool's output is the
  whole answer to a small model, so a denial printed before the hit is read as the answer. A guard for a small model
  must not cost a capable one context: a line printed once when it matters passes, permanent prose every agent
  carries does not
- A skill's description says when to use it; it decides whether the instructions get loaded

A memory file holds the rule, fact or pointer and why it holds. The first line is what to do; `Why:` is one or two
sentences of reason, stated as a reason, not as an incident. Leave out dates, session events and how it was learned
("on <date> the user found ...", "I tested ... and was corrected"); open items are listed without "as of" dates. The
index line says when to open the file. A dated anecdote costs reading time and tells the agent nothing it acts on.

## Wording

- When the tone is hard to get, write two or three variants in different registers and assemble the sentences that
  work, rather than polishing one draft; the right sentences are usually already scattered across the versions
- Go nearer to ASD-STE100 than conversation does (see SKILL.md), still without its dictionary: one concept keeps one
  name throughout; active voice and direct verbs, "delete the cache", not "perform a deletion operation on the cache"
- No exclamation marks, slogans, or unbacked words such as "powerful", "seamless", "one-click"
- Punctuation and spacing between Chinese and Latin text follow the file's existing habit
