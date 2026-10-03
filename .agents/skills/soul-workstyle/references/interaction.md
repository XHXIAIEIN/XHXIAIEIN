# Interfaces and interaction

Use these rules when you design or build an interface. They apply also to menus and HUDs in a game.

- Direct manipulation: express operations in the objects and words of the user. Show state and results beside the
  thing that the user operates on. Do not show IDs, data tables or back-end flows, because the user thinks about the
  thing itself, not about its implementation
- Immediate feedback: if a change can be previewed, show it continuously. Show an asynchronous operation on the object
  itself: in progress, success or failure. Take the key state from the authoritative data, not from front-end guesses
- No modes: avoid hidden, global and persistent modes that change what an operation means. The user forgets which
  mode is on, and the same action then gives different results. If a mode cannot be avoided, make it local, visible,
  brief and easy to leave
- Undo is better than confirm: make operations undoable, cancellable and correctable. Keep the work of the user when
  something fails. Ask for confirmation first only when an action is irreversible, affects the outside or is
  high-risk, and state the consequence. Users click through confirm dialogs by habit
- Make an automatic jump (to skip an intro or a sponsor segment) stop a moment before its target. The user then sees
  what was skipped and trusts the jump
- Give each automatic decision that can be wrong a manual override. Put the override where the user already looks
  (the settings of the app), and show automatic values apart from manual values
- Before you change an interface, read the styles of the components beside it. Reuse its classes and its visual
  language. Add no cards, borders, shadows or accent colours that it does not already use. The user rejects
  decoration that does not match
- Keep on screen the state that the user will look back at. Toasts, redirects and reloads alone are not enough as
  feedback
- When you examine an interface, look at the normal flow and also at errors, empty data, loading and narrow screens
