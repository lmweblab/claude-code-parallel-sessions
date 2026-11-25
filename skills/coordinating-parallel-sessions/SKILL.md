---
name: coordinating-parallel-sessions
description: Use when several Claude chats or sessions edit the same project at once and overwrite each other's work, or nobody knows what is already uploaded to the server. Sets up a shared change log.
license: MIT
---

# Coordinating parallel sessions

## Overview

Sessions on the same folder cannot see each other. They can only talk through files. Give them **one shared change log** and **a short rules block in the project instructions**, and they stop overwriting each other's work and always know what is live.

The whole system is two text files. It works for any agent that edits the project files and reads project instructions (`CLAUDE.md` in Claude Code), and an owner who is not a programmer can read it and fix it.

## When to use

- The user keeps several chats open on one site, app or document folder.
- Work disappeared because a session wrote over another session's change.
- The user can't tell what is already on the server ("what do I still have to upload?").
- Not needed when only one session works at a time. If every session already has its own git branch merged through pull requests, keep that; the log still helps with deploy state. Sessions in separate git worktrees each see their own copy of the log: point the rules at the one in the main folder by its full path.
- A chat that can't edit the folder (a plain web chat) can't keep the log: have it end each change with a ready-to-paste entry.

If the rules block is already in the project instructions and the log exists, setup is done: follow the rules.

## Setup (once per project)

0. **Ask the user not to give the other open chats new work until setup is finished**: setup edits the version files they may be editing.
1. **Look for an existing log** (`CHANGES.md`, `MODIFICHE.md`, a notes file). If there is one, adopt it and add the missing fields. Never start a second one. A public `CHANGELOG.md` written for the users of a library or app is not a session log: leave it alone and create a separate one.
2. **Find the version number**, if the project has one: a constant in a config file, `package.json`, a theme or plugin header (WordPress `style.css`), the `?v=`/`?ver=` on CSS/JS links. Follow the CSS/JS links back to the number they print: that one makes browsers reload. If the same number is written in several places, note every place (file and name) and put in the rules that they always carry the same number. Default: raise the last number (1.4.2 → 1.4.3). If there is none and browsers cache CSS/JS, propose adding one; if the owner says no, drop the version step from the rules and entries say `Version: none`.
3. **List, with exact paths or name patterns, what must never be uploaded**: databases, backups, files users uploaded, secrets (`.env`, key files), the `.git/` folder (its history can still hold old secrets), test copies, config files that differ on the server (live database password, `.htaccess`), setup or migration pages that already ran, things that only run on this computer (`node_modules/`, `package.json`, `package-lock.json`, dev-server and build configs, source files that get compiled), system junk (`.DS_Store` in any folder, `Thumbs.db`), the log and the instructions file. Don't exclude anything the live site loads (a WordPress theme needs `screenshot.png`; some PHP sites need `vendor/`). If a build step makes the files that go online, entries list the built files and the upload answer says to build first. If the owner has always uploaded the database, ask before excluding it: the local copy may be the one they edit.
4. **Ask how the owner uploads** (which program, the whole folder or single files, into which folder on the server), unless they already said. **Then decide where the two files live:**
   - Single files, git or a deploy tool: the project folder.
   - Dragging the whole folder: one level above it, so the two files can't reach the server (Claude Code also reads `CLAUDE.md` from parent folders). First check that upper folder: a `CLAUDE.md` there applies to every chat opened anywhere below it, other sites included. If it holds anything else (other sites, other themes or plugins, a parent theme included), offer to make a folder just for this one and move the project into it, unless something depends on where it is (a WordPress theme must stay in `themes/`). If it can't move, keep the two files in the project and add them to the never-upload list.
   - When the files sit above the project, new chats must be opened on that upper folder: Claude Code asks before editing a file outside the folder a chat was opened on, and a refused prompt means a missing entry. Write in the rules which folder the paths in entries start from.
5. **Create the log** from [templates/CHANGES.md](templates/CHANGES.md). Translate everything into the user's language, file name, field names and status words included, and use the same words in the rules block, because sessions match on them (`Uploaded: no`). Pick status words where none is the start of another: `no` and `not needed` would both match a search for `Uploaded: no`.
6. **Add the rules block** from [templates/rules-block.md](templates/rules-block.md) to the project instructions, translated the same way. Fill in the log file name, the folder the paths in entries start from (delete that placeholder if it is the folder of the log), the version places (file and name, not the current number), the never-upload list and the server folder. Ask whether the site has a page cache or CDN (a cache plugin, the host's cache, Cloudflare): if it does, fill in the "after every upload" bullet with where to clear it; if not, delete that bullet.
7. **Write the starting entry** from the template: everything that would be uploaded, marked as not uploaded, because nobody knows what the server has. Raise the version in it, so browsers reload everything after this first full upload. Before that upload, ask whether anyone ever changed files directly on the server (the hosting file manager, the WordPress editor, plugin updates): the full upload would overwrite those changes, so copy them into the project first.
8. **Tell the user**, in plain words:
   - Chats that are already open don't know the rules and can still overwrite work. Give a phrase to paste into each one before closing it: "Read `<path to CLAUDE.md>`, then add one entry to `<path to the log>` for everything you changed in this chat, with `Version: none (covered by the starting entry)`. Don't change any other file. If you don't know whether it's uploaded, write `Uploaded: no`." Translate its field names and status words exactly as in the log. Then open new chats on the folder from step 4.
   - The first "What do I have to upload?" will list the whole site, once.
   - If they used to drag the whole folder, the never-upload items are probably on the server already, where anyone may be able to download them. From now on they upload only the files on the list, and they fix the server in this order. Name secrets by their key (`DB_PASS`); never write their values in the chat, the log or the rules.
     1. A secrets file the live site reads (search the code for it; `.env` usually is one): leave it on the server, because deleting it breaks the site. They open its full address in a browser, with the server folder in it (`https://<site>/<folder>/.env`). If it shows or downloads, they block it before anything else (give the exact lines to add to the `.htaccess` that is on the server, not a file to upload over it, or the sentence to send to the host), then open it again to check.
     2. A secrets file nothing reads, `.git/` and the other items: they delete them on the server.
     3. Last, they change every password or key that could be downloaded, those in the history of an uploaded `.git/` included: at the provider and in the server's copy of the file, one right after the other.
   - Two phrases work in any chat: "What do I have to upload?" and "I uploaded it".
9. **Check**: search the log for the exact not-uploaded text used in the rules. It must find the starting entry and nothing in the header, and the rules must name the log file exactly as it is called.

That is the whole setup: two files. Hooks, scripts, hash ledgers, locks and "which chat owns which folder" zones only work in one tool, fail silently and can't be repaired by the owner. Plain rules and a readable log are what hold up over months.

## Every change

The rules block makes each session do this: read the newest log entries → search the older ones for what it is about to change, and ask first if the user wanted the opposite → re-read the file right before editing → stop and ask if the lines it has to change are no longer as it last read them → apply only its own change, with a targeted edit → raise the version → add an entry at the top with `Uploaded: no` → say how many changes are waiting to be uploaded, from every session. The list of files comes only when the user asks for it, because it has to cover the whole log.

## "What do I have to upload?"

Search the whole log and take every entry with `Uploaded: no`, plus the files still missing on entries marked `Uploaded: partly`, oldest first. The upload list is the union of their files (a file changed by two sessions goes up once and carries both changes), minus the never-upload list. Then:
- Say plainly which other sessions' changes go online with this upload.
- Say that each file goes into its matching folder on the server (write the full server folder next to each file), that the whole list goes up together, and that they should upload these files only, never the whole folder.
- Add, in order, whatever must happen after the upload: a migration page to open (and whether to delete it afterwards), a change to make in the live database, A before B, clearing the cache.
- Remind the user that work still in progress in other chats isn't in the log yet, and say how to check the new version is live (the version number in the page source).

Build the list from the log, never from file dates or memory. Write `Uploaded: yes (date)` only after the user says it is done, and only on entries whose files all went up. If only some went up, write `Uploaded: partly (date), still to upload: <files>` on each entry with files left, and tell the user what doesn't work online until the rest goes up (a stylesheet that hides a button a script is meant to show). If the version file went up without the rest, raise the version again: browsers and caches may have stored the old files under the new number. Record that raise in a new entry with `Uploaded: no`. If it's unclear what went up, show the files and ask. Never call a change "online" before all its files are up.

## Several apps sharing the same files

If identical files live in several apps (a login page, a shared stylesheet), make the same targeted edit in every copy in the same session, and list every copy in the entry (one entry per log if each app has its own). Never change one copy alone, and never copy one app's file over another's: the other copy may hold changes of its own.

## Common mistakes

| Mistake | Fix |
|---|---|
| Copying a tested file back from a test copy or backup, or rewriting a whole file | Re-read the current file and re-apply just your change with a targeted edit: another session may have edited it after you made the copy |
| Changing lines that another session changed after you last read them, because the user is in a hurry or asked for it before that change | Tell the user what is there now and ask which version to keep |
| Skipping the version bump because the change is tiny | Bump every time an uploaded file changes, and list the version file in the entry: it has to be uploaded too |
| Deleting log entries once they are uploaded | Keep them: the log is also the history of who changed what, and why |
| Building tooling (hooks, scripts, ownership zones) instead of the two files | Rules the owner can read are the rules that survive |
| Undoing or "tidying" changes this session didn't make, or editing another session's entry | Leave them. Record fixes (such as a bump someone forgot) in your own entry. Tell the user if something looks broken |
| Telling the user to upload only this session's files | If they asked what to upload, list every entry still marked `Uploaded: no` or `partly`. If they didn't, say how many changes are waiting, without file names |
| Reading the whole log before every change, or only its newest entries | Read the newest, then search the rest for what you are about to change |
| Marking a whole entry uploaded when only some of its files went up | `Uploaded: partly (date), still to upload: <files>` |
| Starting with an empty log, so later chats answer "nothing to upload" | The starting entry lists the whole site as not uploaded |
