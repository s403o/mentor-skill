# mentor-skill

A Claude Code skill that makes the AI teach you instead of doing the work for you.

You write the code. Claude asks questions, gives hints that escalate step by step, reviews what you wrote, and keeps a log of your weak areas.

## Why

Using AI as an answer machine is fast, but you stop building the skill yourself. This skill is a drill: you stay in charge of the thinking, and the AI acts as a mentor.

## Install

```bash
git clone https://github.com/s403o/mentor-skill.git
mkdir -p ~/.claude/skills/mentor
cp mentor-skill/skills/mentor/SKILL.md ~/.claude/skills/mentor/SKILL.md
```

Restart Claude Code. The skill only runs when you type `/mentor`.

## Usage

| Command | What it does |
|---|---|
| `/mentor <topic>` | Start or switch to a problem |
| `/mentor review` | Review the code you just wrote |
| `/mentor fast` | Urgent work; leave mentor mode for this problem |
| `/mentor log` | Show your weak areas |
| `/mentor exit` | End the session with a short recap |

Example:

```
/mentor my goroutines hang when the channel is closed
```

## How it works

**Context first.** Before any hint, Claude asks what you are trying to do, what you tried, and what you think the error means.

**Hint tiers.** Claude starts at Tier 1 and only moves up when you are stuck. Ask for a bigger hint and it jumps one tier.

| Tier | What it does |
|---|---|
| 1. Conceptual | Asks what you expect and why |
| 2. Directional | Points to where to look |
| 3. Structural | Points to a specific file or function |
| 4. Mechanism | Explains how it works, in plain language |
| 5. Scaffold | Pseudocode or a skeleton with `___` blanks; you fill them |

**Guess check.** If you guess, Claude asks for your reasoning before confirming.

**Syntax exception.** Ask for syntax only and you get a generic example of max 3 lines, never your solution.

**Review.** When your code works, Claude asks questions on correctness, security, performance, and readability. It does not fix anything.

**Fast mode.** For production issues or deadlines, `/mentor fast` leaves mentor mode for one problem. The topic is logged as `Debt` so you can come back to it.

**Weak area log.** After each problem, Claude appends to `~/.claude/mentor_log.md`:

```markdown
## 2026-11-10 | Go channels
- Weak: channel close semantics (Tier 3+)
- Solid: select with default (self-solved, Tier 1-2)
```

Over time this file shows where your understanding is thin.

## What it will never do

- Write or edit files in your project
- Give a complete solution, even as an "example"
- Give in to "just tell me"; it offers the next hint tier instead

## Credits

This skill merges ideas from two MIT-licensed projects:

- [ahmealy/mentor-agent-skill](https://github.com/ahmealy/mentor-agent-skill): base structure, hint tiers, guess check, weak area log
- [Sensei - Junior Mentor](https://github.com/github/awesome-copilot/blob/main/agents/mentoring-juniors.agent.md) from [github/awesome-copilot](https://github.com/github/awesome-copilot) by Thomas Chmara and François Descamps: context gathering, scaffold hints, review axes, urgency mode, session recap

## Contributing

Issues and PRs are welcome, especially better hint wording and real session examples.

## License

MIT
