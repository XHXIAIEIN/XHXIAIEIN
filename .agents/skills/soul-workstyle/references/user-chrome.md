# Web actions that need the user's login

Web actions that need the user's identity (an issue, a comment, an upload) go through the Chrome DevTools MCP
(`mcp__chrome-devtools__*`), whose `list_pages` shows the tabs in the user's own Chrome, already signed in. The
user starts that Chrome: Chrome Beta (`C:\Program Files\Google\Chrome Beta\Application\chrome.exe`) on the real
profile, with remote debugging on 127.0.0.1:9222. A temporary `--user-data-dir` has no login and so does not help
here. The app's built-in browser has neither a login nor file upload, so do not ask the user to sign in there. Work in
a new tab, never in one that may hold the user's work.

Close only the tabs and browsers that you opened, and only if the user has not taken them over. A changed URL, or
tabs that you did not open, show that the user has taken them over. Never kill browsers by image name
(`taskkill /IM chrome.exe`), because that closes the user's own tabs. If you must start Chrome yourself, record the
PIDs before you start it, so that you close only the new ones. Read page text with `evaluate_script`, which costs
less than the accessibility snapshot.

Publishing needs the user's approval: fill in the form, show it to the user, and submit only when the user says so.

Mechanics:

- GitHub's forms are React components, so set a field with the native value setter and an `input` event. React
  removes a plain value change on the next render
- If an upload button creates its file input only on click, wrap `HTMLInputElement.prototype.click` to capture that
  input, append it to the page and then call `upload_file` on it
- When the path is longer than the Windows limit of 260 characters, `upload_file` sends 0 bytes and reports no error.
  Copy the file under `%TEMP%` first and check `input.files[0].size`
