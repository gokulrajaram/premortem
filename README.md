# premortem

A [Claude Code](https://claude.com/claude-code) skill that runs a **premortem** on any plan, launch, product, hire, strategy, or decision before you commit to it.

Instead of asking "what could go wrong?" (which gets you cautious, hedged answers), the skill jumps 6 months into the future, assumes the plan has already failed, and works backward to explain *why it died*. This shift — from prospective risk assessment to retrospective failure narration — is what psychologist Gary Klein called a "premortem." Daniel Kahneman called it his single most valuable decision-making technique. Wharton and Cornell researchers found it significantly improves people's ability to identify the actual causes of future outcomes.

The skill exists because Claude defaults to agreeable, optimistic responses. Ask "is this a good plan?" and it will find reasons to say yes. The premortem frame breaks that pattern by forcing the model into "this is dead, explain how it died" mode — which produces specific, honest, actionable failure scenarios instead of polite risk hedging.

## Install

```bash
git clone https://github.com/gokulrajaram/premortem.git
mkdir -p ~/.claude/skills
cp -r premortem ~/.claude/skills/
```

Then in Claude Code, invoke it with `/premortem` followed by what you want stress-tested. Or just describe the plan in plain English and use a triggering phrase — the skill fires automatically.

```
/premortem I'm about to launch a $297 live workshop for marketing managers at 10–50 person companies. 50 seats. Topic is using Claude for marketing teams.
```

You can also attach a brief, deck, plan doc, or any supporting materials and the skill will pull context from them.

## Triggering phrases

The skill is designed to fire on any of these:

**Mandatory triggers** — `premortem this`, `premortem my`, `run a premortem`, `what could kill this`, `future-proof this`, `stress test this plan`, `what am I missing here`, `find the blind spots`.

**Strong triggers** — `what could go wrong`, `am I missing anything`, `poke holes in this`, `where will this break`, `devil's advocate this`.

It will *not* trigger on simple feedback requests, factual questions, or general LLM Council–style "give me multiple perspectives" asks. Use it when the cost of being wrong is high and you can still change course.

## How a session runs

1. **Context gathering.** The skill scans the current conversation and your workspace (`CLAUDE.md`, `memory/`, attached files) for context. If it's missing one of three things — *what is it*, *who is it for*, *what does success look like* — it asks one focused question at a time until the bar is met. No long forms.
2. **Frame setting.** It explicitly tells you: "It is 6 months from now. This plan has failed." That sentence is the entire psychological mechanism — it's not optional dressing.
3. **Raw premortem.** It generates every genuine reason the plan could have died. Could be 4. Could be 9. Whatever's real for *this* plan, not a forced count.
4. **Parallel deep-dives.** It spawns one investigator sub-agent per failure reason, all in parallel. Each one writes the failure story, names the underlying assumption, and lists 1–2 early warning signs.
5. **Synthesis.** It produces a five-part report: most likely failure, most dangerous failure, hidden assumption, revised plan with concrete changes, and a 3–5 item pre-launch checklist.
6. **Visual output.** A self-contained dark-themed HTML report (`premortem-report-[timestamp].html`) for scanning, plus a full Markdown transcript (`premortem-transcript-[timestamp].md`) for digging in.

## What you get back

Two files in your workspace and a three-sentence summary in chat:

```
premortem-report-[timestamp].html    # visual report — scan this first
premortem-transcript-[timestamp].md  # full reasoning — read if you want to dig in
```

The synthesis is the product. The individual failure cards are there for the user who wants to interrogate each scenario.

## Design notes

- **Honest, not polite.** The skill is explicitly told not to sugarcoat. If your plan has serious problems, it will say so directly. That's the whole point.
- **Concrete revisions.** Every suggested fix has to be something you could actually do this week. Not "consider your pricing." Specifically: "run a $47 pilot with 20 people before committing to the full launch."
- **Comprehensive but not padded.** The skill is told to find every genuine failure reason and stop there — no forcing a round number, no inventing weak failures to pad the list.
- **Not the LLM Council.** The council gives multiple perspectives on a current decision. The premortem narrates a single failed future. Different mechanism, different output.

## What's in the skill

- **`SKILL.md`** — the prompt. Defines the context-gathering bar, the frame-setting, the parallel deep-dive workflow, and the synthesis structure.

## License

MIT.
