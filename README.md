# LLM Wiki Agent

A `CLAUDE.md` system prompt that turns Claude Code into a persistent research wiki — with structured ingest, query, lint, and cross-referencing workflows for Obsidian users.

Drop a paper, an article, or a set of notes and say "ingest this." The agent reads the source, writes a structured summary page, creates or updates entity and concept pages, cross-references everything, flags contradictions against what's already in the wiki, and updates the index and log. Ask it a question and it synthesizes an answer from the wiki and files it as a synthesis page. Tell it to "lint the wiki" and it finds orphaned pages, stub concepts, missing backlinks, and stale claims.

The whole thing lives in Obsidian-compatible markdown. One `CLAUDE.md` file. No plugins, no databases, no external services.

---

## What you get

- **Structured ingest** — every source becomes a summary page with frontmatter, key claims, methodology notes, and wikilinks to related entities and concepts
- **Automatic cross-referencing** — entity and concept pages track every source that mentions them; new pages get backlinks added to existing pages
- **Contradiction detection** — if a new source contradicts something already in the wiki, it's flagged with `⚠️ Contradiction:` on the relevant page
- **Query + synthesis** — ask questions; get answers synthesized from wiki content with inline citations; substantial answers are filed as synthesis pages
- **Lint pass** — periodically check for orphaned pages, stub concepts, missing wikilinks, stale claims, and gaps in coverage
- **Append-only log** — every operation is recorded in `wiki/log.md` so you always know what happened when

---

## Prerequisites

- [Claude Code](https://claude.ai/code) (the CLI or desktop app)
- [Obsidian](https://obsidian.md) (recommended — the wiki uses Obsidian wikilink syntax, but any markdown editor works)
- Your vault or notes folder as the working directory for Claude Code

---

## Setup

### 1. Clone or download this repo

```bash
git clone https://github.com/StevenUmbrello/claude-wiki-agent.git
cd claude-wiki-agent
```

Or download the ZIP and extract it into your notes folder / Obsidian vault.

### 2. Personalize CLAUDE.md

Open `CLAUDE.md` and fill in Section 1 (Identity & Context) with your name, research domains, and any content creation work the agent should be aware of.

The fastest way is to use the setup prompt:

```
Open Claude Code in this directory and paste the contents of setup-prompt.md into the chat.
```

Claude will interview you and generate a personalized Section 1 block you can paste directly into `CLAUDE.md`.

### 3. Configure your source domains (Section 12)

In `CLAUDE.md`, update the source domains table to reflect your actual folder structure. These are folders the agent can read from but will never write to. Common examples:

- A folder of your own published papers
- A newsletter drafts folder
- An existing notes archive

Remove any rows you don't need. Add rows for any additional domains.

### 4. Open Claude Code in your wiki directory

```bash
# If you put the repo inside your Obsidian vault:
claude  # from inside the claude-wiki-agent/ directory

# Or set the working directory explicitly:
claude --dir /path/to/your/vault
```

Claude Code will automatically read `CLAUDE.md` at the start of every session.

### 5. Initialize the wiki

On your first session, say:

```
Initialize the wiki. Today's date is [DATE].
```

Claude will create the `wiki/` directory structure and write the initial `wiki/index.md` and `wiki/log.md`.

---

## Using the agent

### Ingest a source

Drop a file into `raw/articles/`, `raw/papers/`, or `raw/notes/`, then say:

```
Ingest raw/papers/Smith 2024 - AI and Cognition.pdf
```

Or paste text directly:

```
Ingest this: [paste article text]
```

Claude will:
1. Surface 3–5 key takeaways and ask what to emphasize
2. Write `wiki/sources/Smith 2024 - AI and Cognition.md`
3. Create or update entity pages for every person, institution, and publication mentioned
4. Create or update concept pages for every theory or framework engaged
5. Update any topic pages this source informs
6. Flag contradictions against existing wiki content
7. Update `wiki/index.md` and append to `wiki/log.md`

### Ask a question

```
What does the wiki say about [topic]?
What's the relationship between [concept A] and [concept B]?
Which sources engage with [author]'s argument about [X]?
```

Claude reads the relevant wiki pages and synthesizes an answer with `[[wikilink]]` citations. If the answer is substantial, it files it as a synthesis page in `wiki/synthesis/`.

### Lint the wiki

```
Lint the wiki.
```

Claude checks for:
- Pages with no inbound wikilinks (orphans)
- Unresolved `⚠️ Contradiction:` flags
- Concepts mentioned in source pages but without their own concept page
- Entity or concept names mentioned on pages without a wikilink
- Source pages superseded by newer ingests
- Gaps in coverage (new sources to find, questions to investigate)

Run this every few weeks or after a heavy ingest session.

---

## Folder structure

```
claude-wiki-agent/
  CLAUDE.md              — agent system prompt (this is what Claude reads)
  README.md              — this file
  setup-prompt.md        — run this once to personalize CLAUDE.md
  wiki/
    index.md             — master catalog of all wiki pages
    log.md               — append-only operation log
    sources/             — one page per ingested source
    entities/            — people, institutions, publications, projects
    concepts/            — theories, frameworks, ideas, terms
    topics/              — thematic cluster pages
    synthesis/           — filed query answers and cross-cutting analyses
  raw/
    articles/            — web-clipped articles
    papers/              — PDFs or markdown papers
    notes/               — meeting notes, transcripts, journal entries
    assets/              — images
```

---

## Page frontmatter

Every wiki page opens with YAML frontmatter so Obsidian and other tools can query the wiki programmatically.

**Source page** (`wiki/sources/`):
```yaml
---
type: source
title: "Full title"
author: "Last, First; Last, First"
year: YYYY
source-type: paper | article | book | chapter | podcast | note | transcript
tags: []
wiki-pages-created: []
wiki-pages-updated: []
---
```

**Entity page** (`wiki/entities/`): person, institution, journal, project, or publication.

**Concept page** (`wiki/concepts/`): theory, framework, idea, or term.

**Topic page** (`wiki/topics/`): thematic cluster linking related sources, entities, and concepts.

**Synthesis page** (`wiki/synthesis/`): filed answer to a query, cross-cutting analysis, or comparison.

---

## Customization

### Research domains

The agent uses the research domains in Section 1 to make editorial judgments: which concepts deserve their own page, which entities are worth tracking, which connections are worth flagging. Be specific. "AI ethics, philosophy of mind, moral psychology" produces better results than "philosophy."

### Source domains

Any folder listed in Section 12 is treated as read-only. The agent can ingest from it but will never write to it. Add your published papers folder, your blog drafts folder, or any other archive you want the agent to draw from without touching.

### Contradiction detection

The `⚠️ Contradiction:` flag is added automatically when a new source makes a claim that conflicts with something already in the wiki. Review these flags periodically and resolve them: either update the concept page to reflect the current consensus, or note that the contradiction is live and unresolved.

### Running without Obsidian

The wiki uses Obsidian wikilink syntax (`[[Page Name]]`) for internal links. If you're not using Obsidian, this still works fine as plain markdown — the links just won't be clickable. Any text editor works.

---

## Tips

- **Keep raw/ organized.** The agent ingests from `raw/` but the folder can get messy fast. Use subfolders or date prefixes if you ingest heavily.
- **Ingest in batches, not one-by-one.** Tell Claude "ingest all files in raw/papers/ that aren't yet in the wiki." It will work through them and update the index at the end.
- **The log is your audit trail.** If you ever wonder what the agent did in a previous session, `wiki/log.md` has the answer.
- **Lint after gaps.** After a few weeks away from the wiki, run a lint pass before ingesting new sources. It surfaces what's gone stale and gives you a clean baseline.
- **Synthesis pages are underrated.** When you ask the agent a question worth keeping, tell it to file the answer. Over time, `wiki/synthesis/` becomes a library of your own thinking.

---

## License

MIT
