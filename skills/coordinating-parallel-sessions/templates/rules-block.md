## Parallel sessions — shared rules

Several sessions edit this project at the same time and cannot see each other. They coordinate only through the log `{{LOG_FILE}}`, in the same folder as this file. {{PATHS_START_FROM}}

**Before each change**, not only when the chat starts: read the newest log entries (newest at the top). They show what other sessions changed and what is not uploaded yet.

**While changing:**
- Re-read a file right before editing it, even if you read it earlier: another session may have changed it.
- If the lines the user asked you to change are no longer as you last read them, another session changed them in the meantime. Don't change them yet: tell the user what is there now (the log may say which session did it and why) and ask which version to keep. If only other parts of the file changed, keep them and go on with your change.
- If your edit tool refuses because the file changed, your copy is old: re-read the file and start from what is there now. Never get round the refusal with a shell command or a script.
- Change existing files only with small targeted edits. Never rewrite or replace a whole file: not with a whole-file write, `cp`, `mv`, `cat >`, `sed -i` or a script, and not from a test copy, a backup, an old folder or memory. Files made by a build step are the exception.
- Do not undo or tidy changes you didn't make, even if they look unfinished: they are another session's work in progress. If something looks broken, tell the user.
- Try risky changes on a copy outside the project, never on real data.

**When a request is finished:**
1. If it changed files that get uploaded, raise the version in {{VERSION_LOCATION}} (every place, always the same number), even for a tiny change: re-read the file(s) and add one to the last number (1.4.2 → 1.4.3). If that number is already in the log, take the next one.
2. Add an entry at the top of the log, in the format written there, with `Uploaded: no` (or `Uploaded: local only` if none of its files get uploaded). While you are still adjusting the same request and your entry still says `Uploaded: no`, update it instead of adding one or raising the version again. Once it is marked yes or partly, the adjustment is a new change.

**Never upload:** {{NEVER_UPLOAD}}. If you add a file of this kind, add it here.

**When the user asks what to upload:** take every entry marked `Uploaded: no`, plus the files still missing on entries marked `Uploaded: partly`, from any session, oldest first. List each file once, with the full server folder next to it (files marked "deleted" must be deleted on the server), then the "After upload" steps in order. Say:
- which other sessions' changes go online with it;
- that each file goes into its matching folder on the server: {{SERVER_FOLDER}} plus the file's own subfolders; that the whole list goes up together; and to upload these files only, never the whole folder;
- that chats still open may have unfinished changes in these files;
- after every upload: {{AFTER_EVERY_UPLOAD}};
- how to check it is live (the new version number in the page source).

**When the user says it is uploaded:** mark only the entries of the list the user uploaded from (if this chat didn't give that list, show the files you think went up and ask before marking): `Uploaded: yes (YYYY-MM-DD)`, and remind them of any After upload step. If only some files went up, mark each entry with files left `Uploaded: partly (YYYY-MM-DD), still to upload: <files>` and say what won't work online until the rest goes up. If the version file already went up, raise the version again so browsers load the missing files, and add your own entry for it (Files: the version file(s); What changes: "goes up together with <missing files>"; `Uploaded: no`). If it is unclear what went up, show the files and ask. Your edits are only on this computer: never say a change is online before this.

Talk to the user in plain words, in their language.
