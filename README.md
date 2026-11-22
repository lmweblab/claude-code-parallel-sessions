# Parallel Claude sessions on one project, without overwriting each other's work

When two or more Claude Code sessions (or desktop chats) work in the same project folder, one quietly overwrites another's changes, and nobody knows what is already live on the server. This skill fixes that with two plain text files every session follows: a shared change log that also tracks what has been uploaded, and a short block of rules in `CLAUDE.md`. There's no git, no hooks and no scripts, so it works with FTP and shared hosting too.

It was made for site owners who aren't programmers and upload by FTP. Developers can use it next to git, mostly for knowing what's deployed.

## What a log entry looks like

```
## 2026-03-14 16:20 — Contact form: phone number field
- Session: contact form
- Version: 2.3.8
- Files: contact.php, js/form.js, inc/version.php
- What changes: the contact form asks for a phone number (optional)
- After upload: nothing
- Uploaded: no
```

Every session reads the newest entries before it touches anything, re-reads a file right before editing it, and adds its own entry when it's done. The log is written in your language, field names included, and entries are never deleted, so it doubles as the history of who changed what and why.

## What it sets up

- The change log (`CHANGES.md`, or `MODIFICHE.md` in Italian, and so on).
- The rules block in `CLAUDE.md`: read the log first, re-read before editing, change files only with small targeted edits (never copy a test copy or backup back over the current file), raise the version after every change, log it.
- A starting entry that lists the whole site as not uploaded, so one full upload puts the server and the log in step.
- A never-upload list: databases, backups, `.env` and other secrets, `node_modules/`, `.DS_Store`, the log itself.

Then two phrases work in any chat. *"What do I have to upload?"* gives one list built from every chat's pending changes, each file once, plus whatever has to happen on the server afterwards (open a setup page, clear the cache). *"I uploaded it"* marks those entries as uploaded; if only part of the list went up, the entries say which files are still missing.

## How to use it

Open a session on your project and say something like:

> I keep several chats open on this project at the same time. Set things up so they stop overwriting each other and I always know what's uploaded.

Claude asks how you upload, finds your version number and the files that must never go online, creates the two files and tells you what to do with the chats that are already open.

## Install

**Claude Code**, inside a session:

```
/plugin marketplace add lmweblab/claude-code-parallel-sessions
/plugin install coordinating-parallel-sessions@lmweblab
```

`lmweblab` after the `@` is the name of the marketplace this repository provides. From a terminal, the same thing in one line:

```bash
claude plugin install coordinating-parallel-sessions --marketplace lmweblab/claude-code-parallel-sessions
```

To update later: `claude plugin marketplace update lmweblab`, then `claude plugin update coordinating-parallel-sessions@lmweblab`, then restart Claude Code.

**Other agents** that read skills (via [skills.sh](https://skills.sh)):

```bash
npx skills add lmweblab/claude-code-parallel-sessions
```

**By hand:** copy [`skills/coordinating-parallel-sessions/`](skills/coordinating-parallel-sessions/SKILL.md) into `~/.claude/skills/`. Pick one method only; two copies of the same skill just confuse things.

**Claude apps:** download [coordinating-parallel-sessions.zip](https://github.com/lmweblab/claude-code-parallel-sessions/releases/latest/download/coordinating-parallel-sessions.zip) and upload it under Customize > Skills (code execution has to be on in Settings > Capabilities). In a plain chat Claude can't edit your project files, so there it can only write log entries for you to paste.

## What it runs and sends

Nothing. The plugin is one skill: plain instructions that Claude reads. It has no scripts, hooks or MCP servers, fetches nothing from the internet and sends no data anywhere. All it does is have Claude write two text files in your own project folder (the change log and the rules in `CLAUDE.md`).

## Limits

These are rules, not locks. Two chats editing the same file within the same few seconds can still collide; re-reading and small targeted edits make that rare, not impossible. It also needs a Claude that can edit your project folder, such as Claude Code or the Code tab of the desktop app.

## Where it comes from

It grew out of months of work on seven internal web apps for a small company, with three or four Claude chats open at once and everything uploaded by FTP. The shared log there now has more than 100 entries.

Before release, test sessions ran on sample sites with and without the skill. Without it, a session asked to fix the problem built hooks, scripts and folder-ownership rules, and deleted the history after each upload. Later rounds ran it end to end on a WordPress child theme, which is where the handling of half-finished uploads and secrets already on the server came from.

The full instructions Claude follows are in [SKILL.md](skills/coordinating-parallel-sessions/SKILL.md).

## License

MIT

---

## In italiano

Una skill per chi tiene aperte più chat di Claude sullo stesso progetto nello stesso momento. Senza, una chat cancella il lavoro di un'altra senza che nessuno se ne accorga, e non si sa più cosa è già sul server.

Claude crea due file di testo: un registro delle modifiche nella tua lingua (`MODIFICHE.md`) e le regole comuni in `CLAUDE.md`. Ogni chat legge il registro prima di toccare qualcosa e ci scrive cosa ha cambiato. Da lì funzionano due frasi in qualsiasi chat: *"Cosa devo caricare?"*, che ti dà un'unica lista di file con quello che c'è da fare dopo, e *"Ho caricato"*.

Per iniziare, apri una chat sul progetto e scrivi: *"Ho più chat aperte su questo progetto: sistemale in modo che non si cancellino il lavoro a vicenda e che io sappia sempre cosa ho caricato."*

Per installarla in Claude Code, scrivi in una sessione `/plugin marketplace add lmweblab/claude-code-parallel-sessions` e poi `/plugin install coordinating-parallel-sessions@lmweblab`.
