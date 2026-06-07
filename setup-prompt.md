# Setup Prompt

Paste the text below into Claude Code (opened in your `claude-wiki-agent/` directory) to have Claude interview you and generate a personalized Section 1 for your `CLAUDE.md`.

---

## Prompt to paste

```
I'm setting up the LLM Wiki Agent for my own research. I need you to help me personalize Section 1 of CLAUDE.md.

Ask me the following questions one at a time, wait for my answer, then move to the next:

1. What's your name? (This is how the agent will refer to you throughout all operations.)

2. What institution or organization are you affiliated with, if any? (Optional — just say "none" if you'd prefer not to include it.)

3. What are your primary research domains or areas of interest? Be specific — the agent uses this to make editorial judgments about which concepts and entities deserve their own wiki pages. Give 3–6 areas, e.g., "philosophy of technology, AI ethics, moral psychology" or "constitutional law, institutional design, comparative politics."

4. Do you have any public-facing content work the agent should know about? For example: a newsletter, blog, podcast, YouTube channel, or academic publication series. If yes, describe it briefly (name, topic, audience). If no, just say "none."

5. Are there any existing folders in your vault or notes directory that should be treated as immutable source domains — folders the agent can read from to inform the wiki but should never write to? Common examples: a folder of your own published papers, a blog drafts folder, an archive of past notes. List each folder by name and a one-line description of what it contains. If none, say "none."

After I've answered all five questions, generate a personalized Section 1 block formatted exactly like this:

---

## 1. Identity & Context

**User**: [NAME] ([INSTITUTION — omit line if none])

**Research domains**: [LIST]

**Content creation**: [DESCRIPTION — or "none"]

**Your role**: You write and maintain the wiki. [NAME] curates sources, directs analysis, and asks questions. You do the bookkeeping — summarizing, cross-referencing, filing, flagging contradictions, keeping everything current.

---

Then tell me: copy this block and replace Section 1 in your CLAUDE.md with it. Also list any source domains I mentioned so I can update Section 12.
```

---

## What to do with the output

1. Copy the generated Section 1 block.
2. Open `CLAUDE.md`.
3. Replace the existing Section 1 (lines from `## 1. Identity & Context` down to the `---` separator before `## 2. Folder Structure`) with the generated block.
4. Update Section 12 (Source Domains table) with any folders you mentioned in question 5.
5. Save `CLAUDE.md`.

That's it. Open a new Claude Code session in this directory and say "initialize the wiki" to begin.
