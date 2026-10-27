# verification-loop

A build-time self-verification loop for AI agents, written as a
portable skill. The agent checks its own work as it builds,
fixes the small failures it is authorized to fix, and escalates
to you rarely, in a fixed format, with evidence. Your attention
goes to decisions. The trivia goes to the loop.

## Why it exists

It was specified by a non-engineer who works with AI agents
daily and kept hitting the same two failure modes: an agent that
asked him about every little thing, and an agent that fixed
nothing and let him find the breakage later. The loop is the
discipline he asked for instead: verify continuously, repair
inside a written authorization envelope, and never let a small
failure become either an interruption or a surprise.

It proved itself the day it was written. A test suite crashed
mid-run, in a way that looked exactly like the day's code changes
had broken it. The loop's state-check rule (establish what
actually happened before reacting) traced the crash to the test
environment itself: earlier runs had exhausted the environment's
own hourly request budget, so the run's first write was refused
and a later check read a row that was never created. Good code
stayed untouched, the environment was cleaned, the suite passed
in full, and the human got a verdict with evidence instead of a
false alarm.

## What it does

- Runs every applicable check after each checkable step.
- Diagnoses failures from evidence, and treats a broken check
  (harness, fixtures, environment) as a different problem from
  broken work.
- Auto-fixes only when a failure is objectively verifiable, the
  fix is local and reversible, and it is not a decision class.
- Enforces attempt discipline: state-check before any repeat,
  evidence-led retries only, a cap that stops loops without
  stopping repairs.
- Stops and escalates, once, in a fixed format, when a failure
  needs taste, scope, money, or a rule change, and treats
  workarounds around rules as findings, never as fixes.
- Keeps a loop log inside the deliverable, so small failures are
  read about after they are fixed, not during.

## Use it with any AI

The skill is a single Markdown file, `SKILL.md`, in the agent
skills format. An `evals.md` ships alongside it: four
adversarial scenarios for testing whether an agent has
actually adopted the loop. To use it:

- **Claude, and other agents with a skills system:** drop the
  `verification-loop` folder into your skills directory.
- **Any other assistant:** paste `SKILL.md` into the project's
  instruction file, system prompt, or chat, and fill in the
  authorization envelope (checks, auto-fix scope, never-touch
  list, decision classes, attempt cap) for your project. The
  defaults in the file are a sane start.

The envelope is the point. The loop is only as safe as the
boundaries you write into it, and only as useful as the checks
you give it to run.

## Provenance and license

Specified by Rodel Oiga; written and refined in his studio's
working practice, then extracted into portable form. The
escalation block's shape was adapted from a protocol draft he
submitted for review during the same project. Two closing moves
in the loop log, the explicit not-verified list and the
prove-nothing-else-moved snapshot, were adopted from the
receipts-based approach to verifying agent work published at
doanhkhoa.tech ("evidence, not done"), found in a prior-art
survey run before publication; the survey also confirmed the
loop's core (the authorization envelope, attempt discipline,
and escalation shape) is not a restatement of the Ralph loop,
Superpowers' verification-before-completion, or published
loop-engineering workflows, which solve adjacent problems.
Licensed under MIT (see `LICENSE`); change the license if you
publish a fork.
