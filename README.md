# multi-agent-designer

A Claude Code skill for designing multi-agent LLM systems, including Claude Code skills and plugins that
orchestrate subagents. It first decides whether multi-agent is justified at all, then specifies topology,
per-agent contracts, handoffs, state, termination, verification, conflict handling, runtime guards, and an
assumption ledger of scaffolding to remove as models improve.

The guidance is grounded in empirical failure research: the MAST taxonomy, production post-mortems, and
generator/evaluator harness write-ups. `references/evidence.md` rates each claim as structural (transfers
to stronger models) or capability-bound (re-test on current models).

## Install

As a plugin, from inside Claude Code:

```
/plugin marketplace add <path-or-git-url-of-this-repo>
/plugin install multi-agent-designer@multi-agent-designer
```

Or copy the skill directory directly:

```sh
# personal, available in every project
cp -R skills/multi-agent-designer ~/.claude/skills/

# or scoped to one project
cp -R skills/multi-agent-designer <project>/.claude/skills/
```

## Usage

The skill triggers when you ask to design, build, review, debug, or simplify a multi-agent system, agent
team, orchestrator/worker pipeline, planner-generator-evaluator loop, or any skill that spawns subagents.
It produces a design doc (`templates/design-doc.md`) plus one contract per agent
(`templates/agent-contract.md`), and builds the system from that design if asked.

## Layout

```
.claude-plugin/
  plugin.json            plugin manifest
  marketplace.json       single-plugin marketplace so the repo can be added directly
skills/multi-agent-designer/
  SKILL.md               workflow (steps 0–12) and output spec
  references/
    failure-modes.md     MAST's 14 failure modes, mitigations, review checklist, post-mortems
    patterns.md          SOP artifacts, sprint contracts, typed handoffs, constraint tiers, blockers, runtime
    evidence.md          source and strength of each claim; structural vs capability-bound
    claude-code.md       mapping the design onto skills, subagents, hooks, and files
  templates/
    design-doc.md
    agent-contract.md
```

## Editing the skill

- `SKILL.md` is loaded whenever the skill triggers; keep it to the workflow and push detail into
  `references/`, which are read on demand.
- The `description` in `SKILL.md` frontmatter controls when the skill triggers. Change it deliberately.
- When adding a claim with a number attached, add its source and rating to `references/evidence.md`.
- Bump `version` in `.claude-plugin/plugin.json` when publishing changes.

## License

MIT. See `LICENSE`.
