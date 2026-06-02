# README Showcase Design

## Overview

Create a Chinese-first `README.md` for the `Desk Setup Planner Skill` GitHub repository. The README should present the project as an open-source Codex skill, not as a desk setup image gallery or product recommendation list.

The README should be visually richer than plain documentation, but still clear and practical for GitHub readers.

## Audience

The primary reader is a Chinese-speaking Codex user who wants to understand:

- What this skill does.
- When to use it.
- How to install it.
- How to trigger it with a short example.
- What kind of output it produces.
- What the skill does not promise.

## README Style

Use a project showcase style:

- Clear GitHub project homepage structure.
- Chinese as the main language.
- Keep the English project name `Desk Setup Planner Skill` for recognizability.
- Avoid a product-marketing tone.
- Avoid making it look like a desk setup inspiration gallery.
- Keep technical internals minimal.

## Visual Plan

The README should include:

1. A product-illustration-style cover image.
2. A small set of static badges.
3. A Mermaid workflow diagram.
4. Tables or concise lists for capability and output overview.

The cover image should:

- Use product illustration style.
- Communicate an AI-assisted desk setup planning workflow.
- Include visual cues such as a desk, monitor, planning board, checklist, budget or purchase list elements, and tidy cable-management hints.
- Avoid looking like a photoreal desk setup recommendation image.
- Avoid relying on text inside the image, because generated text may be unreliable.
- Be saved as `assets/readme-cover.png`.
- Be referenced from README with `![Desk Setup Planner Skill](assets/readme-cover.png)`.

## README Structure

Use this structure:

1. Static badges.
2. Cover image.
3. Project title and one-sentence summary.
4. "它能做什么" capability overview.
5. Workflow Mermaid diagram.
6. Installation section with simple commands.
7. Short usage example.
8. Output overview.
9. Boundary reminders.
10. Optional license or note section.

## Badges

Use a small set of static shields-style badges:

- `Codex Skill`
- `Markdown`
- `中文文档`
- `Status: Ready`

Do not add CI, release, npm, package, or test badges unless such systems actually exist.

## Installation Section

Keep installation simple:

```bash
mkdir -p ~/.codex/skills
cp -R desk-setup-planner ~/.codex/skills/desk-setup-planner
```

Mention that after copying, a new or refreshed Codex session is usually needed before the skill is discovered.

Do not claim the skill is already installed globally.

## Usage Example

Include one short example input only. Do not include a long generated handbook.

Example:

```text
我想改造现有桌面，预算 3000 元。
主要用途：工作 > 游戏 > 影音。
已有：桌子、主显示器、笔记本电脑。
桌子不换，墙面不能打孔。
我会上传当前桌面整体照片。
```

## Output Overview

Do not show the full planning handbook template.

Briefly explain that the skill can produce a Markdown desk setup planning handbook containing:

- Candidate plans.
- Final combined plan.
- Purchasing list.
- Budget table.
- Deferred or not-recommended purchase list.
- Pre-purchase verification checklist.
- Cable management plan.
- Installation steps.
- Upgrade path.

## Boundary Reminders

Include a short boundary section:

- Suitable for desk setup planning, current desk diagnosis, purchasing guidance, budget breakdown, cable management, installation steps, and upgrade paths.
- Not intended for full home renovation, room decoration, or dedicated ergonomics assessment.
- Specific model, price, stock, latest parameter, regional availability, and warranty details should be verified online before purchase.
- The project does not promise that a brand or model will definitely fit the user.
- The project does not promise the lowest online price.

## GitHub Publishing

The target repository is:

`https://github.com/douzhenyu/Desk-setup-plannerSkill.git`

The local repository currently has no remote configured. Implementation should:

1. Add `origin` if missing.
2. Commit README and visual asset.
3. Push the current feature branch `codex/desk-setup-planner` unless the user later asks to push to another branch.

If GitHub authentication is unavailable, report the failed push clearly and leave the local commit intact.

## Success Criteria

The README is successful when:

- It presents the skill clearly as a Codex skill project.
- It includes a generated product-illustration cover image.
- It has badges, capability overview, workflow diagram, installation commands, one usage example, output overview, and boundary reminders.
- It avoids unnecessary technical directory explanations.
- It does not include the full handbook template.
- It does not imply global installation has already happened.
- It is committed locally and pushed to the target GitHub repository if authentication allows.
