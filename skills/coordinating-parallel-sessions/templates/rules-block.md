## Parallel sessions — shared rules

Several sessions work on this project at the same time. They cannot see each other: they talk only through files. These rules apply to every session.

**Before changing anything:** read the newest entries of `CHANGES.md` (newest at the top). They tell you what other sessions changed and what is not uploaded yet.

**While changing:**
- Re-read a file right before editing it, even if you read it earlier in this session: another session may have changed it since.
- Apply your change to the current file with targeted edits. Never put a file over the current one from a test copy, a backup, an old folder or your memory: re-apply just your change instead.
- Never overwrite existing files with shell commands (`cp`, `mv`, `cat >`, `sed -i`). Use the editing tools, which refuse to write over a file that changed after you read it.
- Leave alone changes you didn't make in this session, even if they look unfinished: they are another session's work in progress. If something looks broken, tell the user.
- Try risky changes on a copy outside the project, never on real data.

**After each change:**
1. Raise the version: {{VERSION_LOCATION}}. Re-read that file first, since another session may have raised it already. Every change, however small, so each change has its own version in the log. If an earlier entry forgot its bump, yours covers it: say so in your entry. If the number you meant to use is already in the log, take the next one.
2. Add an entry at the top of `CHANGES.md`, in the format written there, with `Uploaded: no`. Don't edit other sessions' entries except their `Uploaded` line.

**When the user asks what to upload:** list the files of every entry still marked `Uploaded: no` (from any session), oldest first, each file once, then the "After upload" steps in order.
- Say which other sessions' changes go online with it.
- Say that each file goes into the same folder on the server, and that the whole list goes up together.
- Remind them that work still in progress in other chats isn't in the log yet.
- Never include: {{NEVER_UPLOAD}}. If you add a file of that kind, add it to this list.
- Change entries to `Uploaded: yes (date)` only after the user says the upload is done, and only the entries in the list they uploaded. If that's unclear, show the files and ask.

**Talk to the user** in plain words, in their language.
