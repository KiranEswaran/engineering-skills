# Capabilities Gap Analysis: claude-engineering-skills vs cursor-engineering-rules

**Date:** December 28, 2025  
**Repository:** `dp-claude-engineering-skills` (renamed from `kb`)

## Executive Summary

While `claude-engineering-skills` covers most core engineering domains, it's missing several **operational capabilities** and **workflow patterns** that make `cursor-engineering-rules` more powerful for AI agents.

---

## Missing Capabilities

### 1. 🔴 **Workflow Commands System** (CRITICAL)

**Status:** Completely missing

**What cursor-engineering-rules has:**
- `/init` - Initialize task, detect complexity
- `/plan` - Design solution, document approach
- `/creative` - Explore design options
- `/qa` - Validate environment (dependencies, config, build tools)
- `/build` - Implement approved plan
- `/review` - Verify implementation
- `/self-review` - Comprehensive local PR review (6-phase structured review)
- `/quick-review` - Fast critical issues check
- `/check-progress` - Review work progress, propose commit message
- `/archive` - Document lessons learned

**Why it matters:**
- **Structured workflow** - Forces proper planning before implementation
- **Quality gates** - QA validation prevents implementation failures
- **Review automation** - `/self-review` and `/quick-review` provide automated code review
- **Progress tracking** - `/check-progress` helps track work without committing
- **Audit compliance** - Commands integrate with audit requirements

**Impact:** Without these commands, Claude agents lack structured workflow guidance and automated review capabilities.

**Recommendation:** 
- Add workflow command patterns as reference examples in `agent-workflow/references/workflow-commands.md`
- Document how to trigger similar behaviors in Claude Code (even if not exact `/command` syntax)
- Include command templates that can be adapted for Claude's interaction model

---

### 2. 🟡 **Context Management Templates** (HIGH VALUE)

**Status:** Missing

**What cursor-engineering-rules has:**
- `tasks.md.template` - Task tracking and planning
- `active-context.md.template` - Current focus documentation
- `progress.md.template` - Implementation status tracking
- `project-brief.md.template` - Project overview
- `creative-template.md.template` - Design decision documentation
- `reflect-template.md.template` - Post-implementation review

**Why it matters:**
- **Persistent context** - Helps maintain context across sessions
- **Task organization** - Structured way to track complex work
- **Knowledge capture** - Templates ensure important information is documented
- **Audit trail** - Provides verifiable documentation of work

**Impact:** Without templates, agents may not maintain context effectively or document decisions properly.

**Recommendation:**
- Add templates to `agent-workflow/references/context-templates.md`
- Include examples of how to use each template
- Document when to use each template type

---

### 3. 🟡 **MCP Server Implementation** (MEDIUM VALUE)

**Status:** Missing

**What cursor-engineering-rules has:**
- `mcp/cursor-rules-mcp/` - TypeScript MCP server
- On-demand rule loading via tool calls
- Just-in-time rule loading
- Rule fetching by category/topic

**Why it matters:**
- **Dynamic loading** - Load only needed rules, not all rules
- **Tool integration** - Can be called programmatically
- **Efficiency** - Reduces context token usage

**Impact:** Without MCP server, all skills load at once (if configured), increasing token usage.

**Recommendation:**
- Consider creating MCP server for Claude Skills (if MCP supports Skills API)
- Document MCP integration patterns in `mcp-development/references/`

---

### 4. 🟡 **Setup & Maintenance Scripts** (MEDIUM VALUE)

**Status:** Missing

**What cursor-engineering-rules has:**
- `setup-workspace.sh` - Bootstrap workspace with rules
- `setup-all-repos.sh` - Setup multiple repos
- `scripts/cursor-maintenance.sh` - Clean cache, logs, temp files

**Why it matters:**
- **Onboarding** - Makes it easy to adopt the skills
- **Maintenance** - Helps keep environments clean
- **Automation** - Reduces manual setup work

**Impact:** Without scripts, users must manually set up skills and maintain environments.

**Recommendation:**
- Add setup scripts for Claude Skills installation
- Create maintenance scripts for Claude Code cache/log cleanup
- Document in README

---

### 5. 🟢 **Examples Directory** (LOW VALUE)

**Status:** Missing

**What cursor-engineering-rules has:**
- `.cursorrules-example` - Example Cursor configuration
- `examples/mcp/` - MCP server configuration examples

**Why it matters:**
- **Quick start** - Users can copy examples to get started
- **Best practices** - Shows recommended configurations

**Impact:** Without examples, users must create configurations from scratch.

**Recommendation:**
- Add example Claude Skills configurations
- Include example `.claude/skills/` directory structure

---

## Content Coverage Gaps

### 6. 🟡 **Git Content Expansion** (PARTIAL)

**Status:** ~86% covered (748 lines vs 872 lines)

**Missing from `core-engineering/references/git-*.md`:**
- Some advanced reflog patterns
- Git worktrees documentation (Phantom tool integration)
- Additional pre-commit hook examples

**Recommendation:** Review `cursor-engineering-rules/rules/130-git.mdc` and ensure all patterns are covered.

---

### 7. 🟡 **Configuration Management** (PARTIAL)

**Status:** Partial coverage

**Missing from `infrastructure-iac/references/configuration.md`:**
- Configuration precedence hierarchy details
- Environment variable type conversion patterns
- Boolean value handling (`1` for true)
- Configuration validation examples (Pydantic)
- Secret management patterns (AWS Secrets Manager, Vault, K8s Secrets)

**Recommendation:** Expand configuration.md with missing patterns from `cursor-engineering-rules/rules/110-configuration.mdc`.

---

### 8. 🟡 **Command-Line Utilities** (PARTIAL)

**Status:** Partial coverage

**Missing from `bash-shell-scripting/references/shell-utilities.md`:**
- Documentation ingestion tool selection matrix
- Progressive escalation strategy (curl → lynx → Playwright → Context7 → VLM → OCR)
- Advanced `fd` and `fzf` usage examples
- Documentation retrieval systems (RAG-first approach)
- Anti-patterns (what NOT to do)

**Recommendation:** Expand shell-utilities.md with documentation ingestion patterns from `cursor-engineering-rules/rules/120-utilities.mdc`.

---

### 9. 🟢 **Makefile Patterns** (VERIFY)

**Status:** Exists (`bash-shell-scripting/references/makefile-patterns.md`)

**Action:** Verify completeness against `cursor-engineering-rules/rules/150-makefile.mdc`.

---

### 10. 🟢 **Open Source Patterns** (VERIFY)

**Status:** Exists (`documentation-standards/references/open-source.md`)

**Action:** Verify completeness against `cursor-engineering-rules/rules/820-open-source.mdc`.

---

## Structural Differences

### Commands vs Skills

**cursor-engineering-rules:**
- Uses **commands** (`/plan`, `/build`, etc.) for explicit workflow control
- Commands trigger specific behaviors and phase transitions
- Commands are Cursor IDE-specific

**claude-engineering-skills:**
- Uses **skills** that auto-trigger based on content
- No explicit command system (Claude Skills don't support `/command` syntax)
- Skills are Claude-specific

**Gap:** Claude Skills lack explicit workflow control mechanisms. Agents must infer workflow phases from context.

**Recommendation:** 
- Document workflow patterns in `agent-workflow/SKILL.md`
- Include examples of how to trigger workflow phases through natural language
- Consider adding workflow state tracking patterns

---

## Priority Action Items

### 🔴 High Priority

1. **Add Workflow Command Patterns** (as references)
   - Document `/init`, `/plan`, `/build`, `/review` patterns
   - Show how to trigger similar behaviors in Claude Code
   - Add to `agent-workflow/references/workflow-commands.md`

2. **Add Context Management Templates**
   - Copy templates from cursor-engineering-rules
   - Adapt for Claude Skills format
   - Add to `agent-workflow/references/context-templates.md`

3. **Expand Configuration Management**
   - Add precedence hierarchy
   - Add environment variable patterns
   - Add secret management patterns

### 🟡 Medium Priority

4. **Expand Command-Line Utilities**
   - Add documentation ingestion patterns
   - Add tool selection matrix
   - Add progressive escalation strategy

5. **Add Setup Scripts**
   - Create `setup-claude-skills.sh`
   - Create maintenance scripts
   - Document in README

6. **Verify Makefile/Open Source Coverage**
   - Compare against source rules
   - Fill any gaps

### 🟢 Low Priority

7. **Add Examples Directory**
   - Example Claude Skills configurations
   - Example `.claude/skills/` structure

8. **Consider MCP Server**
   - Evaluate if MCP supports Skills API
   - Document integration patterns if applicable

---

## Summary

**Key Missing Capabilities:**

1. **Workflow Commands System** - Structured workflow control (CRITICAL)
2. **Context Management Templates** - Persistent context tracking (HIGH VALUE)
3. **Configuration Management Expansion** - Missing precedence, validation, secrets (MEDIUM)
4. **Command-Line Utilities Expansion** - Missing documentation ingestion patterns (MEDIUM)
5. **Setup Scripts** - Missing automation for adoption (MEDIUM)

**Content Coverage:** ~90% complete (most domains covered, some need expansion)

**Recommendation:** Focus on adding workflow command patterns and context templates first, as these provide the most value for AI agent workflows.
