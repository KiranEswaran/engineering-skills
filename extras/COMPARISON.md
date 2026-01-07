# Comparison: cursor-engineering-rules vs engineering-skills

This document identifies what's missing in `dp-engineering-skills` compared to `cursor-engineering-rules` in the context of Claude Agent Skills.

## Missing Skills

### 1. **PostgreSQL/Database Skill** ⚠️ CRITICAL

**Status:** Completely missing

**Source:** `cursor-engineering-rules/rules/470-postgresql.mdc` (982 lines)

**What's Missing:**
- Database naming conventions (snake_case, plural table names)
- Schema design patterns (indexes, foreign keys, constraints)
- Migration best practices (reversible, versioned, tested)
- Performance optimization (query planning, indexing strategies)
- Security patterns (RLS, parameterized queries, least privilege)
- Backup and recovery procedures
- PostgreSQL-specific features (JSONB, arrays, full-text search)

**Recommendation:** Create `database-postgresql/` skill with:
- `SKILL.md` - Core PostgreSQL principles and patterns
- `references/schema-design.md` - Naming conventions, table design
- `references/migrations.md` - Migration patterns and best practices
- `references/performance.md` - Indexing, query optimization
- `references/security.md` - RLS, parameterized queries

---

### 2. **Git Skill** ⚠️ PARTIALLY MISSING

**Status:** Partial coverage (132 lines vs 872 lines in source)

**Current:** `core-engineering/references/git-workflow.md` (basic commit standards)

**What's Missing:**
- **Git Fundamentals:**
  - Refs (local branches, remote-tracking branches, tags, HEAD)
  - Three-tier model (remote → remote-tracking → local)
  - Reflog (Git's safety net, recovery patterns)
- **Modern Git Commands:**
  - `git switch` vs `git checkout`
  - `git restore` vs `git checkout -- file`
  - `git mv` vs `mv`
- **Advanced Patterns:**
  - Git worktrees (parallel development)
  - Fetch vs pull patterns
  - Branch pruning (`--prune`)
  - Fast-forward only merges (`--ff-only`)
- **Pre-commit Hooks:**
  - Pre-commit framework setup
  - Hook installation and updates

**Recommendation:** Expand `core-engineering/references/git-workflow.md` or create dedicated `git-version-control/` skill with:
- `SKILL.md` - Git fundamentals and modern commands
- `references/reflog-recovery.md` - Reflog patterns and recovery
- `references/branch-workflow.md` - Branch management and workflows
- `references/pre-commit-hooks.md` - Pre-commit setup and patterns

---

### 3. **Configuration Management Skill** ⚠️ PARTIALLY MISSING

**Status:** Partial coverage in `infrastructure-iac/references/configuration.md`

**Source:** `cursor-engineering-rules/rules/110-configuration.mdc` (full rule)

**What's Missing:**
- **Configuration Precedence:**
  - System → User → Local → Environment → Command-line hierarchy
  - Precedence examples (Git, SSH patterns)
- **Environment Variable Patterns:**
  - String type conversion (explicit conversion required)
  - Boolean values (`1` for true, anything else false)
  - Required values (fail fast with clear errors)
  - Default values (sensible defaults)
- **Configuration Validation:**
  - Early validation patterns
  - Pydantic/validation examples
- **Secret Management:**
  - AWS Secrets Manager
  - HashiCorp Vault
  - Kubernetes Secrets
- **Configuration in Different Environments:**
  - Development, staging, production patterns

**Recommendation:** Expand `infrastructure-iac/references/configuration.md` or create dedicated `configuration-management/` skill.

---

### 4. **Command-Line Utilities Skill** ⚠️ PARTIALLY MISSING

**Status:** Partial coverage in `bash-shell-scripting/references/shell-utilities.md`

**Source:** `cursor-engineering-rules/rules/120-utilities.mdc` (full rule)

**What's Missing:**
- **Documentation Ingestion Patterns:**
  - Tool selection matrix (curl → lynx → Playwright → Context7 → VLM → OCR)
  - Progressive escalation strategy
  - Rate limiting and caching
- **Advanced Tool Usage:**
  - `fd` (find alternative) - comprehensive examples
  - `fzf` (fuzzy finder) - shell integration, git workflows
  - `ripgrep` - advanced patterns
  - `httpie` - API testing examples
- **Documentation Retrieval Systems:**
  - RAG-first approach for official docs
  - Chunking and indexing strategies
- **Anti-patterns:**
  - What NOT to do (defaulting to browser, pasting entire pages, aggressive scraping)

**Recommendation:** Expand `bash-shell-scripting/references/shell-utilities.md` or create dedicated `command-line-utilities/` skill with documentation ingestion focus.

---

## Missing Content Areas

### 5. **Makefile Patterns** ⚠️ PARTIALLY MISSING

**Status:** May be partially covered in `bash-shell-scripting`

**Source:** `cursor-engineering-rules/rules/150-makefile.mdc`

**Recommendation:** Verify coverage in `bash-shell-scripting/references/makefile-patterns.md` or add dedicated content.

---

### 6. **Open Source Patterns** ⚠️ PARTIALLY MISSING

**Status:** May be partially covered in `documentation-standards`

**Source:** `cursor-engineering-rules/rules/820-open-source.mdc`

**Recommendation:** Verify coverage in `documentation-standards/references/open-source.md` or add dedicated content.

---

## Not Applicable to Claude Skills

### 7. **Commands Directory** ℹ️ NOT APPLICABLE

**Status:** Unique to Cursor IDE, not applicable to Claude Skills

**Content:** `/init`, `/plan`, `/build`, `/review`, `/self-review`, `/quick-review`, `/check-progress`, `/qa`, `/creative`, `/archive`

**Note:** These are Cursor-specific workflow commands. Claude Skills don't have command triggers in the same way.

---

### 8. **Templates Directory** ℹ️ POTENTIALLY USEFUL

**Status:** Context management templates, could be adapted

**Content:**
- `active-context.md.template`
- `creative-template.md.template`
- `progress.md.template`
- `project-brief.md.template`
- `reflect-template.md.template`
- `tasks.md.template`

**Recommendation:** These could be included as reference examples in `agent-workflow/references/context-management.md` for users who want to implement similar patterns.

---

## Coverage Summary

| Category | Source Rules | Skills Coverage | Status |
|----------|-------------|----------------|--------|
| **Core Engineering** | 100-core.mdc | ✅ core-engineering | Complete |
| **Workflow** | 010-workflow.mdc | ✅ agent-workflow | Complete |
| **Agent Audit** | 020-agent-audit.mdc | ✅ agent-workflow | Complete |
| **Git** | 130-git.mdc | ⚠️ Partial (15%) | Needs expansion |
| **Configuration** | 110-configuration.mdc | ⚠️ Partial | Needs expansion |
| **Utilities** | 120-utilities.mdc | ⚠️ Partial | Needs expansion |
| **PostgreSQL** | 470-postgresql.mdc | ❌ Missing | **Create new skill** |
| **Python** | 200-python.mdc | ✅ python-development | Complete |
| **TypeScript/JS** | 240-typescript.mdc, 230-javascript.mdc | ✅ typescript-javascript | Complete |
| **Go/Rust** | 210-go.mdc, 220-rust.mdc | ✅ go-rust-systems | Complete |
| **Bash** | 140-bash.mdc | ✅ bash-shell-scripting | Complete |
| **Terraform** | 180-terraform.mdc | ✅ infrastructure-iac | Complete |
| **Docker** | 440-docker.mdc | ✅ infrastructure-iac | Complete |
| **Kubernetes** | 450-kubernetes.mdc | ✅ kubernetes-containers | Complete |
| **Helm** | 460-helm.mdc | ✅ kubernetes-containers | Complete |
| **AWS** | 410-aws.mdc | ✅ cloud-platforms | Complete |
| **Azure** | 430-azure.mdc | ✅ cloud-platforms | Complete |
| **GCP** | 420-gcp.mdc | ✅ cloud-platforms | Complete |
| **Cloudflare** | 400-cloudflare.mdc | ✅ cloud-platforms | Complete |
| **GitHub Actions** | 160-github-actions.mdc | ✅ cicd-github-actions | Complete |
| **Security** | 310-security.mdc | ✅ security-testing | Complete |
| **Testing** | 300-testing.mdc | ✅ security-testing | Complete |
| **API Design** | 320-api-design.mdc | ✅ security-testing | Complete |
| **Observability** | 330-observability.mdc | ✅ security-testing | Complete |
| **Markdown** | 800-markdown.mdc | ✅ documentation-standards | Complete |
| **Documentation** | 810-documentation.mdc | ✅ documentation-standards | Complete |
| **MCP Servers** | 510-mcp-servers.mdc | ✅ mcp-development | Complete |
| **AI/ML** | 500-ai-ml.mdc | ✅ mcp-development | Complete |
| **Makefile** | 150-makefile.mdc | ⚠️ Partial | Verify coverage |
| **Open Source** | 820-open-source.mdc | ⚠️ Partial | Verify coverage |
| **CloudFormation** | 170-cloudformation.mdc | ✅ infrastructure-iac | Complete |
| **Ansible** | 190-ansible.mdc | ✅ infrastructure-iac | Complete |
| **CLI** | 250-cli.mdc | ✅ bash-shell-scripting | Complete |

---

## Priority Recommendations

### 🔴 High Priority (Critical Missing Content)

1. **Create `database-postgresql/` skill** - 982 lines of PostgreSQL best practices completely missing
2. **Expand Git coverage** - Only 15% of Git content covered (740 lines missing)
3. **Expand Configuration Management** - Critical patterns missing (precedence, validation, secret management)

### 🟡 Medium Priority (Partial Coverage)

4. **Expand Command-Line Utilities** - Documentation ingestion patterns missing
5. **Verify Makefile coverage** - Ensure all patterns are included
6. **Verify Open Source coverage** - Ensure all patterns are included

### 🟢 Low Priority (Nice to Have)

7. **Add Templates as References** - Include context management templates as examples in `agent-workflow/references/`

---

## Next Steps

1. **Create PostgreSQL skill** - Highest priority, completely missing
2. **Expand Git content** - Add reflog, modern commands, advanced patterns
3. **Expand Configuration content** - Add precedence, validation, secret management
4. **Expand Utilities content** - Add documentation ingestion patterns
5. **Verify Makefile/Open Source** - Ensure complete coverage
