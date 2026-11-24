# Change log — shared by every session

Each session adds an entry **at the top** (just under the line below) when it finishes a change.
Before each change, read the newest entries and search the older ones for what you are about to change. Rules: the project instructions (`CLAUDE.md`), in this same folder.

Entry format:

```
## YYYY-MM-DD HH:MM — short title
- Session: the chat's topic (products, design, blog…): a chat can't see its own title
- Version: the new version (or "none")
- Files: paths from the project folder, separated by commas, with (new) or (deleted) after a path when it applies, including the file(s) that hold the version; no other notes on this line
- What changes: 1-2 lines, in plain words. If it breaks when only some of its files are online, say so (e.g. "style.css hides the button until main.js shows it")
- After upload: nothing / what must be done on the server, in order (e.g. "open setup.php once as admin, then delete it from the server", "add the new category in the live database")
- Uploaded: <status>
```

Take the date and time from the computer clock (`date`), not from memory.
Status: `no` for a new entry; `yes (YYYY-MM-DD)` once the user says all its files are uploaded; `partly (YYYY-MM-DD), still to upload: <files>` if only some went up; `local only` if none of its files ever get uploaded.
Never delete entries, and don't edit other sessions' entries except their `Uploaded` line.

---

## YYYY-MM-DD HH:MM — Starting point: one full upload
- Session: setup
- Version: the new version (raised so browsers reload everything)
- Files: everything that gets uploaded, i.e. the whole site except the never-upload list (list single files only if there are few; otherwise name the folders and what is left out)
- What changes: nothing new. Before today nobody recorded changes, so it is unknown what the server has. Uploading everything once makes the server match this folder.
- After upload: nothing
- Uploaded: no
