# Engineering Skills

A collection of Agent Skills for Claude that provide best-practice engineering guidance for code review, generation, and infrastructure management.

## What are Agent Skills?

Agent Skills are modular capabilities that extend Claude's functionality with domain-specific expertise. Each skill packages instructions, workflows, and best practices that Claude uses automatically when relevant to your request.

For more information, see [Anthropic's Agent Skills documentation](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview).

## Available Skills

| Skill | Description |
|-------|-------------|
| **core-engineering** | Core coding standards, code review patterns, and generation guidelines |
| **agent-workflow** | Plan/Implement/Review workflow with audit requirements |
| **python-development** | Python 3.12+ standards, type hints, async patterns, testing |
| **typescript-javascript** | TypeScript/JavaScript patterns, strict typing, ESM |
| **go-rust-systems** | Go and Rust systems programming patterns |
| **bash-shell-scripting** | Bash scripting, CLI design, Makefile patterns |
| **infrastructure-iac** | Terraform, Ansible, Docker, CloudFormation |
| **cloud-platforms** | AWS, Azure, GCP, Cloudflare best practices |
| **kubernetes-containers** | Kubernetes and Helm patterns |
| **cicd-github-actions** | GitHub Actions workflows and CI/CD |
| **security-testing** | OWASP Top 10, testing strategies, API design |
| **documentation-standards** | Markdown, Mermaid diagrams, technical writing |
| **mcp-development** | MCP server patterns and AI/ML integration |
| **database-postgresql** | PostgreSQL schema design, migrations, performance, security |

## Installation

### Claude Code

Clone this repository and Claude Code will automatically discover the skills:

```bash
# Clone to your home directory
git clone https://github.com/YOUR_USERNAME/engineering-skills.git ~/.claude/skills/engineering-skills

# Or clone to a project directory
git clone https://github.com/YOUR_USERNAME/engineering-skills.git .claude/skills/engineering-skills
```

Skills in `~/.claude/skills/` are available globally, while skills in `.claude/skills/` are project-specific.

### Claude.ai

1. Go to **Settings > Features**
2. For each skill you want to use:
   - Zip the skill folder (e.g., `python-development/`)
   - Upload the zip file

Note: Claude.ai skills are individual to each user.

### Claude API

Upload skills using the Skills API:

```bash
# Package the skill
zip -r python-development.skill python-development/

# Upload via API (requires skills-2025-10-02 beta header)
curl -X POST https://api.anthropic.com/v1/skills \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-beta: skills-2025-10-02" \
  -F "file=@python-development.skill"
```

## Skill Structure

Each skill follows the Anthropic Agent Skills specification:

```
skill-name/
├── SKILL.md           # Main instructions with YAML frontmatter
└── references/        # Detailed documentation (loaded on-demand)
    ├── topic-1.md
    └── topic-2.md
```

### Progressive Disclosure

Skills use a three-level loading system:
1. **Metadata** (~100 tokens) - Always loaded, used for triggering
2. **SKILL.md body** (<5k tokens) - Loaded when skill triggers
3. **References** (unlimited) - Loaded only when needed

This ensures efficient context usage while providing comprehensive guidance.

## Usage Examples

Once installed, Claude automatically uses relevant skills based on your requests:

- "Review this Python code for security issues" → Triggers `python-development` and `security-testing`
- "Help me set up a Terraform module for AWS" → Triggers `infrastructure-iac` and `cloud-platforms`
- "Create a GitHub Actions workflow" → Triggers `cicd-github-actions`

## Contributing

Contributions are welcome! Please follow these guidelines:

1. Each skill should have a clear, focused purpose
2. Keep `SKILL.md` under 500 lines
3. Move detailed content to `references/`
4. Include clear trigger conditions in the description
5. Test with Claude before submitting

## License

MIT License - See [LICENSE](LICENSE) for details.

## Acknowledgments

These skills are derived from the [cursor-engineering-rules](https://github.com/d-padmanabhan/cursor-engineering-rules/tree/main) project, adapted for the Anthropic Agent Skills format.

Big shout out to [@d-padmanabhan](https://github.com/d-padmanabhan) who made the original cursor engineering rules files.
