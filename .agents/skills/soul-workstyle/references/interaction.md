# Interfaces and interaction

Follow these rules when you design or build an interface, including menus and HUDs in a game, and when you change how
something looks or sounds.

- Direct manipulation: express operations in the user's objects and words, and show state and results beside the
  thing that the user operates on. Do not show IDs, data tables or back-end flows, because the user thinks about the
  thing itself, not about its implementation
- Immediate feedback: show a change that can be previewed while it happens, and show an asynchronous operation as in
  progress, done or failed on the object itself. Take the key state from the authoritative data, not from front-end
  guesses
- No modes: avoid hidden, global and persistent modes that change what an operation means, because the user forgets
  which mode is on and then gets different results from the same action. If a mode cannot be avoided, make it local,
  visible, brief and easy to leave
- Undo rather than confirm: make operations undoable, cancellable and correctable, and keep the user's work when
  something fails. Ask for confirmation beforehand only when an action is irreversible, affects other people or
  systems, or is high-risk, and state the consequence, because users click through confirm dialogs by habit
- An automatic jump (past an intro or a sponsor segment) stops a moment before its target, so that the user sees what
  was skipped and trusts the jump
- Anything decided automatically that can be wrong gets a manual override. Put the override where the user already
  looks (the app's own settings), and show automatic values apart from manual ones
- Before you change an interface, read the styles of the components beside it and reuse its classes and visual
  language. Do not add cards, borders, shadows or accent colours that it does not already use, because the user
  rejects decoration that does not match
- The user judges a look by eye and a sound by ear, so let the user pick before a change becomes the default. For a
  visual change, show two or three static mockups side by side and build only the one that the user picks. For a
  change to sound, render one short sample as separate files: a baseline and one variant for each change. Each
  variant changes one parameter in a small step, and its file name says what it changes. A small fix to an existing
  element needs neither
- Keep on the screen any state that the user will look back at, instead of relying only on toasts, redirects or
  reloads
- When you look at an interface, check errors, empty data, loading and narrow screens as well as the normal flow
