# Web actions that need the user's login

Web actions that need the user's identity (an issue, a comment, an upload) go through the Chrome DevTools MCP
(`mcp__chrome-devtools__*`): `list_pages` shows the tabs of the user's own Chrome, already signed in, with remote
debugging on 127.0.0.1:9222, which the user starts (Chrome Beta,
`C:\Program Files\Google\Chrome Beta\Application\chrome.exe`, on the real profile; a temporary `--user-data-dir` has
no login and is useless here). The app's built-in browser has no login and no file upload, so do not ask the user to
sign in there. Work in a new tab, never in one that may hold the user's work.

Close only what you opened, and only if the user has not taken it over (its URL changed, tabs you did not open
appeared). Never kill browsers by image name (`taskkill /IM chrome.exe`): that closes the user's own tabs. If you must
start the browser, record the PIDs before and close only the new ones. Read page text with `evaluate_script`, which is
cheaper than the accessibility snapshot.

Publishing needs the user's go-ahead: fill the form, show it, submit only when the user says so.

Mechanics:

- GitHub's forms are React: set a field with the native value setter and an `input` event; a plain value change is
  wiped by the next render
- An upload button that makes its file input on click: wrap `HTMLInputElement.prototype.click` to capture the input
  and append it to the page, then `upload_file` to it
- `upload_file` from a path longer than Windows' 260 characters arrives as 0 bytes with no error: copy the file under
  `%TEMP%` first and check `input.files[0].size`
