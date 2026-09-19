---
name: score-vendor
description: Grade a named AI security vendor/product, as a standalone tool, against the full OWASP Agentic (ASI01–ASI10)/LLM Top 10 and MITRE ATLAS taxonomy used in the "Enterprise Strategy & Reference Architecture — Securing Autonomous Agents & NHI" strategy document — producing a per-MAESTRO-layer coverage map, calling out gaps, and naming which other vendor (already in the client's matrix, e.g. Lakera, CrowdStrike, Okta, Aembit, or a new one) would close each gap. Use this whenever the user names a specific AI security company or product and wants to know how much of the security framework it covers on its own, what's missing, or what else a client would need alongside it — for example "score ForceAI", "how would Lasso's new product fit our matrix", "does XYZ Security cover a gap we have", "I just found this vendor at a conference, is it worth a look", "a client asked me about <vendor>, what do I tell them", or "what would I still need if a client bought <vendor>." Also trigger when the user mentions discovering or being pitched a vendor during enterprise client consulting and wants a coverage/gap assessment before recommending it or a complementary stack. Do NOT use this for general AI security news research (that's the separate daily-briefing routine) or for questions about a vendor already named in the strategy doc that don't ask for a fresh assessment.
---

# Vendor Coverage & Gap Analysis: OWASP / MITRE ATLAS Grading

## Why this exists

Clients don't buy one AI security product and stop — they buy a stack, and the consulting value is in knowing exactly what a given product covers and what it still leaves exposed. The strategy document already organizes the full threat surface into 7 MAESTRO layers, each mapped to specific OWASP ASI/LLM categories and MITRE ATLAS techniques (Section 2), and already names which tools cover which layer today (Section 3). This skill uses that existing taxonomy as the grading rubric for a new vendor — not "does this deserve a line in my document," but "graded as a standalone product against the full framework, where does it actually reach, where doesn't it, and what already-known (or new) vendor closes each gap."

The output is a client-ready coverage map, not a single verdict. A vendor scoring well on 2 of 7 layers and poorly elsewhere is a completely normal, useful result — most point products aren't supposed to cover everything, and telling a client "this handles your MCP tooling risk but you still need something for memory poisoning and runtime execution" is the actual deliverable, not a disappointment to soften.

## Inputs

- The vendor or product name (required) — e.g. "Lasso" or "ForceAI."
- Whatever context the user already has: a URL, how they encountered it, a specific claim to check, or a client's specific concern the vendor is being pitched against. Use it, but don't require it — go research if it's missing.

## Step 1: Read the current master fresh

Never assume you know the current file ID or content from an earlier session — both drift as revisions get approved.

1. `mcp__Google_Drive__search_files` with `title = 'Enterprise Strategy & Reference Architecture — Securing Autonomous Agents & NHI'`. If more than one result comes back, or none, stop and ask the user — don't guess. Never use a file whose title contains "Archived."
2. `mcp__Google_Drive__read_file_content` on that file ID. Pull both **Section 2** (the MAESTRO → OWASP ASI/LLM → MITRE ATLAS mapping table — this is the grading rubric itself) and **Section 3** (which vendor is already named against each layer today — this is your source of gap-filling recommendations before you look anywhere else).

## Step 2: Research the vendor

Use WebSearch. The daily briefing routine for this same document has run into fabricated figures and stale claims recirculating through aggregator summaries — apply the same discipline: verify anything specific (a stat, a customer name, an integration claim) before repeating it, and mark it "unverified" rather than dropping a load-bearing claim you can't confirm.

Find out, specifically:
- **What it actually does, mechanically** — the real technical mechanism, not the tagline. You'll map this against each of the 7 layers in Step 3, so you need enough detail to judge, layer by layer, whether it genuinely addresses that layer's threats or just adjacent marketing language.
- **How it enforces, per capability** — for each thing it claims to do, is that a deterministic, non-bypassable control (sandboxing, signed identity, capability tokens, egress control) or an inference-based judgment (a classifier or model scoring another model's output)? The document's own Deterministic Enforcement Axiom treats the latter as weaker coverage even when it technically addresses the layer — note this as a caveat on each row, not a separate score.
- **Evidence of real deployment** — named customers, case studies, independent audits, incident-response engagements, versus launch-announcement-only claims.
- **Anything that cuts against it** — a CVE in the vendor's own product, credible criticism, an incident involving the vendor itself.

## Step 3: Grade coverage, layer by layer, against Section 2's own taxonomy

Use the exact ASI/LLM/ATLAS labels Section 2 already assigns to each MAESTRO layer — this keeps every vendor you ever score in this skill comparable against the same fixed rubric, rather than a new one invented per vendor:

| # | MAESTRO Layer | OWASP (from Section 2) | MITRE ATLAS (from Section 2) |
|---|---|---|---|
| 1 | Model (Inbound & Outbound) | ASI04; LLM05/LLM10 | AML.T0010; AML.T0031 |
| 2 | Agent (Planning) & State | ASI01; ASI10 | AML.T0040; AML.T0051 |
| 3 | Environment & Memory | ASI06; LLM06 | AML.T0035; AML.T0031 |
| 4 | Security & Governance | ASI09; LLM07 | AML.T0043; AML.T0029 |
| 5 | Tools & Skills (MCP) | ASI02; ASI03 | AML.T0053; AML.T0048 |
| 6 | Runtime Execution | ASI05; LLM08 | AML.T0047; AML.T0042 |
| 7 | Operations & Orchestration | ASI07; ASI08 | AML.T0015; AML.T0037 |

(Pull these fresh from the doc in Step 1 rather than trusting this table verbatim — Section 2 is the source of truth and may have changed since this skill was written.)

For each of the 7 layers, grade the vendor **as a standalone product** (assume the client has nothing else in place yet):

- **Full** — directly and substantively addresses the named ASI/LLM categories and ATLAS techniques for this layer, with a real technical mechanism you can point to.
- **Partial** — touches the layer but only addresses part of it, or addresses it through a weaker mechanism (e.g. a classifier judgment where the layer really calls for a deterministic control), or the claim is unverified/marketing-only.
- **None** — the vendor doesn't claim or plausibly reach this layer at all.

Write one sentence of evidence per layer — tie it to something specific from Step 2, not a restatement of the vendor's own category label.

## Step 4: For every Partial or None layer, name what closes the gap

This is the step that makes the output useful to a client, not just a report card.

1. **Check Section 3 first.** For that layer's row, what's already named there (Brownfield Enterprise Leverage column and Frontier AI Security Tooling column)? If something already in the client's matrix covers it, name it directly — e.g. "Row 4 gap → NeMo Guardrails / Lakera Guard already covers this in your matrix."
2. **If Section 3's row is itself thin or flags an unmet gap** (the document sometimes says so explicitly, e.g. the Model-layer safety-training-attestation gap), say that plainly — this is a real market gap, not something to paper over with an invented recommendation.
3. **Only reach for a vendor not yet in the matrix if neither of the above applies** — and when you do, hold it to the same validation bar as Step 2 (real deployment evidence, not just a name you recall).

## Step 5: Present the coverage report in chat

```
## [Vendor Name] — OWASP / MITRE ATLAS Coverage & Gap Analysis

**What it does:** [1-2 sentence plain-language technical summary]

| Layer | Coverage | Evidence | Enforcement note |
|---|---|---|---|
| 1. Model | Full/Partial/None | ... | deterministic / inference-based |
| 2. Agent & State | ... | ... | ... |
| 3. Environment & Memory | ... | ... | ... |
| 4. Security & Governance | ... | ... | ... |
| 5. Tools & Skills (MCP) | ... | ... | ... |
| 6. Runtime Execution | ... | ... | ... |
| 7. Operations & Orchestration | ... | ... | ... |

**Overall:** Full coverage on [N] of 7 layers, partial on [N], none on [N].

**Gaps and what closes them:**
- Layer [X] ([Partial/None]) → [existing matrix vendor] already covers this / [market gap, no good option yet] / [new vendor recommendation, with validation caveat]
- ...

**Bottom line for the client:** [one direct paragraph — what this product is actually good for, what stack it needs alongside it to reach comprehensive coverage, and any enforcement-strength caveats worth flagging even on the layers it does cover.]
```

Be as direct as the daily briefing already is — a vendor covering one layer via a fast classifier is genuinely useful for that layer, but say plainly if that coverage is weaker in kind than the deterministic controls named elsewhere in the matrix, so the client doesn't over-rely on it.

## Step 6: Only if warranted, queue a strategy-doc update

The coverage report is the deliverable most of the time — don't force a document edit onto every scoring. Only continue if a **Full**-rated layer genuinely deserves to be named in Section 3 (it's validated enough to sit alongside what's already there, and either fills a currently-thin row or is a clearly better option than what's named):

1. `mcp__Google_Drive__search_files` with `title = 'Strategy Doc — Pending Proposed Changes'`. Read its full content first if one exists — it may hold other unapproved items from the daily briefing routine or other scorings, and none of that content gets touched or lost.
2. Add the new item into the relevant Section 3 row using the exact conventions already in use: wrap it in `<span style="color:#B8860B"><i>[Proposed <today's date>] ...</i></span>`, written in the same voice as the surrounding text, with sources cited inline. Add a matching dated bullet under "Revision Notes" at the bottom, under a `[Proposed <date>, pending review — not yet approved]` heading (create it if none exists for today, or add to it if one does).
3. Never edit or remove any existing content, approved or already-proposed — only add.
4. If a pending doc already existed, `mcp__Google_Drive__trash_file` the old version, then `mcp__Google_Drive__create_file` the merged replacement (`contentMimeType: "text/html"`, title exactly `Strategy Doc — Pending Proposed Changes`). If none existed, create it fresh. Never touch the approved master directly — promotion only happens on the user's explicit approval.
5. Tell the user plainly what was added and link the pending doc.

## What this skill does not do

No email send and no GitHub commit — that's the daily-briefing routine's job. This skill is chat output plus, occasionally, a Google Drive edit to the pending-changes doc. It never promotes that doc into the master itself; that stays a separate, explicit, user-initiated action.
