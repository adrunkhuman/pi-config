---
name: issue-writing
description: Use when writing or editing an issue title or body. Defaults to concise resumption briefs for personal projects and future LLM sessions, preserving the problem, decisions, useful evidence, and next actions without investigation history.
---

# Writing an issue title and body

## Audience and purpose

- Default to personal-project issues used as durable memory for the user and future LLM sessions. Write a resumption brief, not a formal maintainer handoff.
- Make it possible to resume without the original conversation: preserve the actual problem, desired outcome, relevant code entry points, decisions and their rationale, remaining uncertainty, and concrete next action where known.
- Remove the journey; preserve what we learned. Summarize consequential dead ends and why they failed so the next session does not repeat them.
- For public-facing reports or an explicit repository template, adapt to the maintainer's needs instead. Do not impose that ceremony on personal notes.

## Scope and evidence

- Describe the current understanding of the actual problem, not the journey taken to discover it.
- Inspect relevant code, runtime output, and existing evidence before making claims. Separate confirmed observations from suspected causes; do not present a hypothesis as a diagnosis.
- Omit stream-of-consciousness notes, debugging chronology, discarded theories, unsuccessful commands, and intermediate fixes unless they materially narrow the cause or prevent duplicated work.
- Include a failed workaround or investigation finding only when it changes what the reader should do next. Summarize the conclusion and supporting evidence, not every step.
- Never invent reproduction steps, affected versions, measurements, logs, screenshots, uploads, or code references. Ask for material missing information or explicitly mark the uncertainty.
- This skill governs writing, not authorization to publish, upload, modify code, or change issue metadata. Follow the user's requested scope.

## Title

- Use a short, specific title naming the affected behavior and failure or requested outcome.
- Follow repository conventions when present. Avoid vague titles, hype, and unconfirmed root-cause claims.

## Default body

- Keep it concise. Use bullet points for explanatory text and numbered steps for reproduction. Add headings only when they improve scanning; do not fill a template with empty sections.
- Lead with the actual problem and its user or system impact. Give enough context to understand why it matters without retelling the investigation.
- For bugs, make the behavior mismatch clear without requiring separate expected/actual sections. Include a minimal reproduction when useful to resume or verify the work; include environment details only when they affect the problem.
- Keep reproduction separate from debugging history: steps must trigger or demonstrate the problem, not replay how it was diagnosed. If reproduction is intermittent or unknown, say so.
- Include a short, decisive error excerpt or code snippet rather than a log dump. Link or attach bulky evidence when needed, with secrets and personal data removed.
- Name relevant repository paths, symbols, and entry points so the next session knows where to start. Prefer these over brittle line numbers; add verified links when useful.
- Prefer focused, language-tagged code snippets, sample inputs/outputs, or Mermaid fenced diagrams when they explain the problem more clearly than prose. Useful context matters more than visual formatting; do not force diagrams into simple issues.
- Omit generic validation sections, test-run reports, and statements such as "I ran tests." A specific failing test or experiment belongs only when it demonstrates the problem or materially distinguishes possible causes.
- Mention known workarounds, regression boundaries, or related issues when verified and useful. Do not claim a regression without evidence of an earlier working baseline.
- Preserve agreed direction, constraints, and non-obvious rationale. Distinguish decisions from tentative proposals; do not make the next session rediscover or relitigate settled choices.
- State what remains unresolved and the next concrete action when known. Identify blockers or prerequisites that affect resumption; do not invent an implementation plan to fill space.
- Keep the issue understandable on its own. Avoid references such as "as discussed above" or dependencies on private chat history. If a local artifact is essential, identify it and its portability limits.

## Feature requests

- State the unmet need, who is affected, and the desired observable outcome.
- Include a concrete usage example or concise acceptance criteria when they clarify success.
- Do not assume a particular implementation is required. Include constraints and alternatives only when they affect the decision.

## Visual and performance evidence

- For visual problems, use a comparison table with uploaded images or videos when both states are available. Label columns accurately, such as "Expected/reference" and "Actual" or "Last known working" and "Affected." Do not fabricate an expected screenshot or imply an unverified baseline worked.
- Capture comparable states under the same relevant conditions. Use actual accessible asset URLs; embed images and link videos when inline playback is unsupported.
- For performance comparisons, always use a table with metric names, units, baseline and affected/candidate values, and a delta when useful. Identify the compared versions or revisions and essential measurement conditions.
- Use a verified relevant baseline, not an arbitrary earlier debugging attempt. If only one measurement exists, report it as an observation, not a measured regression.
- Tell the user outside the proposed issue body when desired attachments or comparison evidence are unavailable. Do not publish placeholders or unsupported claims.

## Exceptional issues

- Complex, high-impact, or cross-system problems may warrant a longer technical explanation with context, diagrams, and focused evidence.
- Organize around the problem, mechanism, impact, and remaining uncertainty—not a diary of the investigation.
- Before handing off the draft, remove anything that does not help the reader understand, reproduce, assess, or resolve the issue.
