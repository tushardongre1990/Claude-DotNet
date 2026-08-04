# Project conventions for Claude Code

## About this repo
Personal C#/.NET/ASP.NET Core study notes, 25 numbered topics (see `README.md` for the roadmap and progress table). Originally scoped as senior-developer interview prep, now used to actively learn the material — read section by section, one topic `README.md` at a time.

## Writing style for topic README files
Write every topic `README.md` assuming the reader has **zero background** — not just senior-level quick reference:

- Spell out every abbreviation/acronym in full parentheses the first time it appears **in that file** (e.g. `CLR (Common Language Runtime)`), even "obvious" ones (GC, JIT, IL, API, LTS, DLL, etc.). Don't assume a term defined in one topic file carries over to another file.
- Add ground-up explanations for foundational concepts the first time they're used — e.g. value vs reference types, boxing, stack/heap, pointers/references, immutability, Big-O notation, exceptions/call stack. Use blockquote callouts formatted as `> **New term — X.**` so they read as asides without breaking the flow of the denser interview content.
- Keep the existing interview-prep depth and structure (Mermaid diagrams, tables, "interview trap"/"common interview question" callouts, cheat sheets) — this is additive, not a rewrite into a beginner tutorial.

See `01-CSharp-Basics/README.md` for the reference example of this treatment already applied.

### Readability (applies to all writing/editing in topic README files)
Beginner-friendly must not mean bloated or textbook-dense. Apply these rules whenever writing or revising prose in a topic file:

- **One idea per sentence.** Split any sentence carrying multiple clauses joined by em-dashes/semicolons into separate sentences.
- **No nested parentheticals.** A parenthetical that itself needs a parenthetical (or runs longer than ~5-6 words) should become its own sentence instead, e.g. `CLI (Common Language Infrastructure) is the ECMA standard for X.` + a following sentence disambiguating it from the unrelated "Command Line Interface" meaning — not one sentence stacking both clarifications inside nested parens.
- **Prefer bullets over dense paragraphs** whenever a paragraph is really "here are N characteristics/steps" — state that up front, then list them, then elaborate if needed. (See section 3's "The stack has three defining characteristics" treatment in `01-CSharp-Basics/README.md` for the pattern.)

Goal: a senior dev can still skim it for depth, but a beginner isn't stalled parsing a 60-word sentence with three parenthetical asides.

## Handling follow-up / deep-dive questions
When asked to explain something in more depth that's already covered in a topic `README.md`, fold the resulting explanation into that file at the relevant section — don't just leave it in chat:

- Match the file's existing style: `###` subheadings for deeper dives, fenced ` ```csharp ` code blocks for examples, `> **New term — X.**` blockquotes for definitions (see section 3 of `01-CSharp-Basics/README.md` for the pattern: "Stack frames, concretely," "Copy semantics, concretely," "Generational GC, in more depth," etc.).
- Only create a runnable code project/file when actually executing something adds proof (benchmarking, verifying output, measuring allocations) — plain fenced code blocks inline in the README are the default.
- Always answer in chat too; the file edit is additive to the conversation, not a replacement for it.
