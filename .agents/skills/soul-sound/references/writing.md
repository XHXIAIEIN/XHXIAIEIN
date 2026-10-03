# Project text

Project text is text that stays in a project. Its reader is a person or an agent who later uses or maintains the
project. That reader was not in this session. The reader needs to know what the project is and does, and why. For
each sentence, ask: without it, would the reader understand, act or maintain the project differently? If not, delete
the sentence.

## Wording

- Apply all the sentence rules of ASD-STE100 in the "Sentences" section of SKILL.md. Conversation applies them at
  about 80%. Project text applies all of them, still without the approved-word dictionary
- Say what a thing is, what it does and how to use it. Do not list what it lacks or does not need ("no vector index,
  nothing to install"). A list of absent things makes the reader think of them. Say what the thing runs on instead
  ("runs on the files of the clone"). Keep a real prohibition, with the action to take instead
- Do not use exclamation marks, slogans or claims without evidence, such as "powerful", "seamless", "one-click"
- For punctuation and spacing between Chinese and Latin text, follow the existing habit of the file
- If the tone is hard to get, write two or three variants in different registers. Then take the sentences that work
  from each variant. This works better than many edits of one draft, because each variant usually has some of the
  right sentences

## Only the present

Git, the changelog and decision records hold the history. All other text describes the current state.

- Do not use words that are relative to a change, such as "now", "no longer", "new", "fixed", "updated". The reader
  did not see the old version, so these words tell the reader nothing
- Describe the result as it is. Do not mention the conversation or the task: "as requested", "raised in review", "the
  earlier approach", "the user said"
- After a correction, rewrite the affected titles, openings, labels, file names and descriptions from the final state.
  Do not patch the old sentence with parentheses, synonyms, a "note" or a compliance disclaimer. Check the most
  visible places first: the document title, the opening, the PR title, captions, file names and unpushed commit
  messages
- Write about a rejected option only if it touches an architectural constraint, compatibility or security, or if it
  prevents a later wrong change. Write it as a constraint ("X cannot be used here because Y"), not as history
- Put unfinished work in the report, not as a `TODO` in the file

The result: a reader who was not in this session cannot see that another option existed.

## Comments

A comment gives a short name to a step or to a piece of data. The reader then sees what the code does without
reading all of it.

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

- Write comments in the language that the file already uses. A new file follows the project. The examples show only
  the form
- Write the shortest phrase ("Round the angle", "Player turns left"). Leave out filler words and "so that ..." clauses
- Name the domain event, not the mechanism: "Player turns left", not "set angle to Self.Angle - 90"; "Skip downloaded
  files", not "continue if the path exists"
- If code looks redundant or odd because of a quirk, a trap or a constraint, and someone might remove it, add the
  reason on its own line, as "Destroy a frame later" does above. Other code gets no reason line
- If a name already says what the code does, add no comment. Above code that the reader must read in full to
  understand, write a short phrase
- In a branch, name its case. In `else`, name the rest: "Cache expired: download the index again", "Otherwise stop
  sliding"
- For a variable or a setting, say what it holds, with its unit or range. For a boolean, say the fact that it stands
  for: "Player turn time in seconds", "Screen touched"
- In a long block, put a short phrase before each step as its heading. Keep the block as one block. Do not split it
  into functions only to make room for comments
- Write the description of a function as a phrase too. For parameters, add only what the signature does not make
  clear: unit, range, side effects, exceptions
- Do not translate code line by line. Give one name to each step, not one comment to each line
- State facts about the code. Do not praise it ("standard", "clean", "robust")
- Do not add section rules to a simple file

## Docs and README

- On a page that introduces a tool or a concept, make the first sentence say what it is and what problem it solves
- Order the sections by what the reader does (install, configure, use, build), not by the internal module structure
- Write steps in the imperative, with one action in each step. Write conditional behaviour as "If X, Y happens"
- Before you write a command, a path, a setting or an interface name, check it against the implementation. Run what
  you can run
- Keep facts that change with data or versions out of the text: version numbers, entry counts, test totals, the
  result of one count. Describe the behaviour, and point to the file, command or manual that gives the fact
- Let the length set the structure. A short text gets no headings. Do not put bold everywhere. Do not make a table
  for two or three items. Put parallel items in lists, and causes and reasoning in paragraphs
- If a thing needs a long explanation before people can use it correctly, consider a change to the thing itself

## UI copy

- Use the objects that the user sees and the names on the screen. Do not show IDs, field names or internal flows
- Write a button as a verb that says what will happen. Make a hint name the key or the entry point: "Press Space to
  start"
- Make an error message say what happened and what the user can do. Do not blame the user

## Commit messages and PRs

- Follow the language and the format that the repository already uses. Read `git log` before you write
- Make the title say what changed. Do not use "cleanup" or "misc fixes"
- Put in the body only what the diff does not show: motivation, constraints, impact. Do not list the files
- In a PR description, state the final behaviour and the trade-offs that a reviewer cannot get from the diff. Do not
  use a Summary, Changes and Test plan template. Do not describe intermediate states that were never merged
- Do not write `#N` or `owner/repo#N` in a commit message, an issue or a PR, however the text gets there (`-m`,
  `--body`, `gh`). GitHub adds the reference to the timeline of that issue permanently. Name the issue in words, or
  give the full URL in backticks. A document inside a repository can use the short form, because GitHub parses only
  commits, issues and PRs

## Text agents read

Agents read AGENTS.md, CLAUDE.md, prompts, skills and memory files, and follow them literally. Therefore:

- Write this text in English (see the language table in SKILL.md). A rule of the project takes precedence
- Give each rule its reason, so the agent can judge the cases that the text does not cover
- Keep hard numbers, "must", "never" and capitals for real hard constraints. An agent applies any other emphasis too
  widely
- Next to a prohibition, say what to do instead
- Agents copy examples. Use examples from different domains, or say which point an example shows
- For a prompt that reviews, checks or verifies, give plain instructions and permission to say "not sure". Do not give
  an expert persona, because a persona makes the agent sound certain, not be right
- Leave out what an agent does by default. If a script, a check or a test can guarantee a rule, put the rule there,
  not in text that agents read every time
- The description of a skill says when to use the skill. The description decides whether the agent loads the
  instructions

Some agents run on small models. For them:

- Put the guidance where a small model acts. Make the correct form the default of a helper. Put a line in the output
  of the tool that says what to write. A small model does not open additional prose
- A small model takes the first line of a tool's output as the whole answer. If a denial comes before the hit, the
  small model reads the denial as the answer
- A guard for a small model must not take space in the context of a capable model. A line that the tool prints once,
  when it matters, is acceptable. Permanent prose that every agent carries is not acceptable

A memory file holds a rule, a fact or a pointer, and the reason that it holds. The first line says what to do.
`Why:` gives the reason in one or two sentences, as a reason, not as an incident. The index line says when to open
the file.

Leave out dates, session events and how the fact was learned ("on <date> the user found ...", "I tested ... and was
corrected"). List open items without "as of" dates. A dated story takes time to read and gives the agent nothing to
act on.
