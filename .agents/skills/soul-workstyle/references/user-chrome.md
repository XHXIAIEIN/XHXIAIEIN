# Web actions that need the user's login

For web actions that need the identity of the user (an issue, a comment, an upload), use the Chrome DevTools MCP
(`mcp__chrome-devtools__*`). `list_pages` shows the tabs of the user's own Chrome, which is already signed in. The
user starts Chrome Beta (`C:\Program Files\Google\Chrome Beta\Application\chrome.exe`) on the real profile, with
remote debugging on 127.0.0.1:9222. A temporary `--user-data-dir` has no login, so it does not work here. The
built-in browser of the app has no login and no file upload, so do not ask the user to sign in there. Work in a new
tab. Never work in a tab that may hold the work of the user.

Close only the tabs that you opened. If the user took over a tab, leave it open. Signs of this: its URL changed, or
tabs that you did not open appeared. Never kill browsers by image name (`taskkill /IM chrome.exe`), because that
closes the user's own tabs. If you must start Chrome, record the PIDs before the start and close only the new PIDs.
Read page text with `evaluate_script`, because it costs less than the accessibility snapshot.

Before you publish, get the approval of the user: fill the form, show it to the user, and submit only when the user
says so.

Mechanics:

- GitHub forms use React. Set a field with the native value setter and an `input` event. The next render of React
  removes a plain value change
- If an upload button creates its file input on click, wrap `HTMLInputElement.prototype.click` to capture the input.
  Append the input to the page, then call `upload_file` on it
- If the path is longer than the Windows limit of 260 characters, `upload_file` sends a file of 0 bytes and shows no
  error. Copy the file under `%TEMP%` first, then check `input.files[0].size`
