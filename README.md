# gtd.el

A lightweight Getting Things Done workflow for Org mode: a fixed set
of files under one directory, an inbox capture template, refile
targets, TODO keywords and faces, and an agenda layout tailored to
that file set.

## Files

`gtd/init` expects these files under its directory (missing ones are
simply empty refile/agenda targets until created):

- `inbox.org` — capture target for new, untriaged items
- `projects.org` — multi-step outcomes, refiled up to 3 levels deep
- `actions.org` — single next actions
- `someday.org` — someday/maybe items
- `resources.org` — reference material, refiled one level deep
- `tickler.org` — entries to resurface on a date; refiling here is
  rejected unless the entry has a `SCHEDULED` date

## Usage

```elisp
(require 'gtd)
(gtd/init "~/org/gtd")
```

This wires up:

- an `"i"` capture template that files into `inbox.org`
- `org-refile-targets` and `org-agenda-files` for the files above
- the `TODO` / `URGENT` / `WAITING` / `ACTIVE` / `DONE` / `CANCELLED`
  keyword sequence, with faces
- an agenda prefix format showing the enclosing project and
  scheduled date in `todo`-type views
- a global skip function that hides `WAITING` items from the agenda

Interactive commands, meant to be bound to whatever keys you like —
this package does not bind any global keys itself:

- `gtd/inbox`, `gtd/projects`, `gtd/actions`, `gtd/resources` — open
  the corresponding file (`gtd/inbox` also runs `gtd/sync` first)
- `gtd/prune-completed` — delete `DONE`/`CANCELLED` subtrees from the
  current buffer
- `gtd/prune-completed-all` — run that over every `.org` file in the
  GTD directory

In the Org agenda, under `evil` motion state (only when `evil` is
loaded; not required), `w` toggles hiding `WAITING` items.

## The `gtd` CLI

`gtd/sync` shells out to an external `gtd` executable
(`~/.local/bin/gtd` by default; see `gtd--gtd-executable`) that reads
and rewrites the GTD files on disk, e.g. to pull in items from other
sources. This is optional — everything else in this package works
without it.

## Installing

This package isn't on a package archive. With `straight.el` and
Doom Emacs, symlink this directory into straight's `repos/` and
declare it as a local, unmanaged recipe:

```elisp
;; packages.el
(package! gtd
  :recipe (:type nil :local-repo "emacs-gtd" :files ("*.el")))
```

```sh
ln -s /path/to/emacs-gtd ~/.config/emacs/.local/straight/repos/emacs-gtd
doom sync
```

Without straight, just put `gtd.el` on your `load-path`.
