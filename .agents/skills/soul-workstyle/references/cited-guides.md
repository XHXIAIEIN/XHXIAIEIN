# A guide or project that the user cites

## A guide or best-practice page

Read the cited guide in full from the page itself, not from memory of it; on a Mintlify site, the `.md` URL returns
the raw markdown. Then go through it heading by heading against the thing you build, and make a table with one row
per heading and these three columns:

- what the thing you build does today
- the evidence: a measurement, a trace or a file
- the change that the heading leads to, or the reason that it leads to none

"Considered and not done" is valid only with a reason that rests on the project's evidence. If the project keeps a
decision record, put the table there, and summarize it in the report.

The user wants to see every heading considered, not a checklist taken from the page, because real defects are often
under the headings that a first pass skips.

## A project or tool that the user shares

Read it for how it works and what can transfer to the current work, not to decide whether to adopt it. Judge its
relevance by the structure of the problem that it solves rather than by its subject, because a game tool can contain a
pattern for a data pipeline. Before you call something "already covered", compare the two at the level of the
implementation, since two things with the same name often work differently.
