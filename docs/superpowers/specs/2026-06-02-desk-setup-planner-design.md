# Desk Setup Planner Skill Design

## Overview

`desk-setup-planner` is a Codex skill for desk setup planning. It acts as a structured desk setup consultant that helps a user move from requirements to a final Markdown planning handbook.

The skill focuses on two user goals:

- Planning a new or heavily rebuilt desk setup.
- Diagnosing and improving an existing desk setup.

The skill should not become a full home renovation, room design, or dedicated ergonomics assessment workflow. Room-level details are considered only when they directly affect the desk setup.

## Trigger Conditions

Use this skill when the user wants to:

- Plan a desk setup.
- Improve, diagnose, or reorganize an existing desk setup.
- Generate desk setup options from budget, use cases, aesthetic preferences, and existing equipment.
- Analyze current desk photos or compare them with reference desk setup photos.
- Produce purchasing guidance, budget tables, cable management plans, installation steps, or upgrade paths.
- Summarize the outcome as a Markdown desk setup planning handbook.

## Primary Modes

The skill starts by identifying one of two modes.

| Mode | When to Use | Goal |
|---|---|---|
| Build From Zero | The user has no current setup, wants a new setup, or plans a major rebuild | Collect requirements, generate inspiration images if needed, propose three options, converge on a final plan, and produce a handbook |
| Diagnose and Improve | The user already has a setup, uploads current photos, or describes current problems | Identify existing items, reusable items, visible issues, low-cost improvements, and upgrade paths |

If the mode is unclear, ask:

> Are you building from zero, or improving an existing desk setup?

## Interaction Model

Use a mixed interaction style:

1. Start with a short questionnaire.
2. Ask consultant-style follow-up questions based on conflicts, missing information, or risky assumptions.
3. Generate three candidate plans.
4. Let the user mix modules across plans.
5. Produce a final combined plan and Markdown handbook.

The tone should be professional and structured, with enough aesthetic language to support decisions. Avoid exaggerated marketing language or unsupported guarantees.

## Initial Questionnaire

The first questionnaire should stay short and gather only the core planning inputs:

1. Are you building from zero or improving an existing desk setup?
2. What is the main use-case order? For example: work > gaming > media > content creation.
3. What is the total budget? Default currency is RMB. If the user is unsure, guide them with 500 RMB budget intervals.
4. What large items do you already own and want to keep? Examples: desk, chair, monitor, computer, speakers, lights, monitor arms.
5. What cannot change? Examples: desk must stay, wall cannot be drilled, existing monitor must be used.
6. What styles do you like or dislike? The user may describe them freely or upload reference images.

## Budget Rules

- Default currency is RMB.
- Ask for the total budget early.
- If the user is unsure, use 500 RMB intervals such as 500-1000, 1000-1500, 1500-2000, and extend dynamically as needed.
- Do not set a fixed upper budget range.
- Use the budget to shape the planning boundary, not only the final table.
- If the budget conflicts with the user's goals, clearly explain the conflict and provide trade-offs.
- Existing items that the user confirms they will keep must be excluded from the purchasing list and placed in the reuse list.

## Image Handling Rules

When the user uploads images, first classify the image type.

| Image Type | Use |
|---|---|
| Current desk or room photo | Identify visible existing items, current issues, reusable items, and pending confirmations |
| Reference desk setup image | Extract style, layout, atmosphere, and transferable ideas |
| Current photo plus reference image | Compare differences, transferable elements, and unsuitable elements |
| Detail photo | Confirm a narrow detail such as cables, the back of a monitor, outlets, or under-desk space |

For current desk diagnosis, ask for an overall photo when possible. It should ideally show:

- Full desktop.
- Monitors and peripherals.
- Left and right desktop boundaries.
- Approximate under-desk or cable condition.
- Background wall or area behind the desk.
- Outlet or power location if convenient.

Detail photos can supplement the diagnosis, but should not be treated as enough to infer the whole setup.

## Immutable Constraints

Maintain an explicit list of immutable constraints:

- Items the user explicitly says should not be replaced.
- Structures that cannot change, such as walls that cannot be drilled, fixed cabinets, or fixed furniture.
- Large visible items in photos whose replaceability is unclear. Mark these as pending confirmation instead of assuming they can be replaced.

When generating final plans or final reference images, preserve immutable constraints. For an existing setup, do not alter the desk style, desktop size, wall structure, fixed cabinets, or user-confirmed retained equipment unless the user explicitly allows it.

For a build-from-zero setup, current physical constraints do not need to be preserved unless the user provides specific constraints.

## Reference Image Generation

The skill can generate visual references in two stages only:

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

Style categories are only prompts for discussion, not closed options.

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

If the user is mainly focused on aesthetics, the three plans may be three visual directions. If the user is mainly focused on cost, the three plans may be three budget tiers.

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

The user does not have to choose exactly one plan. Allow mixing:

- Plan A's budget discipline.
- Plan B's monitor and arm layout.
- Plan C's lighting or storage direction.
- Deleting modules.
- Retaining existing equipment.
- Compressing budget or deferring purchases.

The final output should be a combined plan that explains which modules were adopted and why.

## Purchasing List Rules

The purchasing list should use "specifications first, brand directions second."

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

Do not guarantee that a brand or model will definitely be good or suitable. The skill can only provide lower-risk suggestions based on specifications, fit, review patterns, and the user's constraints.

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

Use the following diagnosis categories. It is acceptable to mark a category as not applicable or not visible.

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

The skill must not:

- Guarantee that a specific brand or model will be good or suitable.
- Promise the lowest online price.
- Give certain conclusions when key dimensions, load capacity, ports, or power information is missing.
- Infer the full desk setup from a local detail photo.
- Expand into full room renovation, home decoration, or a dedicated ergonomics assessment.
- Recommend buying items the user has confirmed they will keep.
- Show image generation prompts unless the user explicitly asks.
- Maintain version history in the final handbook.

## Success Criteria

The skill is successful when:

- It reliably separates build-from-zero and diagnose-and-improve workflows.
- It asks enough questions to avoid obvious planning errors without turning the interaction into a long form.
- It produces three comparable candidate plans.
- It supports mixing plan modules into a final combined plan.
- It excludes retained items from purchasing recommendations.
- It distinguishes specifications and brand directions from exact model recommendations.
- It warns about online verification when current product details are involved.
- It produces a complete Markdown desk setup planning handbook.
