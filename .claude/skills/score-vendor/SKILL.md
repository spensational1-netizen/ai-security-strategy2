---
name: score-vendor
description: Score a new or unfamiliar AI security vendor/product against the "Enterprise Strategy & Reference Architecture — Securing Autonomous Agents & NHI" strategy document's own criteria and tooling matrix. Use this whenever the user names a specific AI security company or product and asks how it stacks up, fits, compares, or scores against their strategy, guidelines, framework, or matrix — for example "score ForceAI", "how would Lasso's new product fit our matrix", "does XYZ Security cover a gap we have", "I just found this vendor at a conference, is it worth a look", or "a client asked me about <vendor>, what do I tell them." Also trigger when the user mentions discovering or being pitched a vendor during enterprise client consulting and wants an assessment before recommending it. Do NOT use this for general AI security news research (that's the separate daily-briefing routine) or for questions about a vendor already named in the strategy doc that don't ask for a fresh score.
---

# Vendor Scorecard: AI Security Tooling Assessment

## Why this exists

The strategy document already encodes a specific, opinionated point of view: guardrails aren't a security boundary, brownfield leverage beats point solutions, and nothing gets named without evidence of real-world validation. When a new vendor comes up in a client conversation, the useful question isn't "is this a good product" in the abstract — it's "does this hold up against the same bar everything else in this document had to clear." This skill applies that existing bar to a new name, rather than inventing a generic vendor-eval rubric from scratch.

Most vendor lookups should end in "interesting, not yet actionable." Only a minority will be solid enough to propose as a change to the document. Resist the pull to make every lookup feel productive by queuing something — a scorecard that concludes "no action" is a successful, complete use of this skill.

## Inputs

- The vendor or product name (required) — e.g. "ForceAI."
- Whatever context the user already has: a URL, how they encountered it (client mentioned it, conference booth, a cold pitch), a specific claim they want checked, or which MAESTRO layer they suspect it targets. Use it, but don't require it — go research if it's missing.

## Step 1: Read the current master fresh

Never assume you know the current file ID or the document's current content from an earlier session — both drift over time as revisions get approved.

1. `mcp__Google_Drive__search_files` with `title = 'Enterprise Strategy & Reference Architecture — Securing Autonomous Agents & NHI'`. If more than one result comes back, or none, stop and ask the user — don't guess. Never use a file whose title contains "Archived."
2. `mcp__Google_Drive__read_file_content` on that file ID to get the full current text. You need Section 3 (the layer-by-layer tooling matrix) in front of you before you can score anything against it — the whole point is comparing the new vendor to what's already named there, row by row.

## Step 2: Research the vendor

Use WebSearch. The daily AI-security-briefing routine for this same document has already run into fabricated regulatory figures and stale claims recirculating through aggregator summaries — apply the same discipline here: verify anything specific (a stat, a customer name, a claimed integration, a funding figure) against the vendor's own site or an independent outlet before repeating it, and say "unverified" rather than dropping a claim you can't confirm but that seems load-bearing.

Look for:
- **What it actually does, mechanically.** Not the tagline — the actual technical approach. This is what determines which MAESTRO layer(s) it addresses: Model (Inbound/Outbound) / Agent & State / Environment & Memory / Security & Governance / Tools & Skills (MCP) / Runtime Execution / Operations & Orchestration. A vendor can span more than one row.
- **Evidence of real deployment.** Named customers, case studies, incident-response engagements, independent audits or pen-test results — versus a site that's all launch-announcement and no track record.
- **Anything that cuts against it.** A CVE in the vendor's own product, credible criticism, a security incident involving the vendor itself, or a claim that doesn't survive a second source.
- **How it relates to what Section 3 already names for that row.** Complementary to an existing tool (e.g. another NHI-governance option alongside Aembit/Oasis Security), a direct competitor, or something genuinely new that no current row covers.

## Step 3: Score against the document's own criteria

Don't invent a fresh rubric — use the standards the document already applies to everything else in it. Rate each as **Strong / Conditional / Weak / Not Applicable** with one sentence of justification tied to something you actually found in Step 2, not a generic statement.

1. **Deterministic Enforcement Axiom.** The document's core position is that natural-language/LLM-based guardrails are not a security boundary — only deterministic, non-bypassable controls are (sandboxing, signed identity, capability tokens, network egress control). Does this vendor's mechanism actually enforce anything deterministically, or is its "security" a model judging model output?
2. **Brownfield Leverage vs. Point Solution.** The document explicitly prefers extending an enterprise's existing platform investment (identity, EDR, SIEM, DLP — see the "Brownfield Enterprise Leverage" column) over adding a new standalone tool to operate. Which is this?
3. **Real-World Validation.** Score as Validated / Emerging / Unvalidated. The document already treats several named tools this way — e.g. it names CrowdStrike Falcon Guardian and flags it "independently validate before relying on it exclusively" rather than treating a launch announcement as proof. Hold this vendor to the same standard: a slick site with no customers is Unvalidated regardless of how good the pitch is.
4. **Gap Coverage.** Does this fill a row that's currently thin (e.g. Model-layer safety-training attestation, which the document itself flags as an unmet tooling need) or does it compete in a row that's already well covered? Filling a real gap is worth more than being a slightly-better version of something already named.
5. **Regulatory/Compliance Relevance.** Does it bear on Section 7 — EU AI Act GPAI obligations, or the vendor-disclosure-practices angle the document tracks? Most vendors will be Not Applicable here; that's fine.

## Step 4: Present the scorecard in chat

Use this structure:

```
## [Vendor Name] — Vendor Scorecard

**What it does:** [1-2 sentence plain-language technical summary]
**MAESTRO layer(s):** [row(s)]

| Criterion | Rating | Why |
|---|---|---|
| Deterministic Enforcement | ... | ... |
| Brownfield vs. Point Solution | ... | ... |
| Real-World Validation | ... | ... |
| Gap Coverage | ... | ... |
| Regulatory/Compliance Relevance | ... | ... |

**Bottom line:** [one direct sentence — e.g. "Worth a pilot conversation," "Not yet proven, revisit in 6 months," or "Duplicates existing Row 5 tooling, no action needed."]
```

Be as direct and skeptical here as the daily briefing already is with its own bottom-line calls — a vendor with no case studies and a guardrail-only approach should read as unimpressive, not be softened into false balance.

## Step 5: Decide whether this is material — and act accordingly

Most scorecards stop here. Only continue to queuing a document change when the finding is genuinely material: it fills a row that's currently empty or thin, it's validated enough to sit alongside the tools already named, or it corrects something already in the doc. "Interesting but Unvalidated" or "duplicates existing tooling" is a complete, successful outcome on its own — say so and stop.

If it is material:

1. `mcp__Google_Drive__search_files` with `title = 'Strategy Doc — Pending Proposed Changes'`. If one exists, read its full content first — it may already hold other unapproved items from the daily briefing routine, and none of that content gets touched or lost.
2. Take that content (or, if no pending doc exists yet, the current master's content) as your base, and add the new item into the relevant Section 3 row using the exact conventions already in use in that document: wrap the addition in `<span style="color:#B8860B"><i>[Proposed <today's date>] ...</i></span>`, write it as a precedent/tooling addition in the same voice as the surrounding text, and cite your sources by name and URL inline. Then add a matching dated bullet under the "Revision Notes" section at the bottom, under a `[Proposed <date>, pending review — not yet approved]` heading (create that heading if today is the first proposal since the last approval, or add to it if one already exists for today).
3. Never edit or remove any existing content, approved or already-proposed — only add.
4. If a pending doc already existed, `mcp__Google_Drive__trash_file` the old version, then `mcp__Google_Drive__create_file` the merged replacement (`contentMimeType: "text/html"`, title exactly `Strategy Doc — Pending Proposed Changes`). If none existed, just create it fresh. Never edit the approved master directly — that only happens when the user explicitly approves and asks for promotion.
5. Tell the user plainly what was added and link the pending doc, e.g.: "Queued: added ForceAI as a second Row 2 option alongside Aembit/Oasis Security. Awaiting your review here: [link]."

## What this skill does not do

No email send and no GitHub commit — that's the daily-briefing routine's job, not this one. This skill is chat output plus, occasionally, a Google Drive edit. It also never promotes the pending doc into the master itself; that stays a separate, explicit, user-initiated action, same as it already is for the daily briefing's findings.
