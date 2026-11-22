---
name: coordinating-parallel-sessions
description: Use when two or more Claude sessions or chats work on the same project folder at the same time, when one session overwrote or undid another session's work, when nobody knows which changes are already uploaded or deployed to the server, or when a project keeps a shared change log that sessions must follow.
---

# Coordinating parallel sessions

## Overview

Sessions on the same folder cannot see each other. They can only talk through files. Give them **one shared change log** and **a short rules block in the project instructions**, and they stop overwriting each other's work and always know what is live.

The whole system is two text files. It works for any agent that edits the project files and reads project instructions (`CLAUDE.md` in Claude Code), and an owner who is not a programmer can read it and fix it.

## When to use

- The user keeps several chats open on one site, app or document folder.
- Work disappeared because a session wrote over another session's change.
- The user can't tell what is already on the server ("what do I still have to upload?").
- Not needed when only one session works at a time. If every session already has its own git branch merged through pull requests, keep that; the log still helps with deploy state.
- A chat that can't edit the folder (a plain web chat) can't keep the log: have it end each change with a ready-to-paste entry.

## Setup (once per project)

1. **Look for an existing log** (`CHANGES.md`, `MODIFICHE.md`, `CHANGELOG`, a notes file). If there is one, adopt it and add the missing fields. Never start a second one.
2. **Find the version number**, if the project has one: a constant in a config file, `package.json`, the `?v=` on CSS/JS links. Write down the exact file and name. Default: raise the last number (1.4.2 → 1.4.3). If there is none and browsers cache CSS/JS, propose adding one.
3. **List, with exact paths, what must never be uploaded**: databases, backups, files users uploaded, secrets, test copies, the log and the instructions file. If the owner has always uploaded the database, ask before excluding it: the local copy may be the one they edit.
4. **Decide where the two files live.** If the owner uploads by dragging the whole folder, put them one level above the uploaded folder (Claude Code also reads `CLAUDE.md` from parent folders), so they can't end up on the server. Then write in the rules where the log is (`../CHANGES.md`) and which folder the paths in entries start from. Otherwise, the project folder is fine.
5. **Create the log** from [templates/CHANGES.md](templates/CHANGES.md). Translate everything into the user's language, file name and field names included, and use the same words in the rules block, because sessions match on them (`Uploaded: no`).
6. **Add the rules block** from [templates/rules-block.md](templates/rules-block.md) to the project instructions, translated the same way, with the version location (file and name, not the current number) and the never-upload list filled in.
7. **Write the starting entry** from the template. It lists every file that would be uploaded, marked `Uploaded: no`, because nobody knows what the server has. Raise the version in it, so browsers reload everything after this first full upload.
8. **Tell the user**, in plain words:
   - Chats that are already open don't know the rules and can still overwrite work. Give a phrase to paste into each one before closing it: "Read `<path to CLAUDE.md>` and add an entry to `<path to the log>` for everything you changed in this chat; if you don't know whether it's uploaded, write `Uploaded: no`." Then open new chats on the project folder.
   - The first "What do I have to upload?" will list the whole site, once.
   - If they drag the whole folder to the server: from now on, upload only the files on the list, or the never-upload items (the database first of all) go up too.
   - Two phrases work in any chat: "What do I have to upload?" and "I uploaded it".

That is the whole setup: two files. Hooks, scripts, hash ledgers, locks and "which chat owns which folder" zones only work in one tool, fail silently and can't be repaired by the owner. Plain rules and a readable log are what hold up over months.

## Every change

The rules block makes each session do this: read the newest log entries → re-read the file right before editing → apply only its own change to the current file → raise the version → add an entry at the top with `Uploaded: no`.

## "What do I have to upload?"

Take every entry with `Uploaded: no`, oldest first. The upload list is the union of their files (a file changed by two sessions goes up once and carries both changes), minus the never-upload list. Then:
- Say plainly which other sessions' changes go online with this upload.
- Say that each file goes into the same folder on the server, and that the whole list goes up together.
- Add, in order, whatever must happen after the upload: a migration page to open (and whether to delete it afterwards), a change to make in the live database, A before B.
- Remind the user that work still in progress in other chats isn't in the log yet.

Build the list from the log, never from file dates or memory. Write `Uploaded: yes (date)` only after the user says it is done, and only on the entries in the list they uploaded. If that's unclear, show the files and ask. Never call a change "online" before that.

## Several apps sharing the same files

If identical files live in several apps (a login page, a shared stylesheet), keep one master copy in a folder that is never uploaded and add a rule: change the master first, then copy it into every app in the same session. Never change one copy alone.

## Common mistakes

| Mistake | Fix |
|---|---|
| Copying a tested file back from a test copy or backup over the current one | Re-read the current file and re-apply just your change: another session may have edited it after you made the copy |
| Skipping the version bump because the change is tiny | Bump every time, and list the version file in the entry: it has to be uploaded too |
| Deleting log entries once they are uploaded | Keep them: the log is also the history of who changed what, and why |
| Building tooling (hooks, scripts, ownership zones) instead of the two files | Rules the owner can read are the rules that survive |
| Undoing or "tidying" changes this session didn't make, or editing another session's entry | Leave them. Record fixes (such as a bump someone forgot) in your own entry. Tell the user if something looks broken |
| Listing only this session's files when asked what to upload | List every entry still marked `Uploaded: no` |
| Starting with an empty log, so later chats answer "nothing to upload" | The starting entry lists the whole site as `Uploaded: no` |
