---
name: pr-writing
description: Use when writing or editing a pull request title or body. Produces concise, evidence-backed PR descriptions centered on the final aggregate change, with diagrams, code snippets, visual comparisons, and benchmark tables where relevant.
---

# Writing a PR title and body

## Scope and evidence

- Inspect the complete PR diff against its target branch and enough surrounding code to explain the result accurately.
- Describe what the final aggregate squash-merge commit delivers, not the sequence of commits or iterations that produced it.
- Omit intermediate refactors, abandoned approaches, review-fix history, and PR-size reductions. A change from +6,000 lines to +1,000 lines during development is not part of the story.
- Do not invent behavior, measurements, screenshots, uploads, or code references. If required evidence is unavailable, tell the user outside the proposed PR body and request it rather than publishing placeholders or unsupported claims.
- This skill governs writing, not authorization to publish, upload, commit, push, or modify code. Follow the user's requested scope.

## Title

- Use a short, specific title that names the final outcome or behavior change.
- Follow repository title conventions when present. Avoid vague titles, hype, and implementation chronology.

## Default body

- Keep it concise. No essays, boilerplate, or exhaustive file-by-file inventories.
- Use bullet points for explanatory text. State the problem or motivation only when it helps the reader understand the change.
- Prefer compact Mermaid fenced code blocks and focused code snippets over lengthy prose. Show architecture, control/data flow, internal mechanics, or sample usage when they clarify the change; do not force diagrams into trivial PRs.
- Use language-tagged code fences. Keep snippets faithful to the final code and small enough to scan.
- Use code references when useful: repository paths and symbols, or verified links to relevant lines. Avoid a wall of links.
- Do not include validation sections, test commands, test results, or statements such as "I ran tests." Still perform validation required by the task; simply leave that process reporting out of the PR body. Describe substantive testing features when they are themselves the change, not as a validation report.
- Retain material compatibility, migration, rollout, and risk information when relevant. Concision must not hide consequences.

## Visual changes

- For direct or indirect user-visible visual changes, include a before/after table with uploaded images or videos.
- Never commit PR screenshots or videos to the repository. Use `gh --attach` on PR or comment commands to upload and embed them in the message body instead.
- Capture the target-branch baseline and the PR candidate under comparable conditions: same viewport, state, inputs, and theme where applicable.
- Use actual accessible uploaded asset URLs. Embed images in table cells; link uploaded videos from cells when inline playback is not supported.
- Add rows for distinct affected states when needed. Do not substitute prose for required visual evidence.

| Before — target branch | After — PR |
| --- | --- |
| Baseline image/video | Candidate image/video |

- The table above illustrates structure only; replace its cells with real media before publishing.

## Benchmarks

- Whenever reporting benchmarks, use a before/after table: baseline measured from the target branch, candidate measured from the PR.
- Compare the same workload and measurement method under comparable conditions. Identify revisions and essential environment details briefly so the numbers are interpretable.
- Include metric names, units, and baseline/candidate values; include a delta when helpful and make clear whether higher or lower is better.
- Report uncertainty or limitations that materially affect interpretation. Do not present estimates as measurements or use an earlier PR iteration as the baseline.

## Exceptional changes

- For genuinely impressive, difficult, high-risk, or wide-scope changes, a longer technical-blog-style body can be appropriate.
- Explain context, constraints, the final design, and its consequences as a coherent story. Use brief bullet-point commentary supported by diagrams, code, before/after comparisons, and media.
- Earn the extra length with technical substance. The same rules still apply: final aggregate change only, no development diary, no test-run reporting, and no unsupported claims.
