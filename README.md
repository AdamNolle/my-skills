# My Skills

A personal collection of Codex skills for interface design and evidence-led engineering review. Each skill is a self-contained directory under [`skills/`](skills/).

## Skills

| Skill | Use it for |
| --- | --- |
| [Design UI](skills/design-ui/SKILL.md) | Design, build, or refine distinctive websites and app interfaces using a 22-site visual research archive. |
| [anti-ai](skills/anti-ai/SKILL.md) | Start a broad code-quality review, select the relevant specialist, or carry out explicitly requested cleanup. |
| [anti-ai-comments](skills/anti-ai-comments/SKILL.md) | Check source comments and docstrings for accurate, useful information. |
| [anti-ai-contracts](skills/anti-ai-contracts/SKILL.md) | Review API, state, ownership, and compatibility boundaries. |
| [anti-ai-docs](skills/anti-ai-docs/SKILL.md) | Audit and maintain READMEs, guides, examples, and operational documentation. |
| [anti-ai-performance](skills/anti-ai-performance/SKILL.md) | Investigate measured performance and concurrency behavior. |
| [anti-ai-refactor](skills/anti-ai-refactor/SKILL.md) | Plan and perform bounded, compatibility-aware simplification. |
| [anti-ai-security](skills/anti-ai-security/SKILL.md) | Triage trust and authority boundaries during engineering review; use a dedicated security workflow for a formal scan. |
| [anti-ai-tests](skills/anti-ai-tests/SKILL.md) | Check behavioral coverage and whether verification actually proves the claimed result. |

The anti-ai family includes eight complete packages: each has a `SKILL.md`, Codex agent metadata, an icon, and its own reference material. The broad [anti-ai skill](skills/anti-ai/SKILL.md) includes language and domain references; [its routing guide](skills/anti-ai/references/stack-routing.md) helps select a specialist for work that crosses domains. Load only the references relevant to the task.

## Install

Clone this repository, then copy the skill directories you want from `skills/` into your Codex user skills directory (`~/.codex/skills/` on macOS/Linux, or `%USERPROFILE%\.codex\skills\` on Windows). Keep each directory intact so relative links to `references/`, `agents/`, `assets/`, and Design UI's `evidence/` continue to work. For example, on macOS/Linux:

```sh
git clone https://github.com/AdamNolle/my-skills.git
mkdir -p ~/.codex/skills
cp -R my-skills/skills/anti-ai* ~/.codex/skills/
```

That example installs all eight anti-ai skills. Copy `my-skills/skills/design-ui` as well if you want Design UI. If a skill with the same name is already installed, compare it before replacing it.

## Use

Invoke the broad review with `$anti-ai`, a focused review with a name such as `$anti-ai-tests`, or interface work with `$design-ui`. You can also describe the task in plain language and let Codex choose a relevant installed skill.

The anti-ai skills **audit first**. A request to review code normally produces findings and evidence without editing it. Ask for a fix or implementation when you want changes. They assess concrete engineering behavior and maintainability; style alone does not establish whether AI wrote code. Reviews stay within the requested scope and report what was inspected and verified.

Design UI starts with the [core workflow](skills/design-ui/SKILL.md), then uses [design directions](skills/design-ui/references/design-directions.md), [build guidance](skills/design-ui/references/craft-and-build.md), and the [review guide](skills/design-ui/references/review.md). Its screenshots and captured data document a 22-site study from 2026-09-17. They are research evidence, not production assets or licenses to copy another creator's work.

## Validation

[CI](.github/workflows/ci.yml) checks the `SKILL.md` metadata and content heading for every installed skill directory on pull requests and pushes to `main`.
