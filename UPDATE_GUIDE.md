# Quick Guide: Updating AGENT.md with Skills

This guide condenses key documentation guidelines from Claude (Anthropic), Cursor, and OpenAI for 2026, focused on modularity, token-efficiency, and cross-compatibility. Use this checklist to validate your update. [blog.lvrpiz](https://www.blog.lvrpiz.com/p/agent-skills-guia-completa-skillsmd)

## Checklist: Essential Criteria

- **Clear AGENT.md structure**: Include project overview (1-2 paragraphs), general coding rules, directory structure, and explicit skill references (e.g., "For data analysis, invoke skill-data"). Limit to <2000 tokens total. [frantufro](https://frantufro.com/como-programo-2026-4-agentes)
- **Modular skills in SKILLS.md**: Create a `/skills/` dir with one SKILLS.md per ability. Required YAML frontmatter: `name`, `description` (explicit, <100 tokens, with triggers like "when you see X"), `when_to_use`. Add step-by-step instructions and scripts. [q2bstudio](https://www.q2bstudio.com/nuestro-blog/472220/crea-habilidades-codex-skillsmd)
- **Precise descriptions for matching**: Be explicit in `description` (e.g., "Activate only for Electron v28+ upgrades"). Avoid ambiguity so Claude/Cursor auto-detects; test with simulation prompts. [github](https://github.com/heilcheng/awesome-agent-skills/blob/main/README.es.md)
- **Integration without bloat**: In AGENT.md, list available skills and fallback ("If no skill applies, explain why"). Enable on-demand loading to save context. Cursor recommends sync script for GitHub repos. [o-mega](https://o-mega.ai/articles/top-10-ai-agent-skills-for-2026-an-in-depth-guide)
- **OpenAI/Claude/Cursor compatibility**: Use open standards (Agent Skills spec 1.2+). Include MCP metadata if using external APIs. OpenAI suggests JSON outputs in skills for universal parsing. [cometapi](https://www.cometapi.com/es/claude-skills-vs-mcp-the-2026-guide-to-agentic-architecture/)
- **Testing best practices**:
  - Iterative testing: metadata first, then instructions/scripts.
  - Verify no overlap between skills (unique descriptions).
  - Limit to 5-10 initial skills; expand based on actual usage. [teamday](https://www.teamday.ai/es/blog/agent-skills-hard-truth)
- **2026 optimizations**: MCP UI support for interactive inputs. Cursor: add `version: 1.2` in YAML. Anthropic: include real input/output examples in skills. [o-mega](https://o-mega.ai/articles/top-10-ai-agent-skills-for-2026-an-in-depth-guide)

## Final Validation

Run this table for self-audit:

| Criterion | Done (✓/✗) | Notes |
|-----------|------------|-------|
| AGENT.md <2000 tokens [es.linkedin](https://es.linkedin.com/pulse/agentsmd-la-gu%C3%ADa-para-tus-asistentes-de-ia-mauricio-perera-j6tne) | | |
| Each skill with complete YAML [blog.lvrpiz](https://www.blog.lvrpiz.com/p/agent-skills-guia-completa-skillsmd) | | |
| Explicit triggers without overlap [frantufro](https://frantufro.com/como-programo-2026-4-agentes) | | |
| Cross-references AGENT-skills [eesel](https://www.eesel.ai/es/blog/skills-md-vs-agents-md) | | |
| Tested in Claude/Cursor [teamday](https://www.teamday.ai/es/blog/agent-skills-hard-truth) | | |

Copy this guide to your `UPDATE_GUIDE.md` and mark progressively. This ensures interoperability and efficiency per current specs. [aimafia.substack](https://aimafia.substack.com/p/skills-ia)
