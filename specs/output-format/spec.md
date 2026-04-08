# 提示词输出格式规格

## Why

用户需要将生成的二级提示词用于实际的 AI 图像/视频生成工具（如 Stable Diffusion、Midjourney 等），这些工具通常接受英文提示词。同时，为了方便用户理解提示词内容，需要提供中文翻译版本作为参考。

## What Changes

- **输出语言**: 二级提示词统一输出为英文
- **词数限制**: 英文提示词控制在 800 词以内
- **中文翻译**: 每条提示词输出后附带中文翻译
- **更新 SKILL.md**: 将输出格式规范整合到技能描述中

## Impact

- Affected specs: 所有现有规格
- Affected code: `.trae/skills/prompt-crafter/SKILL.md`

---

## ADDED Requirements

### Requirement: 输出语言规范

**WHEN** 转换完成并输出二级提示词
**THEN** 所有提示词内容必须为英文

**原因**:
- 大多数 AI 生成模型基于英文训练
- 英文提示词能获得更好的生成效果
- 兼容 Stable Diffusion、Midjourney、Runway 等主流工具

### Requirement: 词数限制规范

**WHEN** 输出英文提示词
**THEN** 英文提示词总词数（按空格分割的单词计数）应控制在 800 词以内

**计算方式**:
```
英文提示词总词数 = 空格分割的单词数量
示例: "beautiful woman portrait" = 3 词
```

**优化策略**:
- 合并相似描述
- 移除冗余修饰词
- 使用缩写替代完整词组（如 CU 替代 close-up）
- 保留核心质量标签

### Requirement: 中文翻译规范

**WHEN** 输出英文提示词后
**THEN** 必须附带中文翻译版本

**翻译原则**:
- 保持结构分段对应
- 专有名词保持原文（如 SD、Midjourney 相关术语）
- 便于用户理解提示词含义

### Requirement: 输出格式模板

**WHEN** 输出最终结果时
**THEN** 必须遵循以下格式：

```markdown
## 📝 二级提示词

### 英文提示词
(主体) [English subject description]
(场景) [English scene description]
(构图) [English composition]
(镜头) [English lens/shot description]
(技术) [English technical parameters]
(光照) [English lighting]
(氛围) [English atmosphere/mood]
(保护) [English protective prompts]
(质量) [English quality tags]

### 负面提示词
[English negative prompts, max 800 words total]

---

### 中文翻译
(主体) [中文主体描述]
(场景) [中文场景描述]
(构图) [中文构图描述]
(镜头) [中文镜头描述]
(技术) [中文技术参数]
(光照) [中文光照描述]
(氛围) [中文氛围描述]
(保护) [中文保护性提示]
(质量) [中文质量标签]

### 负面提示词
[中文负面提示词]
```

---

## 技术约束

1. 词数统计不包括分段标签（如 `(主体)`）
2. 词数统计不包括括号和逗号
3. 缩写词按实际单词计数（如 "WA" 计为 1 词）
4. 质量标签通常占 5-15 词，属于必要部分
5. 如果用户输入非常复杂导致可能超过限制，自动精简非核心描述
