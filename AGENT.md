# AGENT.md - Project Entry Point

## First Step
**Read `CLAUDE.md` before any action.** It contains philosophy, skills, and agent behavior.

## Project Analysis
1. Detect stack from dependencies (composer.json, package.json, requirements.txt)
2. Load relevant skills from `skills/` directory
3. Follow skill patterns for all code

## Skill Detection

See `CLAUDE.md` for full skills table with triggers:
- **Laravel/PHP**: .php files, composer.json → `skills/skill-laravel.md`
- **Livewire**: wire:model, TALL stack → `skills/skill-livewire.md`
- **Filament**: Filament resources → `skills/skill-filament.md`
- **React + Inertia**: .tsx/.jsx, RILT stack → `skills/skill-react-inertia.md`
- **Tailwind CSS**: Tailwind classes → `skills/skill-tailwind.md`
- **Python/Django**: .py files, manage.py → `skills/skill-python-django.md`
- **AI Workflow**: AI patterns → `skills/skill-ai-workflow.md`

## Agent Specialists

For specific tasks, refer to specialized agents in `agents/`:
- **agent-code.md**: Code generation and new features
- **agent-review.md**: Code review and refactoring
- **agent-debug.md**: Debugging and troubleshooting

## Fallback
If no skill applies, explain why and propose approach following Philosophy from `CLAUDE.md`.

---
*This file is minimal (<500 tokens). Full config is in `CLAUDE.md`.*
