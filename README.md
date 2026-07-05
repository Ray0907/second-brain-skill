# second-brain

A Claude Code skill that turns anything you're learning — an article,
transcript, book excerpt, code pattern, research finding — into a permanent,
self-organizing markdown knowledge base, using the **PACER** learning
framework instead of dumping raw notes.

The core idea: information only sticks if it goes through the right
*digestion* process for its type. Capturing without digesting is just future
homework — a note that skips its process is worse than no note, because it
feels like it was learned when it wasn't.

## What it does

Two modes:

- **Capture** — share something you're learning ("add this to my second
  brain", "I just read/watched X"), and the skill classifies each distinct
  idea into a PACER type, then does the digestion step *in the note, right
  now* (not left as homework for later):
  - **P**rocedural → write what practicing it would look like, mark unpracticed
  - **A**nalogous → write an actual critique (where it holds, where it breaks)
  - **C**onceptual → link to at least one related existing note
  - **E**vidence / **R**eference → store it short, no essay
- **Roast** — say "roast me" / "quiz me" / "check my second brain", and it
  quizzes you against your *weakest* stored notes (unpracticed procedures,
  uncritiqued analogies, unconnected concepts) to expose what was never
  properly digested, instead of just re-testing material that's already
  solid.

## Why PACER instead of one big note pile

Not all information should be remembered the same way. The PACER framework
(Procedural / Analogous / Conceptual / Evidence / Reference) identifies which
category a piece of information belongs to, because each category has a
different process for making it stick:

| Type | What it is | Digestion process |
|---|---|---|
| **P**rocedural | How to do something (a technique, workflow, code pattern) | Practice — must be used, not memorized |
| **A**nalogous | Reminds you of something you already know | Critique — how far does the analogy hold, where does it break |
| **C**onceptual | Facts, theories, relationships, the "what" | Mapping — connect to other concepts, no note is an island |
| **E**vidence | Concrete facts/stats/cases that support a concept | Store + rehearse — apply it in an explanation later |
| **R**eference | Nitty-gritty detail with no conceptual weight | Store + rehearse via recall, not re-reading |

Using the wrong process for a category (e.g. trying to memorize a
procedural technique instead of practicing it) makes retention far less
effective — this is what the skill actively steers you away from.

**PACER is the direction; everything else (flat vault notes vs. Projects) is
just a detail about where the note lands** — the classification logic never
changes based on location.

## Storage backends

Works with two backends, same PACER logic either way:

1. **Obsidian** (recommended) — if an Obsidian MCP tool is connected (vault +
   Local REST API plugin), notes are written straight into your vault and use
   native `[[wikilink]]` syntax, so Obsidian's graph view visualizes the
   connections automatically. See [`references/obsidian-setup.md`](references/obsidian-setup.md)
   for the one-time setup (Claude Desktop, Obsidian, the Local REST API
   plugin, and the `claude mcp add-json` command to wire them together).
2. **Plain files** — falls back to flat markdown files at `~/.second-brain/`
   with no setup required. This is the default for most users.

## Projects

For a large recurring area of work (not a single article — a whole domain
like "my YouTube channel" or "job search"), the skill can scaffold a
`projects/<name>/` structure: a scoped `CLAUDE.md` (built by interviewing you
one question at a time), plus `Inputs/ Process/ Outputs/ Feedback/` folders.
PACER capture/roast logic inside a project is identical to the vault root —
only the location and the added project context differ.

The skill deliberately does **not** reimplement scheduling or workflow
automation — it points you at Claude Code's own `/schedule` skill (e.g. for
clearing out a project's `Inputs/` on a cadence) and `skill-creator` (for
turning a repeated workflow into a real skill), rather than rebuilding either
inside second-brain.

## Installation

Drop this directory into your Claude Code skills folder:

```bash
cp -r second-brain ~/.claude/skills/
```

Or clone this repo directly there:

```bash
git clone <this-repo-url> ~/.claude/skills/second-brain
```

No further setup needed for the plain-files backend. For the Obsidian
backend, follow [`references/obsidian-setup.md`](references/obsidian-setup.md).

## Usage

Just talk naturally — the skill triggers on intent, not a slash command:

```
"I just read this article on X, add it to my second brain"
"help me learn this transcript: <paste>"
"roast me"
"quiz me on what I've stored"
"check my second brain"
```

## License

MIT
