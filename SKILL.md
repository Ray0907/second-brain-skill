---
name: second-brain
description: Use whenever the user shares something they're learning (an article, transcript, book excerpt, code pattern, research finding) and wants to retain it, or says things like "help me learn this", "add this to my second brain", "I just read/watched X" — captures it into a permanent markdown knowledge base classified by learning type. Also use when the user wants to test their retention with phrases like "roast me", "quiz me", "test what I know", or "check my second brain" — pulls from stored notes to expose gaps instead of just re-reading them back. Also use when the user asks a question they want answered from their stored notes, e.g. "according to my second brain", "answer this from my second brain", "what do my notes say about X".
---

# Second Brain

A permanent markdown knowledge base at `~/.second-brain/`, built on the PACER
learning framework: information only sticks if it goes through the right
*digestion* process for its type, not just wide passive consumption. Capture
without digestion is just future homework — a note that skips its process is
worse than no note, because it feels like it was learned.

Three modes: **capture** (file new material), **roast** (quiz against what's
stored, and call out what was never properly digested), and **consult**
(answer a question from what's stored).

## Hierarchy: PACER is the direction, everything else is placement detail

**PACER decides *how* a piece of knowledge gets digested** — procedural,
analogous, conceptual, evidence, or reference — and that classification
logic is the same everywhere, whether the note ends up at the vault root or
inside a project. It never changes based on where a note lives.

**Everything else in this skill is a detail about *where the note lands***,
not a second learning framework:

- Flat vault-root notes (`<type>/<slug>.md`) — the default container.
- **Projects** (`projects/<name>/<type>/...`) — an alternate container for a
  large recurring area of work. A project still stores PACER-classified
  notes inside it; it just adds a folder structure, a scoped `CLAUDE.md`, and
  pointers to `/schedule` and `skill-creator` for project-specific upkeep.

If you're ever unsure whether something is a PACER question or a placement
question: "what type of information is this and how do I make it stick" is
PACER; "which folder does this note belong in" is placement.

Notes normally live flat under `<type>/<slug>.md` at the vault root. For a
large recurring area of work (not a single topic — a whole domain like "my
YouTube channel" or "job search"), see **Projects** below instead of filing
everything into the flat PACER folders.

## User profile

If the Obsidian backend is active (see below) and a `CLAUDE.md` exists at the
vault root, read it before capturing or roasting — it holds who the user is,
their goals, how they want explanations delivered, and their known strengths
and weaknesses. Use it to shape tone and depth, not to change the PACER
classification logic itself. If it doesn't exist, skip this — it's optional
context, not a requirement to create one unprompted.

## Storage backend

Two possible backends, same PACER logic either way — only where the note
lands changes.

1. **Check for an Obsidian connection first**: look for an MCP tool whose
   name contains `obsidian` (e.g. from `claude mcp add-json obsidian-vault`).
   If one exists, use it as the backend — this is the setup described in
   `references/obsidian-setup.md`, and it means the user already has an
   Obsidian vault with the Local REST API plugin running.
   - Write notes through the Obsidian MCP tool, into the same
     `<type>/<slug>.md` folder structure, in the user's vault root instead of
     `~/.second-brain/`.
   - Obsidian's `[[wikilink]]` syntax already does what the "Connections"
     section needs — link directly with `[[other-note-slug]]`, Obsidian
     builds the graph view automatically.
   - Read existing notes the same way (via the MCP tool) when scanning for
     roast mode or searching for connections.
   - **The whole vault is reference material, not just the PACER folders.**
     When digesting a capture (finding connections, critiquing an analogy) or
     picking roast questions, search the entire vault with
     `obsidian_simple_search` / `obsidian_complex_search` — the user's
     pre-existing notes count as "related concepts" and may be linked with
     `[[wikilinks]]` exactly like second-brain notes. Never modify a
     non-PACER vault note; read and link only.
2. **No Obsidian MCP tool found**: fall back to plain files at
   `~/.second-brain/`, exactly as described below. This requires no setup and
   is the default for most users.
3. If the user asks to switch to Obsidian, or asks how, point them at
   `references/obsidian-setup.md` rather than walking them through it inline
   — it's a one-time setup with several manual steps outside this
   conversation (installing Obsidian, enabling a plugin, copying an API key).

### Status check

If the user asks "which backend am I using" or "is Obsidian connected", don't
guess — report what step 1 above actually found: name the Obsidian MCP tool
if one showed up, or state plainly that none was found and plain files at
`~/.second-brain/` are in use. A wrong guess here just relocates notes to the
wrong place next capture.

## Projects

A project is a large recurring area of work, not a single piece of material
— e.g. "youtube-channel", "job-search", "client-acme". Use one when the user
names an ongoing domain they want Claude scoped to, not for a one-off article
they want filed.

Create it only when the user asks to start a project, or a capture clearly
belongs to a domain big enough to need its own space (ask first — don't
infer this silently from one note).

Structure, created under `projects/<project-name>/` at the vault root (or via
the Obsidian MCP tool at the same relative path):

```
projects/<project-name>/
  CLAUDE.md      # this project's goal + Claude's role in it — same idea as
                 # the vault-root CLAUDE.md, scoped to just this project
  Inputs/        # raw material dropped in, not yet processed
  Process/       # working files while Claude is actively working
  Outputs/       # finished deliverables
  Feedback/      # results, metrics, notes on what worked
```

Write the project's `CLAUDE.md` by interviewing the user **one question at a
time** (not a batched multi-question ask like the vault-level profile) —
project setup has more moving parts than a general profile (goal, current
phase, constraints, existing material), and questions asked one at a time
surface follow-ups a batched ask would miss. Cover at minimum: the project's
one goal, Claude's specific role in it, and what (if anything) already exists
for it. Wait for each answer before asking the next. Guessing instead of
asking produces vague filing decisions later.

PACER capture/roast logic is unchanged inside a project; only the location
and the added `CLAUDE.md` context differ from flat vault-root notes.

A couple of things stay adjacent to a project but are not this skill's job —
point at the existing mechanism rather than rebuilding it here:

- **Keeping `Inputs/` from piling up**: if the user wants it processed on a
  cadence (e.g. "clear my Inputs every morning"), tell them to set it up with
  the `/schedule` skill rather than expecting second-brain to run on its own.
- **A repeated workflow inside the project** (e.g. always drafting client
  emails the same way): suggest `skill-creator` to save it as a real skill,
  instead of storing it as a procedural note that has to be manually re-read
  each time.

Live data feeds (calendar, email, Slack) are not part of the default project
structure — only relevant if a specific project's own goal depends on one,
and that project's `CLAUDE.md` should say so explicitly rather than it being
assumed for every project.

## PACER types and their digestion process

Every piece of material is one (or more) of these. Identify the type(s)
before filing — this determines the note's shape, not just its folder.

| Type | What it is | Digestion process | Folder |
|---|---|---|---|
| **P**rocedural | How to do something (a technique, a workflow, code pattern) | Practice — must be used, not memorized. For a problem-solving technique specifically, frame the practice run as Pólya's four steps (understand → plan → carry out → look back), not just "try it once" | `procedural/` |
| **A**nalogous | Reminds you of something you already know | Critique — how far does the analogy hold, where does it break | `analogous/` |
| **C**onceptual | Facts, theories, relationships, the "what" | Mapping — connect to other concepts, no note is an island | `conceptual/` |
| **E**vidence | Concrete facts/stats/cases that support a concept | Store + rehearse — apply it in an explanation later | `evidence/` |
| **R**eference | Nitty-gritty detail with no conceptual weight (a constant, a name) | Store + rehearse via recall, not re-reading | `reference/` |

Most real material is a mix. File the dominant type as the primary note, and
mention secondary types in the frontmatter — don't force a single label.

## Capture mode

1. **Read the material** the user shared (pasted text, a doc, a summary of
   something they watched/read).
2. **Classify** each distinct idea into PACER types. Don't lump an entire
   article into one note if it contains, say, three conceptual ideas and one
   procedural technique — split them.
3. **Write one note per idea** to `~/.second-brain/<type>/<slug>.md` using the
   shape below. Create the folder structure on first use if it doesn't exist.
4. **Do the digestion step yourself, in the note, right now** — don't just
   store raw content and leave the hard part for later, that's the exact
   failure mode the framework warns against:
   - Procedural → write what practicing this would concretely look like, mark it unpracticed
   - Analogous → write an actual critique (where it holds, where it breaks)
   - Conceptual → link to at least one related existing note (search first — with the Obsidian backend, search the whole vault, not just `conceptual/`); if genuinely nothing relates yet, say so explicitly
   - Evidence/Reference → keep it short, store it, no essay
5. **Update `~/.second-brain/index.md`** with a link to the new note under its
   type heading. This is the graph — without it, notes are just files in
   folders, not a connected brain.
6. Tell the user in 1-2 lines what got filed and as what type. Don't dump the
   full note content back into the conversation.

### Note format

```markdown
---
type: conceptual        # primary PACER type
secondary: [analogous]  # optional, other types this note also touches
topic: <short topic name>
date: <YYYY-MM-DD>
source: <where this came from, or "conversation">
practiced: false        # procedural only — flip true once actually used
---

# <Title>

<The actual content, in your own words, not copy-pasted verbatim>

## Connections          <!-- conceptual: required -->
- Relates to [[other-note-slug]] because ...

## Critique              <!-- analogous: required -->
- Holds because ...
- Breaks down when ...
```

Only include the section relevant to the note's type. Evidence/Reference
notes skip both — just the content and frontmatter.

## Consult mode

The user asks a question and wants it answered *from their stored knowledge*
("according to my second brain, how should I think about this?",
"what do my notes say about X").
This is retrieval + grounded answering, not capture and not a quiz.

1. **Search the notes** for anything relevant: `index.md` + the type folders,
   or with the Obsidian backend, `obsidian_simple_search` /
   `obsidian_complex_search` across the whole vault.
2. **Answer grounded in what was found**, citing which notes it came from
   (`[[note-slug]]`). Prefer the user's own words/framing from the notes over
   a generic textbook answer — the point is surfacing *their* accumulated
   thinking.
3. **Keep sources honest.** Clearly separate "your notes say X" from "my
   general knowledge adds Y". If nothing in the vault is relevant, say so
   plainly and just answer normally — never dress up general knowledge as if
   it came from their notes.
4. If answering exposed a gap or produced a genuinely new connection, offer
   (don't silently do it) to capture it as a new note or add a `[[wikilink]]`
   between the notes involved.

## Roast mode

The point is exposing what was never actually digested, not re-testing
material that's already solid. Be direct about which failure it is.

1. **Read `~/.second-brain/index.md`** and scan notes across types. If the
   vault doesn't exist yet or is empty, say so — there's nothing to roast.
2. **Find the weak spots** — this is what makes it a roast, not a quiz:
   - Procedural notes still marked `practiced: false`
   - Analogous notes with a thin or missing critique section
   - Conceptual notes with zero connections (isolated nodes in the graph)
   - Evidence/Reference notes never referenced again anywhere else
   - Topics with only one note where the material clearly had more angles
3. **Quiz on 3-5 of the weakest notes**, one at a time, waiting for the
   answer before moving on:
   - Procedural → ask them to walk through doing it, not describe it
   - Analogous → ask where their analogy breaks (if they never critiqued it, this is where it shows)
   - Conceptual → ask how this connects to something else in the vault, or pose a small problem to solve with it — walk it through Pólya's four steps rather than accepting a jump straight to the answer: what's known/unknown (understand), "do you know a related problem?" (plan — this is the connection question), execute (carry out), then "can you use this result or method for some other problem?" (look back)
   - Evidence/Reference → direct recall, no peeking
4. **Grade bluntly.** If they recite the note back, that's memorization, not
   learning — say so. If they can extend, apply, or connect it unprompted,
   that's real. Name the specific gap ("you've never practiced this," "this
   concept has zero connections after two weeks") rather than generic
   encouragement.
5. **When an answer falls short, fix it through that note's own PACER
   digestion process — not a generic explanation.** Explaining the right
   answer in prose just adds another layer of passive consumption, the exact
   thing PACER exists to avoid. Instead:
   - Procedural → don't describe the fix, make them redo the task with the
     corrected approach right now
   - Analogous → don't hand them the critique, push back on their answer and
     make them re-derive where it holds/breaks themselves
   - Conceptual → don't state the connection or the answer, roll back to
     the plan step (the connection question from the quiz above) and make
     them redo it out loud before executing again
   - Evidence/Reference → direct correction is fine here, there's no deeper
     process to route through
   Only move on once they've produced the corrected version themselves, not
   after you've explained it to them.
6. **Update the note** whose gap was exposed: flip `practiced: true` if they
   just demonstrated it, add the connection they just made, or leave a note
   that it's still shaky. The roast should leave the vault stronger, not just
   the user's ego bruised.

## Notes

- Never fabricate connections or critiques to fill out a section — an honest
  "no connection found yet" is more useful than a forced one.
- If the user's paste is pure reference/procedural noise with no conceptual
  core, don't invent one. Not everything needs a mind-map entry.
