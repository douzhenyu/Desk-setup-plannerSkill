# Desk Setup Planner Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a Codex skill named `desk-setup-planner` that guides multi-round desk setup planning and produces a Markdown desk setup planning handbook.

**Architecture:** The skill is implemented as a self-contained skill folder in this repository, with a concise `SKILL.md` for trigger metadata and operating workflow plus `agents/openai.yaml` for UI-facing metadata. The repository artifact can then be copied into `$CODEX_HOME/skills/desk-setup-planner` when the user wants to install it globally.

**Tech Stack:** Markdown skill instructions, YAML metadata, shell verification with `rg`, `sed`, and optional copy installation.

---

## File Structure

- Create: `desk-setup-planner/SKILL.md`
  - Required skill file.
  - Contains YAML frontmatter with `name` and `description`.
  - Contains the workflow, mode selection, questionnaire, image rules, candidate plan rules, purchase rules, online verification rules, diagnosis mode, handbook template, and boundaries.
- Create: `desk-setup-planner/agents/openai.yaml`
  - UI-facing metadata for skill lists and chips.
  - Contains deterministic display name, short description, and default prompt.
- Modify: no existing source files.
- Optional install target after user approval: `$CODEX_HOME/skills/desk-setup-planner/`
  - Copy the completed `desk-setup-planner` folder into the Codex skills directory.

### Task 1: Create the Skill Folder and `SKILL.md`

**Files:**
- Create: `desk-setup-planner/SKILL.md`

- [ ] **Step 1: Create the folder**

Run:

```bash
mkdir -p desk-setup-planner
```

Expected: command exits with code 0.

- [ ] **Step 2: Create `SKILL.md` with complete skill instructions**

Write this exact content to `desk-setup-planner/SKILL.md`:

```markdown
---
name: desk-setup-planner
description: Use when planning, diagnosing, or improving a desk setup; when the user wants desk setup options based on budget, use cases, existing equipment, photos, or reference images; or when the user wants a purchasing list, budget table, cable management plan, installation steps, upgrade path, visual references, or a final Markdown desk setup planning handbook.
---

# Desk Setup Planner

Use this skill as a structured desk setup consultant. Help the user move from requirements to a final Markdown planning handbook through short questionnaires, follow-up questions, three candidate plans, mixed-plan convergence, and a final implementation-oriented plan.

Do not turn the task into full home renovation, room design, or a dedicated ergonomics assessment. Room-level details matter only when they directly affect the desk setup.

## Operating Principles

- Use a mixed style: short questionnaire first, then consultant-style follow-up.
- Be structured, direct, and cautious about assumptions.
- Prefer practical trade-offs over decorative recommendations.
- Use aesthetic language only when it helps a decision.
- Do not over-praise the user or the plan.
- Do not make guarantee-style claims about brands, models, prices, or user experience.
- Maintain a clear list of retained items, immutable constraints, and pending confirmations.
- Do not recommend buying items the user has confirmed they will keep.

## Step 1: Choose the Mode

Start by identifying one of two modes:

| Mode | Use When | Goal |
|---|---|---|
| Build From Zero | The user has no current setup, wants a new setup, or plans a major rebuild | Collect requirements, optionally generate inspiration images, propose three plans, converge on a final combined plan, and produce a handbook |
| Diagnose and Improve | The user already has a setup, uploads current photos, or describes current problems | Identify existing items, reusable items, visible issues, low-cost improvements, and upgrade paths |

If the mode is unclear, ask:

> 你是准备从 0 搭建，还是基于现有桌面做诊断和改造？

## Step 2: Ask the Short Questionnaire

Keep the first questionnaire short. Ask for:

1. Mode: from zero or improving an existing desk setup.
2. Main use-case order, such as work > gaming > media > content creation.
3. Total budget. Default currency is RMB.
4. Large existing items the user owns and wants to keep, such as desk, chair, monitor, computer, speakers, lights, and monitor arms.
5. Immutable constraints, such as desk must stay, wall cannot be drilled, existing monitor must be used.
6. Style preferences or dislikes. The user may describe them freely or upload reference images.

If the user is unsure about budget, guide with 500 RMB intervals such as 500-1000, 1000-1500, 1500-2000, and extend dynamically as needed. Do not set a fixed upper range.

## Step 3: Handle Images

When images are provided, classify them before using them:

| Image Type | Use |
|---|---|
| Current desk or room photo | Identify visible existing items, current issues, reusable items, and pending confirmations |
| Reference desk setup image | Extract style, layout, atmosphere, and transferable ideas |
| Current photo plus reference image | Compare differences, transferable elements, and unsuitable elements |
| Detail photo | Confirm a narrow detail such as cables, the back of a monitor, outlets, or under-desk space |

For diagnosis or improvement, prefer an overall current photo that shows:

- Full desktop.
- Monitors and peripherals.
- Left and right desktop boundaries.
- Approximate under-desk or cable condition.
- Background wall or area behind the desk.
- Outlet or power location if convenient.

Detail photos can supplement the diagnosis, but must not be treated as enough to infer the whole setup.

## Immutable Constraints

Maintain an explicit immutable constraints list:

- Items the user says should not be replaced.
- Structures that cannot change, such as walls that cannot be drilled, fixed cabinets, or fixed furniture.
- Large visible items in photos whose replaceability is unclear.

Mark unclear items as pending confirmation instead of assuming they can be replaced.

When generating final plans or final reference images for an existing setup, preserve immutable constraints. Do not alter the desk style, desktop size, wall structure, fixed cabinets, or user-confirmed retained equipment unless the user explicitly allows it.

For a build-from-zero setup, current physical constraints do not need to be preserved unless the user provides specific constraints.

## Reference Images

Generate reference images only in these stages:

1. Early inspiration images.
2. Final plan effect images.

For early inspiration images:

- Generate 2-4 images when useful.
- Use them to help the user choose aesthetic direction and atmosphere.
- Do not show prompts unless the user explicitly asks.
- Add a short note for each image explaining direction, suitable use case, budget tendency, and implementation difficulty.

For final plan effect images:

- Generate images only after the final plan is confirmed.
- Follow confirmed budget, retained items, immutable constraints, and final configuration.
- Do not show prompts unless the user explicitly asks.
- Add a short note explaining which final plan elements each image reflects.

## Style Handling

Style categories are prompts for discussion, not closed options.

The user may freely mix styles, such as:

- Minimal but warmer with wood tones.
- White tech style, but not too cold.
- Content-creation friendly, but not like a studio in daily use.

If the user uploads reference images, extract concrete style traits instead of forcing the image into a fixed label.

When comparing a current setup with a reference setup, output:

- Similarities.
- Differences.
- Transferable elements.
- Elements not recommended for transfer.
- Low-cost improvement path.
- High-cost upgrade path.

## Candidate Plans

Generate three candidate plans by default:

| Plan | Positioning |
|---|---|
| Plan A: Conservative and Practical | Reuse existing items and solve core problems with minimal spending |
| Plan B: Balanced Recommendation | Balance budget, effect, and implementation difficulty |
| Plan C: Advanced Upgrade | More complete and stylistically stronger, with higher cost and complexity |

If the user mainly cares about aesthetics, the three plans may be three visual directions. If the user mainly cares about cost, the three plans may be three budget tiers.

Each candidate plan must include:

- Core idea.
- Estimated budget.
- Reused items.
- Suggested purchases.
- Deferred or not-recommended purchases.
- Advantages.
- Costs or trade-offs.
- Best-fit scenario.

## Plan Convergence

Do not force the user to choose only one plan. Allow mixing:

- Plan A's budget discipline.
- Plan B's monitor and arm layout.
- Plan C's lighting or storage direction.
- Deleted modules.
- Retained existing equipment.
- Compressed budget or deferred purchases.

The final output should be a combined plan that explains which modules were adopted and why.

## Purchasing List Rules

Use "specifications first, brand directions second."

| Field | Requirement |
|---|---|
| Category | Monitor, arm, light, storage, cable management, power, audio, peripherals, etc. |
| Recommended Specification | Prefer specifications over exact models |
| Brand Direction | Provide broad brand direction or same-tier references |
| Budget | Use RMB by default |
| Priority | Must-buy, recommended, optional, deferred |
| Required | Yes or no |
| Alternative | Low-cost or second-hand alternative where appropriate |
| Verification Reminder | Dimensions, ports, load capacity, warranty, and similar checks |

Confirmed retained items belong in the reuse list and must not be recommended again in the purchasing list.

## Online Verification Rules

If the output involves any of the following, remind the user to verify online or perform online verification if requested and available:

- Specific model.
- Current price.
- Stock.
- Latest specifications.
- Regional availability.
- Platform promotions.
- Warranty or return policy.
- Detailed compatibility.

Without online verification, limit output to specifications, brand directions, decision criteria, and pre-purchase checks.

Do not guarantee that a brand or model will definitely be good or suitable. Provide lower-risk suggestions based on specifications, fit, review patterns, and the user's constraints.

Do not claim to find the lowest online price.

## Second-Hand and Low-Cost Alternatives

Second-hand or low-cost alternatives are allowed when the budget is tight or the user explicitly prefers them.

Good candidates may include:

- Monitor arms.
- Monitors.
- Desk lamps.
- Storage items.
- Speakers.

Avoid recommending risky second-hand categories such as:

- Unknown power strips.
- Unknown power adapters.
- Aged cables.
- Chairs with structural damage.

Second-hand suggestions must include verification points such as condition, ports, dead pixels, arm damping, heat, noise, and return possibility.

## Diagnosis Mode

In diagnose-and-improve mode, first diagnose the current setup before planning changes.

Use these diagnosis categories. It is acceptable to mark a category as not applicable or not visible.

| Category | Checks |
|---|---|
| Desktop clutter | Too many items, mixed frequent and infrequent items, insufficient operation area |
| Cable mess | Exposed, crossing, hanging, or floor-dragging cables; poor power location |
| Display and arms | Monitor count, position, stand footprint, blocked sight lines |
| Lighting and atmosphere | Desk lamp, monitor light bar, ambient light, glare, visual effect for content |
| Storage and access | Drawers, trays, shelves, headphone or controller placement, hard drive placement |
| Peripheral consistency | Keyboard, mouse, desk mat, speakers, microphone, color and material consistency |
| Power safety | Power strip count, wattage, cable quality, under-desk mounting, kick or pull risks |
| Background and filming | Camera-visible area, wall, cabinets, clutter, streaming or video background |
| Budget waste risk | Buying duplicates, buying decorations before solving core issues |

Diagnosis output must include:

- Visible existing items.
- Likely reusable items.
- Pending confirmations.
- Main issues.
- Priority improvements.
- Low-cost improvements.
- Purchases to approach cautiously.

## Final Markdown Handbook

The final deliverable is a Markdown handbook. Do not generate CSV or XLSX files by default.

Use this structure:

```markdown
# 桌搭规划手册

## 1. 封面摘要
- 桌搭定位：
- 主用途排序：
- 总预算：
- 关键约束：
- 最终方案一句话总结：

## 2. 需求与约束
### 用户目标
### 已有并保留的物品
### 不可变条件
### 待确认信息

## 3. 三套候选方案回顾
### 方案 A：保守实用
### 方案 B：均衡推荐
### 方案 C：进阶升级

## 4. 最终合成方案
### 采用模块
### 舍弃模块
### 组合理由

## 5. 采购清单
| 品类 | 推荐规格 | 品牌方向 | 预算 | 优先级 | 是否必须 | 替代方案 | 核验提醒 |
|---|---|---|---:|---|---|---|---|

## 6. 预算表
| 类型 | 金额 | 说明 |
|---|---:|---|
| 总预算 |  |  |
| 必买项 |  |  |
| 可选项 |  |  |
| 暂缓项 |  |  |
| 预算余量/超支 |  |  |

## 7. 暂缓/不推荐购买清单
| 项目 | 建议 | 原因 | 何时再考虑 |
|---|---|---|---|

## 8. 购买前核验清单
| 类别 | 核验点 |
|---|---|
| 尺寸 | 桌面长宽、显示器尺寸、支架占位、桌下空间 |
| 承重 | 桌板承重、支架承重、显示器重量、夹持厚度 |
| 接口 | 显示器 HDMI/DP/USB-C、电脑接口、扩展坞接口 |
| 安装 | VESA 孔位、是否需要打孔、支架夹具是否适配桌边 |
| 电源 | 插座数量、排插功率、充电器功率、线长 |
| 线材 | 是否自带线材、线材长度、是否需要转接头 |
| 售后 | 退换货政策、质保、是否支持无理由退货 |
| 视觉 | 颜色、材质、尺寸比例是否与现有桌面协调 |
| 风险 | 评价中的共性问题、噪音、发热、松动、掉漆等 |

## 9. 线缆整理方案
### 电源规划
### 走线路径
### 理线配件
### 桌面隐藏策略
### 安全提醒

## 10. 安装步骤
### 采购顺序
### 组装顺序
### 调试顺序
### 验收检查

## 11. 升级路线
### 近期
### 中期
### 长期
### 每阶段触发条件

## 12. 参考图说明
```

## Boundaries

Do not:

- Guarantee that a specific brand or model will be good or suitable.
- Promise the lowest online price.
- Give certain conclusions when key dimensions, load capacity, ports, or power information is missing.
- Infer the full desk setup from a local detail photo.
- Expand into full room renovation, home decoration, or a dedicated ergonomics assessment.
- Recommend buying items the user has confirmed they will keep.
- Show image generation prompts unless the user explicitly asks.
- Maintain version history in the final handbook.
```

- [ ] **Step 3: Verify the required frontmatter exists**

Run:

```bash
sed -n '1,8p' desk-setup-planner/SKILL.md
```

Expected output begins with:

```text
---
name: desk-setup-planner
description: Use when planning, diagnosing, or improving a desk setup
```

- [ ] **Step 4: Commit Task 1**

Run:

```bash
git add desk-setup-planner/SKILL.md
git commit -m "Add desk setup planner skill"
```

Expected: commit succeeds and includes `desk-setup-planner/SKILL.md`.

### Task 2: Add Skill UI Metadata

**Files:**
- Create: `desk-setup-planner/agents/openai.yaml`

- [ ] **Step 1: Create the metadata folder**

Run:

```bash
mkdir -p desk-setup-planner/agents
```

Expected: command exits with code 0.

- [ ] **Step 2: Create `agents/openai.yaml`**

Write this exact content to `desk-setup-planner/agents/openai.yaml`:

```yaml
display_name: 桌搭规划顾问
short_description: 通过问卷、照片诊断、三套方案和采购执行清单规划桌搭。
default_prompt: 帮我规划一个桌搭方案。请先判断我是从 0 搭建还是改造现有桌面，再询问必要参数，给出三套候选方案，并在我确认后汇总成 Markdown 桌搭规划手册。
```

- [ ] **Step 3: Verify the metadata file**

Run:

```bash
sed -n '1,20p' desk-setup-planner/agents/openai.yaml
```

Expected output:

```text
display_name: 桌搭规划顾问
short_description: 通过问卷、照片诊断、三套方案和采购执行清单规划桌搭。
default_prompt: 帮我规划一个桌搭方案。请先判断我是从 0 搭建还是改造现有桌面，再询问必要参数，给出三套候选方案，并在我确认后汇总成 Markdown 桌搭规划手册。
```

- [ ] **Step 4: Commit Task 2**

Run:

```bash
git add desk-setup-planner/agents/openai.yaml
git commit -m "Add desk setup planner metadata"
```

Expected: commit succeeds and includes `desk-setup-planner/agents/openai.yaml`.

### Task 3: Validate Against the Design Spec

**Files:**
- Read: `docs/superpowers/specs/2026-06-02-desk-setup-planner-design.md`
- Read: `desk-setup-planner/SKILL.md`
- Read: `desk-setup-planner/agents/openai.yaml`

- [ ] **Step 1: Check for required sections in `SKILL.md`**

Run:

```bash
rg -n "Step 1: Choose the Mode|Step 2: Ask the Short Questionnaire|Step 3: Handle Images|Immutable Constraints|Reference Images|Style Handling|Candidate Plans|Plan Convergence|Purchasing List Rules|Online Verification Rules|Second-Hand and Low-Cost Alternatives|Diagnosis Mode|Final Markdown Handbook|Boundaries" desk-setup-planner/SKILL.md
```

Expected: each listed section appears at least once.

- [ ] **Step 2: Check for incomplete marker words in the generated skill**

Run:

```bash
rg -n "T[B]D|T[O]DO|F[I]XME|待[ ]定|暂[ ]定|以后[ ]补|fill[ ]in|place[ ]holder" desk-setup-planner
```

Expected: no matches.

- [ ] **Step 3: Check that retained-item and online-verification rules are present**

Run:

```bash
rg -n "Do not recommend buying items the user has confirmed they will keep|Specific model|Current price|Stock|Latest specifications|Warranty or return policy|Do not claim to find the lowest online price" desk-setup-planner/SKILL.md
```

Expected: all phrases appear.

- [ ] **Step 4: Check repository status**

Run:

```bash
git status --short
```

Expected: only the implementation plan file is uncommitted, unless Task 1 or Task 2 was not committed yet.

- [ ] **Step 5: Commit Task 3 if validation required fixes**

If Task 3 required edits, run:

```bash
git add desk-setup-planner docs/superpowers/plans/2026-06-02-desk-setup-planner.md
git commit -m "Validate desk setup planner skill"
```

Expected: commit succeeds. If no edits were needed, skip this commit.

### Task 4: Optional Global Installation

**Files:**
- Copy from: `desk-setup-planner/`
- Copy to: `$CODEX_HOME/skills/desk-setup-planner/`

This task should run only after the user confirms they want the skill installed into their Codex skills directory.

- [ ] **Step 1: Resolve the Codex skills directory**

Run:

```bash
printf '%s\n' "${CODEX_HOME:-$HOME/.codex}/skills"
```

Expected output is an absolute path, usually:

```text
/Users/yangleduo/.codex/skills
```

- [ ] **Step 2: Install the skill**

Run:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
rm -rf "${CODEX_HOME:-$HOME/.codex}/skills/desk-setup-planner"
cp -R desk-setup-planner "${CODEX_HOME:-$HOME/.codex}/skills/desk-setup-planner"
```

Expected: command exits with code 0.

- [ ] **Step 3: Verify installed files**

Run:

```bash
find "${CODEX_HOME:-$HOME/.codex}/skills/desk-setup-planner" -maxdepth 3 -type f | sort
```

Expected output includes:

```text
/Users/yangleduo/.codex/skills/desk-setup-planner/SKILL.md
/Users/yangleduo/.codex/skills/desk-setup-planner/agents/openai.yaml
```

- [ ] **Step 4: Report installation status**

Tell the user:

```text
已安装到 /Users/yangleduo/.codex/skills/desk-setup-planner。新的 Codex 会话通常需要重新加载后才能看到新 skill。
```

Do not commit files under `$CODEX_HOME`; they are outside this repository.

## Self-Review Checklist

- [ ] The plan creates the required `SKILL.md`.
- [ ] The plan creates recommended `agents/openai.yaml`.
- [ ] The plan keeps the actual skill in the repository before optional global installation.
- [ ] The plan covers build-from-zero and diagnose-and-improve modes.
- [ ] The plan covers image classification, immutable constraints, reference image timing, three candidate plans, mixed convergence, purchasing rules, online verification, second-hand rules, diagnosis categories, and the final Markdown handbook.
- [ ] The plan avoids unsupported guarantee claims and lowest-price claims.
- [ ] The plan includes exact verification commands.
