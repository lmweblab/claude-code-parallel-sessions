# Change log — shared by every session

Each session adds an entry **at the top** (just under the line below) when it finishes a change.
Before touching anything, read at least the newest entries. Rules: the project instructions (`CLAUDE.md`).

Entry format:

```
## YYYY-MM-DD HH:MM — short title
- Session: the chat's title, or its topic if it has none (products, design, blog…)
- Version: the new version (or "none")
- Files: paths from the project folder ("new" for new files, "deleted" for removed ones), including the file that holds the version
- What changes: 1-2 lines, in plain words
- After upload: nothing / what must be done on the server, in order (e.g. "open setup.php once as admin, then delete it from the server", "add the new category in the live database")
- Uploaded: no
```

When the user confirms the upload, change `Uploaded: no` to `Uploaded: yes (date)`, only on the entries in the list they uploaded.
An entry that touches only files that never get uploaded says `Uploaded: not needed`.
Never delete entries, and don't edit other sessions' entries except their `Uploaded` line.

---

## YYYY-MM-DD HH:MM — Starting point: one full upload
- Session: setup
- Version: the new version (raised so browsers reload everything)
- Files: every file that gets uploaded (list them all), i.e. the whole site except the never-upload list
- What changes: nothing new. Before today nobody recorded changes, so it is unknown what the server has. Uploading everything once puts the server and this log in step.
- After upload: nothing
- Uploaded: no
