<!-- KIMI-CLAUDE-BRIDGE:CLAUDE_MD:START -->
# CLAUDE.md Instructions (auto-synced by kimi-claude-bridge)

The following instructions are from the Claude Code ecosystem's CLAUDE.md files.
They define system-wide context, conventions, and preferences for this project.

<!-- Source: /Users/chesterjohn/.claude/CLAUDE.md (scope: user) -->
<!-- OMC:START -->
<!-- OMC:VERSION:4.7.9 -->

# oh-my-claudecode - Intelligent Multi-Agent Orchestration

You are running with oh-my-claudecode (OMC), a multi-agent orchestration layer for Claude Code.
Coordinate specialized agents, tools, and skills so work is completed accurately and efficiently.

<operating_principles>
- Delegate specialized work to the most appropriate agent.
- Prefer evidence over assumptions: verify outcomes before final claims.
- Choose the lightest-weight path that preserves quality.
- Consult official docs before implementing with SDKs/frameworks/APIs.
</operating_principles>

<delegation_rules>
Delegate for: multi-file changes, refactors, debugging, reviews, planning, research, verification.
Work directly for: trivial ops, small clarifications, single commands.
Route code to `executor` (use `model=opus` for complex work). Uncertain SDK usage → `document-specialist`.
</delegation_rules>

<model_routing>
`haiku` (quick lookups), `sonnet` (standard), `opus` (architecture, deep analysis).
Direct writes OK for: `~/.claude/**`, `.omc/**`, `.claude/**`, `CLAUDE.md`, `AGENTS.md`.
</model_routing>

<agent_catalog>
Prefix: `oh-my-claudecode:`. See `agents/*.md` for full prompts.

explore (haiku), analyst (opus), planner (opus), architect (opus), debugger (sonnet), executor (sonnet), verifier (sonnet), security-reviewer (sonnet), code-reviewer (opus), test-engineer (sonnet), designer (sonnet), writer (haiku), qa-tester (sonnet), scientist (sonnet), document-specialist (sonnet), git-master (sonnet), code-simplifier (opus), critic (opus)
</agent_catalog>

<tools>
External AI: `/team N:executor "task"`, `omc team N:codex|gemini "..."`, `omc ask <claude|codex|gemini>`, `/ccg`
OMC State: `state_read`, `state_write`, `state_clear`, `state_list_active`, `state_get_status`
Teams: `TeamCreate`, `TeamDelete`, `SendMessage`, `TaskCreate`, `TaskList`, `TaskGet`, `TaskUpdate`
Notepad: `notepad_read`, `notepad_write_priority`, `notepad_write_working`, `notepad_write_manual`
Project Memory: `project_memory_read`, `project_memory_write`, `project_memory_add_note`, `project_memory_add_directive`
Code Intel: LSP (`lsp_hover`, `lsp_goto_definition`, `lsp_find_references`, `lsp_diagnostics`, etc.), AST (`ast_grep_search`, `ast_grep_replace`), `python_repl`
</tools>

<skills>
Invoke via `/oh-my-claudecode:<name>`. Trigger patterns auto-detect keywords.

Workflow: `autopilot`, `ralph`, `ultrawork`, `team`, `ccg`, `ultraqa`, `omc-plan`, `ralplan`, `sciomc`, `external-context`, `deepinit`, `deep-interview`, `ai-slop-cleaner`
Keyword triggers: "autopilot"→autopilot, "ralph"→ralph, "ulw"→ultrawork, "ccg"→ccg, "ralplan"→ralplan, "deep interview"→deep-interview, "deslop"/"anti-slop"/cleanup+slop-smell→ai-slop-cleaner, "deep-analyze"→analysis mode, "tdd"→TDD mode, "deepsearch"→codebase search, "ultrathink"→deep reasoning, "cancelomc"→cancel. Team orchestration is explicit via `/team`.
Utilities: `ask-codex`, `ask-gemini`, `cancel`, `note`, `learner`, `omc-setup`, `mcp-setup`, `hud`, `omc-doctor`, `omc-help`, `trace`, `release`, `project-session-manager`, `skill`, `writer-memory`, `ralph-init`, `configure-notifications`, `learn-about-omc`
</skills>

<team_pipeline>
Stages: `team-plan` → `team-prd` → `team-exec` → `team-verify` → `team-fix` (loop).
Fix loop bounded by max attempts. `team ralph` links both modes.
</team_pipeline>

<verification>
Verify before claiming completion. Size appropriately: small→haiku, standard→sonnet, large/security→opus.
If verification fails, keep iterating.
</verification>

<execution_protocols>
Broad requests: explore first, then plan. 2+ independent tasks in parallel. `run_in_background` for builds/tests.
Keep authoring and review as separate passes: writer pass creates or revises content, reviewer/verifier pass evaluates it later in a separate lane.
Never self-approve in the same active context; use `code-reviewer` or `verifier` for the approval pass.
Before concluding: zero pending tasks, tests passing, verifier evidence collected.
</execution_protocols>

<hooks_and_context>
Hooks inject `<system-reminder>` tags. Key patterns: `hook success: Success` (proceed), `[MAGIC KEYWORD: ...]` (invoke skill), `The boulder never stops` (ralph/ultrawork active).
Persistence: `<remember>` (7 days), `<remember priority>` (permanent).
Kill switches: `DISABLE_OMC`, `OMC_SKIP_HOOKS` (comma-separated).
</hooks_and_context>

<cancellation>
`/oh-my-claudecode:cancel` ends execution modes. Cancel when done+verified or blocked. Don't cancel if work incomplete.
</cancellation>

<worktree_paths>
State: `.omc/state/`, `.omc/state/sessions/{sessionId}/`, `.omc/notepad.md`, `.omc/project-memory.json`, `.omc/plans/`, `.omc/research/`, `.omc/logs/`
</worktree_paths>

## Setup

Say "setup omc" or run `/oh-my-claudecode:omc-setup`.

<!-- OMC:END -->

## gstack

Use gstack skills for this project. For all web browsing and browser automation,
use `/browse` from gstack. Never use `mcp__claude-in-chrome__*` tools.

Available skills: /office-hours, /plan-ceo-review, /plan-eng-review, /plan-design-review,
/design-consultation, /design-shotgun, /design-html, /review, /ship, /land-and-deploy,
/canary, /benchmark, /browse, /open-gstack-browser, /qa, /qa-only, /design-review,
/setup-browser-cookies, /setup-deploy, /setup-gbrain, /sync-gbrain, /retro, /investigate,
/document-release, /document-generate, /codex, /cso, /autoplan, /pair-agent, /careful, /freeze,
/guard, /unfreeze, /gstack-upgrade, /learn.

---

<!-- Source: /Users/chesterjohn/Downloads/kimi-gstack-test/CLAUDE.md (scope: project) -->
## gstack (REQUIRED — global install)

**Before doing ANY work, verify gstack is installed:**

```bash
test -d ~/.claude/skills/gstack/bin && echo "GSTACK_OK" || echo "GSTACK_MISSING"
```

If GSTACK_MISSING: STOP. Do not proceed. Tell the user:

> gstack is required for all AI-assisted work in this repo.
> Install it:
> ```bash
> git clone --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack
> cd ~/.claude/skills/gstack && ./setup --team
> ```
> Then restart your AI coding tool.

Do not skip skills, ignore gstack errors, or work around missing gstack.

Using gstack skills: After install, skills like /qa, /ship, /review, /investigate,
and /browse are available. Use /browse for all web browsing.
Use ~/.claude/skills/gstack/... for gstack file paths (the global path).
<!-- KIMI-CLAUDE-BRIDGE:CLAUDE_MD:END -->





<!-- KIMI-CLAUDE-BRIDGE:AGENTS:START -->
## Claude Code Specialized Agents

The following specialized agents are available from the Claude Code ecosystem.
You can reference their expertise when the user asks for domain-specific help.
Use `kimi --agent-file ~/.kimi/cache/claude-agents/<name>/agent.yaml` to load one directly.

- **audit-budget** (sonnet): Budget and bidding specialist. Audits budget allocation, bidding strategies, learning phase health, audience targeting, and campaign structure across LinkedIn, TikTok, and Microsoft.

- **audit-compliance** (sonnet): Compliance and performance specialist. Audits regulatory compliance, ad policies, privacy requirements, campaign settings, and performance benchmarks across LinkedIn, TikTok, and Microsoft.

- **audit-creative** (sonnet): Creative quality specialist. Audits ad creative across LinkedIn, TikTok, and Microsoft for format diversity, fatigue signals, platform-native content, and spec compliance.

- **audit-google** (sonnet): Google Ads audit specialist. Analyzes conversion tracking, wasted spend, account structure, keywords, Quality Score, ad assets, PMax, bidding, and settings.

- **audit-meta** (sonnet): Meta Ads audit specialist. Analyzes Pixel/CAPI health, EMQ scores, creative diversity and fatigue, account structure, learning phase, audience targeting, and Advantage+ campaigns.

- **audit-tracking** (sonnet): Conversion tracking specialist. Audits pixel installation, server-side tracking, event configuration, and attribution across LinkedIn, TikTok, and Microsoft platforms.

- **copy-writer** (sonnet): Ad copy specialist for paid advertising. Reads campaign-brief.md and brand-profile.json to write platform-compliant headlines, primary text, descriptions, and CTAs. Validates character counts before writing. Appends the copy deck to campaign-brief.md.

- **creative-strategist** (opus): Campaign concept strategist for paid advertising. Reads brand-profile.json and optional audit results to generate structured campaign concepts, messaging pillars, and creative direction for each platform. Produces the strategic sections of campaign-brief.md.

- **format-adapter** (haiku): Ad asset format validator and spec-compliance checker. Reads generation-manifest.json, verifies image dimensions meet platform requirements, checks safe zone compliance, reports missing formats, and writes format-report.md.

- **seo-content** (): Content quality reviewer. Evaluates E-E-A-T signals, readability, content depth, AI citation readiness, and thin content detection.
- **seo-geo** (): GEO and AI search specialist. Analyzes AI crawler accessibility, llms.txt compliance, passage-level citability, brand mention signals, and platform-specific optimization for Google AI Overviews, ChatGPT, Perplexity, and Bing Copilot.
- **seo-performance** (): Performance analyzer. Measures and evaluates Core Web Vitals and page load performance.
- **seo-schema** (): Schema markup expert. Detects, validates, and generates Schema.org structured data in JSON-LD format.
- **seo-sitemap** (): Sitemap architect. Validates XML sitemaps, generates new ones with industry templates, and enforces quality gates for location pages.
- **seo-technical** (): Technical SEO specialist. Analyzes crawlability, indexability, security, URL structure, mobile optimization, Core Web Vitals, and JavaScript rendering.
- **seo-visual** (): Visual analyzer. Captures screenshots, tests mobile rendering, and analyzes above-the-fold content using Playwright.
- **visual-designer** (sonnet): Visual ad creative specialist. Reads campaign-brief.md and brand-profile.json to construct image generation prompts, calls generate_image.py for each platform asset, organizes outputs into ad-assets/ directories, and writes generation-manifest.json for the format-adapter agent.

<!-- KIMI-CLAUDE-BRIDGE:AGENTS:END -->

<!-- KIMI-CLAUDE-BRIDGE:OMC:START -->
## Project Conventions (adapted from oh-my-claudecode)

### Operating Principles
- Delegate specialized work to the most appropriate agent.
- Prefer evidence over assumptions: verify outcomes before final claims.
- Choose the lightest-weight path that preserves quality.
- Consult official docs before implementing with SDKs/frameworks/APIs.

### Delegation Rules
Delegate for: multi-file changes, refactors, debugging, reviews, planning, research, verification.
Work directly for: trivial ops, small clarifications, single commands.
Route code to `executor` (use `model=opus` for complex work). Uncertain SDK usage → `document-specialist`.

### Verification
Verify before claiming completion. Size appropriately: small→haiku, standard→sonnet, large/security→opus.
If verification fails, keep iterating.

### Execution Protocols
Broad requests: explore first, then plan. 2+ independent tasks in parallel. `run_in_background` for builds/tests.
Keep authoring and review as separate passes: writer pass creates or revises content, reviewer/verifier pass evaluates it later in a separate lane.
Never self-approve in the same active context; use `code-reviewer` or `verifier` for the approval pass.
Before concluding: zero pending tasks, tests passing, verifier evidence collected.

### Model Routing
`haiku` (quick lookups), `sonnet` (standard), `opus` (architecture, deep analysis).
Direct writes OK for: `~/.claude/**`, `.omc/**`, `.claude/**`, `CLAUDE.md`, `AGENTS.md`.
<!-- KIMI-CLAUDE-BRIDGE:OMC:END -->
