# Coordinating parallel sessions

A Claude skill for anyone who keeps several Claude chats open on the same project at once.

**Without it**, one chat quietly overwrites another chat's work, and nobody knows what is already on the server.
**With it**, Claude sets up two plain text files that every session follows: a shared change log and a short block of rules in `CLAUDE.md`.

No git, hooks or scripts needed. It was made for owners who are not programmers and upload their site by FTP, and it works just as well for developers.

## What it sets up

- **A change log** (`CHANGES.md`, written in your language). One entry per change: which chat made it, the new version, the files, what changes, what to do after uploading, and `Uploaded: no / yes (date)`. Entries are never deleted, so it is also the history of who changed what, and why.
- **Rules for every session**, in `CLAUDE.md`:
  - read the log before touching anything;
  - re-read a file right before editing it;
  - never copy a test copy or backup back over the current file;
  - raise the version after every change and log it.
- **A starting point**: the first entry lists the whole site as not uploaded, so the server and the log start out equal after one full upload.
- **Two phrases** that work in any chat:
  - *"What do I have to upload?"* gives one list built from every chat's pending changes, each file once, plus the steps to do after the upload.
  - *"I uploaded it"* marks those entries as uploaded.

## How to use it

Open a session on your project and say something like:

> I keep several chats open on this project at the same time. Set things up so they stop overwriting each other and I always know what's uploaded.

Claude finds your version number and the files that must never be uploaded (databases, backups, secrets), creates the two files, and tells you what to do with the chats that are already open.

## Install

**Claude Code:** add this repository as a plugin marketplace, then install the plugin:

```
/plugin marketplace add lmweblab/coordinating-parallel-sessions
/plugin install coordinating-parallel-sessions@parallel-sessions
```

Or copy `skills/coordinating-parallel-sessions/` into `~/.claude/skills/`.

**Claude apps:** download `coordinating-parallel-sessions.zip` from the Releases page and upload it as a custom skill in Settings.

The log only works where Claude can edit your project files, as in Claude Code. A plain web chat can't keep it.

## Where it comes from

It comes from months of building and maintaining seven internal web apps for a small company, with three or four Claude chats open at the same time. The shared log there passed 120 entries without one chat losing another chat's work.

The skill was written test-first. Sample sessions ran on a test site without the skill, then with it. Without the skill, a session asked to "fix this" built hooks, scripts and folder-ownership rules. It also deleted history after each upload and had no place for after-upload steps. With the skill, sessions set up the two files, re-applied changes instead of copying old files back, and built the upload list from every chat's pending entries.

## License

MIT

---

## In italiano

Una skill per chi tiene aperte più chat di Claude sullo stesso progetto nello stesso momento.

Claude crea due file di testo:
- un **registro delle modifiche**, nella tua lingua;
- le **regole comuni** in `CLAUDE.md`.

Così le chat non si cancellano più il lavoro a vicenda e sai sempre cosa è già sul server. Funzionano due frasi in qualsiasi chat: *"Cosa devo caricare?"* e *"Ho caricato"*.

Per iniziare, apri una chat sul progetto e scrivi: *"Ho più chat aperte su questo progetto: sistemale in modo che non si cancellino il lavoro a vicenda e che io sappia sempre cosa ho caricato."*
