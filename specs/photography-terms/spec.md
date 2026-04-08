# 摄影术语扩展规格

## Why

用户在进行图生图/图生视频时，除了描述主体、场景等元素外，还会补充大量摄影层面的专业描述，如镜头运动（升格镜头、推镜头、拉镜头）、景别（特写、近景、全景）、拍摄角度（俯视、仰视）等。这些词汇需要被正确识别并转换为标准术语，才能生成高质量的 AI 生成提示词。

## What Changes

- **新增摄影术语映射**: 将用户输入的摄影口语词汇转换为标准英文术语
- **新增镜头运动类型**: 扩展现有的镜头运动映射表
- **新增景别分类**: 添加完整的景别术语映射
- **新增拍摄角度映射**: 添加角度术语映射
- **更新 SKILL.md**: 将摄影术语整合到现有技能中

## Impact

- Affected specs: prompt-levels、image-reference
- Affected code: `.trae/skills/prompt-crafter/SKILL.md`

---

## ADDED Requirements

### Requirement: 镜头运动术语映射

系统 SHALL 能够将用户输入的镜头运动词汇转换为标准术语。

| 用户输入 | 标准术语 | 说明 |
|----------|----------|------|
| 升格镜头 | slow motion, high frame rate (60fps+), cinematic slow motion | 慢动作拍摄 |
| 降格镜头 | fast motion, timelapse, speed ramp | 快动作/延时 |
| 推镜头 | zoom in, push in, dolly in, close-up | 推进画面 |
| 拉镜头 | zoom out, pull back, dolly out, wide shot | 拉远画面 |
| 摇镜头 | pan, pan left, pan right, tilt, tilt up, tilt down | 水平/垂直摇动 |
| 移镜头 | tracking shot, dolly shot, moving shot, crab shot | 移动拍摄 |
| 跟镜头 | follow shot, tracking shot, leading shot | 跟随之 |
| 甩镜头 | whip pan, fast pan, swish pan | 快速摇镜 |
| 升镜头 | crane up, tilt up, rising shot | 上升拍摄 |
| 降镜头 | crane down, tilt down, falling shot | 下降拍摄 |
| 旋转镜头 | spin, rotate, roll, barrel roll | 旋转效果 |
| 轻微运镜 | subtle camera movement, gentle motion | 轻微移动 |

### Requirement: 景别术语映射

系统 SHALL 能够将用户输入的景别词汇转换为标准术语。

| 用户输入 | 标准术语 | 说明 |
|----------|----------|------|
| 特写 | close-up shot, CU, tight shot | 局部细节 |
| 近景 | close-up, bust shot | 胸部以上 |
| 中景 | medium shot, MS, waist shot | 腰部以上 |
| 中全景 | medium full shot, MFS, three-quarter shot | 膝盖以上 |
| 全景 | wide shot, full shot, WS, full body view | 全身 |
| 远景 | wide shot, establishing shot, long shot, LS | 环境交代 |
| 全身镜头 | full body shot, full length shot | 完整全身 |
| 半身镜头 | half body shot, waist-up shot | 半身 |
| 头部特写 | headshot, portrait, face close-up | 头部 |
| 局部特写 | detail shot, insert shot, extreme close-up | 细节 |

### Requirement: 拍摄角度术语映射

系统 SHALL 能够将用户输入的拍摄角度词汇转换为标准术语。

| 用户输入 | 标准术语 | 说明 |
|----------|----------|------|
| 俯视角度 | high angle, from above, bird's eye view, overhead shot | 居高临下 |
| 平视角度 | eye level, straight on, neutral angle | 水平视角 |
| 仰视角度 | low angle, from below, worm's eye view | 仰视 |
| 正面拍摄 | front view, straight-on, facing camera | 正对镜头 |
| 侧面拍摄 | side view, profile, three-quarter view | 侧面 |
| 背面拍摄 | back view, rear view, from behind | 背面 |
| 斜角度 | angled shot, Dutch angle, canted angle | 倾斜角度 |
| 水平角度 | horizontal angle | 水平 |
| 轻微俯视 | slightly from above, gentle high angle | 轻微俯视 |
| 轻微仰视 | slightly from below, heroic angle | 轻微仰视 |

### Requirement: 焦段术语映射

系统 SHALL 能够将用户输入的焦段词汇转换为标准术语。

| 用户输入 | 标准术语 | 说明 |
|----------|----------|------|
| 35mm焦段 | 35mm lens, wide-angle normal | 广角标准 |
| 50mm焦段 | 50mm lens, standard lens, natural perspective | 标准镜头 |
| 75mm焦段 | 75mm lens, portrait telephoto | 人像焦段 |
| 85mm焦段 | 85mm lens, portrait lens, short telephoto | 人像经典 |
| 135mm焦段 | 135mm lens, medium telephoto | 中长焦 |
| 24mm焦段 | 24mm lens, wide-angle | 广角 |
| 广角端 | wide-angle, ultra-wide | 超广角 |
| 长焦端 | telephoto, zoom | 长焦 |

### Requirement: 光圈术语映射

系统 SHALL 能够将用户输入的光圈词汇转换为标准术语。

| 用户输入 | 标准术语 | 说明 |
|----------|----------|------|
| 大光圈 | wide aperture, shallow depth of field, f/1.4-f/2.8 | 背景虚化 |
| 小光圈 | small aperture, deep focus, f/11-f/16 | 前后清晰 |
| 中等光圈 | medium aperture, f/5.6-f/8 | 均衡 |
| 最大光圈 | maximum aperture, wide open | 全开 |
| 收缩光圈 | stopped down, narrow aperture | 收缩 |

### Requirement: 摄影风格术语映射

系统 SHALL 能够将用户输入的摄影风格词汇转换为标准术语。

| 用户输入 | 标准术语 | 说明 |
|----------|----------|------|
| 电影感 | cinematic, film-like, cinematic feel | 电影风格 |
| 纪录片风格 | documentary style, handheld, raw | 真实感 |
| 商业广告 | commercial photography, advertising style | 广告感 |
| 时尚大片 | fashion editorial, high fashion, editorial | 时尚 |
| 复古胶片 | vintage film, retro, analog photography | 复古 |
| 现代简约 | modern minimalist, clean, sleek | 简约 |
| 赛博朋克 | cyberpunk, neon, futuristic | 科幻 |
| 电影胶片 | film grain, cinematic color grading | 电影感 |

---

## MODIFIED Requirements

### Requirement: 词块分类体系扩展

**原分类**: 主体描述、服装描述、场景/地点、动作/行为、构图参数、技术参数、光照/氛围、风格标签、图片引用

**扩展**: 新增摄影术语分类，包含镜头运动、景别、拍摄角度、焦段等

**扩展示例**:
```
升格镜头 特写 仰视角度 50mm焦段 大光圈 电影感
```

---

## 技术约束

1. 摄影术语应与现有术语映射表整合，避免重复
2. 多个摄影术语可以叠加使用（如"大光圈+特写+仰视"）
3. 对于组合表达（如"轻微运镜"），应识别为"轻微"修饰+镜头运动
