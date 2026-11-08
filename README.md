# nokko workspace format

A nokko workspace is a folder of plain Markdown files. This repository documents the
format nokko reads and writes, so the files stay useful in any editor, in Obsidian, and
in whatever comes after both, and it ships a small sample workspace you can open in
nokko, in Obsidian or in a text editor.

It contains **no application code**. nokko itself is a native Mac and iPhone app by
Anemo Agency, available on the App Store; this repository is the file format, not the app.

- Website: https://nokko.dev/
- The format, in full: https://nokko.dev/guide/tasks-dataview-projects and
  https://nokko.dev/guide/time-entries-in-markdown
- What Markdown project management is: https://nokko.dev/markdown-project-management

## The shape of a workspace

```
Workspace/
├── Inbox.md                   # quick captures, a board like any other
├── Client Work/               # a space: a top-level folder
│   ├── _space.md              # the space's colour and status pipeline
│   ├── Bakehouse Website.md   # a board: one project, one file
│   └── How nokko works.md     # a doc: a file without tasks
└── Clients/
    └── Bakehouse.md           # a client record: rates and billing details
```

A **space** is a top-level folder. A **board** (project) is one Markdown file inside it. A
file with no tasks is a **doc**. Clients live in `Clients/`. nokko's own view settings sit
in a hidden `.nokko/` folder at the top of the workspace and are not part of the format.

## The task line

Tasks follow the [Obsidian Tasks](https://publish.obsidian.md/tasks/) convention, so a
nokko workspace is also an Obsidian vault and the other way round.

```markdown
- [ ] Set up hosting [Stage:: Build] [Budget:: 150] [est:: 2h] 📅 2026-11-09
- [/] Homepage design [est:: 6h] ⏫ 🛫 2026-11-03 📅 2026-11-15 #frontend
  - Two directions so far: warm/rustic and clean/modern
  - ⏱ 2026-11-06 09:15-12:30 (3h15m) — first pass at the hero
  - [ ] Pick a typeface pair
- [>] Logo files from the client
- [x] Kickoff call with the baker ✅ 2026-11-02
- [-] Print flyers
- [ ] Design sign-off 🎯 📅 2026-11-15
```

| Mark | Meaning |
| --- | --- |
| `[ ]` `[/]` `[>]` `[x]` `[-]` | to do, in progress, waiting, done, cancelled |
| `🛫 date` `⏳ date` `📅 date` `✅ date` | start, scheduled, due, completed (ISO dates) |
| `⏫` `🔼` `🔽` | high, medium, low priority |
| `🔁 every month` | repeat rule |
| `🎯` | milestone |
| `🆔 id` `⛔ id` | an identifier, and a dependency on another task |
| `[key:: value]` | a Dataview inline field: `est`, `Budget`, `Stage`, `Owner`, or any column you declare |
| `#tag` | a tag |
| indented `- ⏱ …` line | a logged time entry (below) |
| indented `- [ ]` line | a subtask |
| indented prose | a note on the task |

nokko rewrites only the characters it changes. Everything else on the line and around it,
including syntax it does not recognise, survives byte for byte.

## The time entry

```markdown
  - ⏱ 2026-11-06 09:15-12:30 (3h15m) — first pass at the hero
```

Date, start–end span, the duration in parentheses, and an optional note after an em dash.
Entries are indented under the task they belong to. Totals are recomputed from these lines
on every read; nothing is cached. The full parsing rules, worked examples and Dataview
queries are at https://nokko.dev/guide/time-entries-in-markdown.

## Frontmatter

A board:

```yaml
---
type: project
status: active
client: Bakehouse
start: 2026-11-01
due: 2026-11-29
columns:
  - "Budget: number"
  - "Stage: select(Discovery, Design, Build, Launch)"
---
```

A space (`_space.md`):

```yaml
---
type: space
color: "#4E9A6A"
statuses:
  - "Backlog: todo"
  - "Scoping: active"
  - "In Design: active #7A6FE0"
  - "In Development: active"
  - "Done: done"
---
```

A client (`Clients/Name.md`): `type: client`, then a `## Billing details` section in
prose. A doc: `type: doc`.

## The sample workspace

`sample-workspace/` is the workspace nokko writes on first run, with fixed dates. Open the
folder in nokko (File ▸ Open Workspace), or open it as a vault in Obsidian with the Tasks
and Dataview plugins, or just read it.

## Trademarks

Obsidian is a trademark of Dynalist Inc. nokko is an independent app by Anemo Agency, not
affiliated with or endorsed by Obsidian; it reads and writes plain Markdown files that
Obsidian also opens. Obsidian Tasks and Dataview are community plugins by their own authors;
this document describes their published conventions and adds nothing to them.

## Licence

The sample files and this document are released under [CC0 1.0](LICENSE): use them, copy
them, build on them. nokko the application is proprietary software and is not part of this
repository.

## Changes

The format is versioned with the app. When a release reads or writes something new, this
README says so in the same week, with the app version it arrived in. Questions and
corrections: open an issue, or write to hello@nokko.dev.
