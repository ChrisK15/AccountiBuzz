# AccountiBuzz — Project Guide

## Jira
- Site: `comp586.atlassian.net` (NOT `ai-mentor.atlassian.net` — different project)
- Project: `SCRUM`
- Board: https://comp586.atlassian.net/jira/software/projects/SCRUM/boards/1
- Account: Timothy Do (`thinhdo1410@gmail.com`)

> Note: `ai-mentor.atlassian.net` is a separate, unrelated project. Do not mix them up.

## Agent Workflow

Each story follows this pipeline:

```
project-manager → technical-architect → code-coach → quality-reviewer
```

1. **project-manager** — pulls the current sprint from Jira, writes `.claude/context/current-story.md`
2. **technical-architect** — reads the story, designs the solution, writes `.claude/specs/SCRUM-XX.md`
3. **code-coach** — reads the spec, guides implementation step-by-step, tracks progress in `.claude/progress/SCRUM-XX-progress.md`
4. **quality-reviewer** — reviews completed code against spec and acceptance criteria, writes `.claude/reviews/SCRUM-XX-review.md`

## Project Structure

```
.claude/
├── agents/          # The 4 agent definitions
├── context/         # current-story.md (updated by project-manager)
├── specs/           # SCRUM-XX.md files (written by technical-architect)
├── progress/        # SCRUM-XX-progress.md (tracked by code-coach)
└── reviews/         # SCRUM-XX-review.md (written by quality-reviewer)
```

## Starting a Session

Always start with:
> "Check Jira and tell me what we're working on"

This invokes **project-manager** to sync from Jira and update context.
