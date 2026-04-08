# 好莱坞专业分镜头脚本规范规格

## Why

用户希望分镜头创作能够像好莱坞电影分镜头脚本一样专业，包含机位、景别、场景、构图、角色动作姿势等完整细节。需要基于业界标准的分镜头脚本格式制定规范。

## What Changes

- **定义专业分镜头格式**: 基于好莱坞标准的分镜头脚本格式
- **定义分镜头字段**: 包含镜头号、机位、景别、运动、画面内容、时长等
- **更新分镜头输出模板**: 从简单描述升级为完整的分镜头脚本格式
- **更新 SKILL.md**: 将专业分镜头规范整合到技能描述中

## Impact

- Affected specs: shot-composition
- Affected code: `.trae/skills/prompt-crafter/SKILL.md`

---

## ADDED Requirements

### Requirement: 好莱坞分镜头脚本标准字段

专业分镜头脚本应包含以下字段：

| 字段 | 说明 | 示例 |
|------|------|------|
| 镜头号 | 唯一标识符 | Shot 1, Shot 2A |
| 机位/角度 | 相机相对于主体的位置 | Low angle, High angle, Eye level |
| 景别 | 画面取景范围 | Wide, Medium, Close-up |
| 运动 | 相机运动方式 | Static, Pan left, Dolly in |
| 画面内容 | 详细的场景/角色描述 | Visual Description |
| 角色动作 | 具体的动作和表情 | Character Action |
| 构图 | 画面元素布局 | Composition |
| 光照 | 光线设计 | Natural light, Rim light |
| 时长 | 预计持续秒数 | 3-5 sec |
| 备注 | 特殊要求 | 升格拍摄 |

### Requirement: 机位类型定义

| 机位 | 说明 | 英文 |
|------|------|------|
| 平视 | 相机与主体视线齐平 | Eye level |
| 俯视 | 相机从高处向下拍 | High angle, Bird's eye |
| 仰视 | 相机从低处向上拍 | Low angle, Worm's eye |
| 正面 | 相机正对主体 | Front view, Straight-on |
| 侧面 | 相机与主体侧面成90度 | Side view, Profile |
| 背面 | 相机在主体背后 | Back view, Rear |
| 斜角度 | 相机与主体成45度 | Dutch angle, Canted |

### Requirement: 景别定义

| 景别 | 说明 | 英文 |
|------|------|------|
| 极远景 | 建立大环境 | Extreme Wide Shot (EWS) |
| 远景 | 交代场景 | Wide Shot (WS), Establishing Shot |
| 全景 | 显示全部主体 | Full Shot (FS) |
| 中全景 | 膝盖以上 | Medium Full Shot (MFS) |
| 中景 | 腰部以上 | Medium Shot (MS) |
| 近景 | 胸部以上 | Close-up (CU) |
| 特写 | 局部细节 | Extreme Close-up (ECU) |
| 两人对话 | 过肩镜头 | Over-the-Shoulder (OTS) |

### Requirement: 相机运动定义

| 运动类型 | 说明 | 英文 |
|----------|------|------|
| 固定 | 相机不动 | Static, Locked off |
| 推 | 相机靠近主体 | Dolly in, Zoom in, Push in |
| 拉 | 相机远离主体 | Dolly out, Zoom out, Pull out |
| 摇 | 相机水平旋转 | Pan left/right |
| 倾 | 相机垂直旋转 | Tilt up/down |
| 移 | 相机横向移动 | Tracking, Dolly |
| 跟 | 相机跟随主体移动 | Follow, Tracking shot |
| 环绕 | 相机围绕主体旋转 | Orbit, Revolve |
| 升降 | 相机垂直上下移动 | Crane up/down, Pedestal |
| 手持 | 手持晃动效果 | Handheld |

### Requirement: 专业分镜头脚本格式模板

**WHEN** 用户请求分镜头创作时
**THEN** 按以下专业格式输出：

```markdown
### 分镜头脚本

**SHOT 01 | 远景建立 | 固定机位**
- **机位**: Eye level / Wide
- **景别**: Wide Shot (WS) / Establishing
- **运动**: Static, tripod mounted
- **画面内容**: [详细描述场景环境]
- **角色动作**: [描述角色动作和表情]
- **构图**: [画面元素布局，如 rule of thirds]
- **光照**: [光线设计，如 Golden hour sunlight]
- **时长**: 4-5 sec
- **备注**: 升格拍摄 / 4K 60fps

---

**SHOT 02 | 中景叙事 | 推镜头**
- **机位**: Eye level / Front
- **景别**: Medium Shot (MS)
- **运动**: Slow dolly in, tracking subject
- **画面内容**: [描述主体和环境关系]
- **角色动作**: [具体动作，如 walking forward slowly]
- **构图**: Subject left of center, negative space right
- **光照**: Soft key light from right, subtle fill
- **时长**: 5-6 sec
- **备注**: 浅景深，背景虚化

---

**SHOT 03 | 特写强调 | 固定机位**
- **机位**: Eye level / Front
- **景别**: Close-up (CU)
- **运动**: Static, slight breathing movement acceptable
- **画面内容**: [强调细节，如面部表情、手部动作]
- **角色动作**: [微妙动作，如 gentle smile forming]
- **构图**: Face in upper third, eyes on power line
- **光照**: Rembrandt lighting, 45-degree key
- **时长**: 3-4 sec
- **备注**: 电影感色调
```

### Requirement: 角色动作描述规范

角色动作应包含以下细节维度：

| 维度 | 说明 | 示例 |
|------|------|------|
| 姿态 | 身体的整体姿势 | standing, sitting, crouching |
| 位置 | 身体在空间中的位置 | center frame, left third, foreground |
| 方向 | 身体面对的方向 | facing camera, profile left, back to camera |
| 动作 | 具体的行为 | walking, turning, reaching |
| 表情 | 面部的情绪表达 | smiling, looking away, eyes widening |
| 细节 | 手部、眼部等细节 | fingers touching chin, eyes glancing |

### Requirement: 构图原则

| 原则 | 说明 | 应用 |
|------|------|------|
| 三分法 | 将画面横竖各分三等份 | 主体放在交叉点上 |
| 头部空间 | 眼睛方向留出空间 | Looking right时，右边留白 |
| 负空间 | 利用空白区域 | 增强情绪表达 |
| 前景/背景 | 画面层次 | 前景物体增加深度 |
| 视线引导 | 引导观众视线 | 沿对角线放置元素 |

---

## 技术约束

1. 分镜头脚本应用于图生视频（暗号 `v`）时
2. 当用户输入包含分镜头指令时触发
3. 每个镜头描述应尽可能详细专业
4. 时长估算应合理（单镜头通常 3-8 秒）
5. 保持与用户一级提示词的一致性
