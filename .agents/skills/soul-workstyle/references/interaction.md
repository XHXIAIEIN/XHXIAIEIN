# Interfaces and interaction

Follow these when designing or building an interface, menus and HUDs in a game included.

- Direct manipulation of what the user sees: express operations in the user's objects and words, show state and
  results beside the thing operated on, never IDs, data tables or back-end flows, because the user thinks about the
  thing itself, not its implementation
- Immediate feedback: previewable changes show continuously; an asynchronous operation shows in progress, success or
  failure on the object itself; key state comes from the authoritative data, not front-end guesses
- No modes: avoid hidden, global, persistent modes that change what an operation means, because the user forgets
  which mode they are in and the same action gives different results. If one is unavoidable, make it local, visible,
  brief and easy to leave
- Undo beats confirm: make operations undoable, cancellable and correctable, and keep the user's work when something
  fails; confirm beforehand only when an action is irreversible, affects the outside or is high-risk, and state the
  consequence, because confirm dialogs get clicked through by habit
- An automatic jump (skipping an intro, a sponsor segment) lands a moment before its target, so the user sees what
  was skipped and trusts it
- Anything decided automatically that can be wrong comes with a manual override, placed where the user already
  looks (the app's own settings), with automatic values shown apart from manual ones
- Before changing an interface, read the styles of the components beside it and reuse its classes and visual
  language; add no cards, borders, shadows or accent colours it does not already use. Decoration that does not match
  gets sent back
- Feedback does not rely only on toasts, redirects or reloads: state the user will look back at stays on screen
- When looking at an interface, besides the normal flow, look at errors, empty data, loading and narrow screens
