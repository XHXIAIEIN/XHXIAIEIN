# Project text

Project text is text that stays in a project. Its reader is the person or agent who later uses or maintains the
project. That reader was not in this session, so the text says what the project is and does, and why. Test each
sentence by asking whether the reader would understand, act or maintain the project differently without it; if not,
delete it.

## Wording

- Apply all the sentence rules in the "Sentences" section of SKILL.md, still without the approved-word dictionary
- Say what a thing is, what it does and how to use it, not what it lacks or does not need ("no vector index, nothing
  to install"). A list of absent things makes the reader think of them, while the positive statement is shorter and
  tells the reader what to do: "runs on the clone's files". A real prohibition stays, together with the action to
  take instead
- Do not use exclamation marks, slogans or unbacked words such as "powerful", "seamless" or "one-click"
- For punctuation and spacing between Chinese and Latin text, follow the habit that the file already has
- If the tone is hard to get, write two or three variants in different registers and assemble the sentences that
  work. This works better than many edits of one draft, because each variant usually contains some of the right
  sentences

## Only the present

Git, the changelog and decision records hold the history, so all other text describes the current state.

- Do not use words that are relative to a change, such as "now", "no longer", "new", "fixed" or "updated". The reader
  never saw the old version, so these words tell them nothing
- Describe the result as it is, without mentioning the conversation or the task: "as requested", "raised in review",
  "the earlier approach", "the user said"
- After a correction, rewrite the affected titles, openings, labels, file names and descriptions from the final
  state. Do not patch the old sentence with parentheses, synonyms, a "note" or a compliance disclaimer. Check the most
  visible places first: the document title, the opening, the PR title, captions, file names and unpushed commit
  messages
- Write about a rejected option only if it touches an architectural constraint, compatibility or security, or if it
  prevents a later wrong change. Even then, write it as a constraint ("X cannot be used here because Y"), not as
  history
- Put unfinished work in the report, not as a `TODO` in the file

When these rules hold, a reader who was not in this session cannot tell that another option ever existed.

## Comments

A comment gives a step or a piece of data a short name, so that the reader knows what the code does without reading
every line.

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

- Write comments in the language that the file already uses; a new file follows the project. The examples show only
  the form
- Write the shortest phrase ("Round the angle", "Player turns left"), without filler words or "so that ..." clauses
- Name the domain event, not the mechanism: "Player turns left", not "set angle to Self.Angle - 90"; "Skip downloaded
  files", not "continue if the path exists"
- If code looks redundant or odd because of a quirk, a trap or a constraint, and someone might remove it, add the
  reason on its own line, as in the "Destroy a frame later" example above. Other code does not get a reason line
- When a name already says what the code does, the code needs no comment. Above code that the reader would have to
  read in full to know what it does, write a short phrase
- A branch names its case and `else` names the rest: "Cache expired: download the index again", "Otherwise stop
  sliding"
- A variable or a setting says what it holds, with its unit or range, and a boolean says the fact that it stands for:
  "Player turn time in seconds", "Screen touched"
- In a long block, a short phrase before each step works as its heading. Keep the block as one block instead of
  splitting it into functions to make room for comments
- A function's description is also a phrase. For its parameters, add only what the signature leaves unclear: unit,
  range, side effects, exceptions
- Do not translate code line by line: give each step one name, not each line one comment
- State facts about the code instead of praise such as "standard", "clean" or "robust", and do not add section rules
  to a simple file

## Docs and README

- A page that introduces a tool or a concept says in its first sentence what the thing is and what problem it solves
- Order the sections by what the reader does (install, configure, use, build), not by the internal module structure
- Write steps in the imperative with one action each, and write conditional behaviour as "If X, Y happens"
- Check commands, paths, settings and interface names against the implementation before you write them, and run what
  you can run
- Keep facts that change with data or versions out of the text: version numbers, entry counts, test totals, the
  result of one count. Describe the behaviour instead, and point to the file, command or manual that produces the fact
- Let the length set the structure: a short text has no headings, bold does not go everywhere, and two or three items
  do not need a table. Parallel items go in lists, while causes and reasoning go in paragraphs
- If a thing needs a long explanation before people can use it correctly, consider changing the thing itself

## UI copy

- Use the objects that the user sees and the names on the screen, never IDs, field names or internal flows
- A button is a verb that says what will happen, and a hint names the key or the entry point: "Press Space to start"
- An error message says what happened and what the user can do, without blaming the user

## Commit messages and PRs

- Follow the language and format that the repository already uses, so read `git log` before you write
- The title says what changed; "cleanup" and "misc fixes" say nothing
- The body holds only what the diff does not show: motivation, constraints and impact, without a list of files
- A PR description states the final behaviour and the trade-offs that a reviewer cannot recover from the diff. It does
  not follow a Summary, Changes and Test plan template, and it does not describe intermediate states that were never
  merged
- Never write `#N` or `owner/repo#N` in a commit message, an issue or a PR, however the text gets there (`-m`,
  `--body`, `gh`), because GitHub then adds the text to that issue's timeline permanently. Name the issue in words, or
  give its full URL in backticks. A document inside a repository may use the short form, because GitHub parses only
  commits, issues and PRs

## Text agents read

Agents read AGENTS.md, CLAUDE.md, prompts, skills and memory files and follow them literally, so follow these rules
when you write them:

- Write it in English (see the language table in SKILL.md), unless the project has its own rule
- Give each rule its reason, so that the agent can judge the cases that the text does not cover
- Keep hard numbers, "must", "never" and capitals for real hard constraints, because an agent applies any other
  emphasis too widely
- Next to a prohibition, say what to do instead
- Agents copy examples, so take examples from different domains or say which point an example shows
- A prompt for review, checking or verification gets plain instructions and permission to say "not sure", not an
  expert persona, because a persona makes the agent sound certain rather than be right
- Leave out what an agent does by default. If a script, a check or a test can guarantee a rule, put the rule there
  instead of in text that agents read every time
- A skill's description says when to use the skill, and so it decides whether the agent loads the instructions at all

Some agents run on small models, and guidance for them works only where they act:

- Make the correct form the default of a helper, and let the tool's output say in one line what to write. A small
  model does not open extra prose
- A small model takes the first line of a tool's output as the whole answer, so it reads a denial printed before the
  hit as the answer
- A guard for a small model must not take space in a capable model's context. A line that the tool prints once, when
  it matters, is acceptable; permanent prose that every agent carries is not

A memory file holds a rule, a fact or a pointer, together with the reason that it holds. Its first line says what to
do, and `Why:` gives the reason in one or two sentences, as a reason rather than as an incident. The index line says
when to open the file.

Leave dates, session events and the story of how the fact was learned out of a memory file ("on <date> the user found
...", "I tested ... and was corrected"), and list open items without "as of" dates. A dated story takes time to read
and gives the agent nothing to act on.
