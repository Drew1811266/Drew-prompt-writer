# 保护性提示词扩展规格

## Why

在用户使用图片引用功能时，生成的提示词需要包含保护性约束，确保 AI 模型的输出符合用户预期：
1. 避免出现肢体错误（多手指、缺手指、融合手指等）
2. 严格参考图片中服装/产品的设计，保持一致
3. 避免常见的 AI 生成缺陷

## What Changes

- **新增保护性提示词模板**: 为图片引用场景添加专门的保护性提示词
- **新增肢体保护规则**: 针对人物主体的肢体完整性保护
- **新增引用一致性规则**: 针对图片引用的服装/产品一致性保护
- **更新 SKILL.md**: 将保护性提示词整合到现有技能中

## Impact

- Affected specs: image-reference
- Affected code: `.trae/skills/prompt-crafter/SKILL.md`

---

## ADDED Requirements

### Requirement: 保护性提示词分类

保护性提示词分为以下几类：

| 类型 | 说明 | 应用场景 |
|------|------|----------|
| 肢体保护 | 防止肢体畸形、多数手指等 | 人物主体时自动添加 |
| 引用一致 | 确保服装/产品与参考图一致 | 使用图片引用时添加 |
| 构图保护 | 防止画面变形、比例失调 | 通用 |
| 质量保护 | 防止低质量输出 | 通用 |

### Requirement: 肢体保护提示词

**WHEN** 用户输入包含人物主体
**THEN** 自动添加以下肢体保护提示词：

```
perfect anatomy, correct number of fingers, no extra fingers,
no fused fingers, no missing fingers, no deformed hands,
proper hand structure, natural pose, correct body proportions,
no limb duplication, no limb fusion, normal limbs,
```

### Requirement: 引用一致性保护提示词

**WHEN** 用户输入包含图片引用
**THEN** 自动添加以下引用一致性保护提示词：

**服装类引用**:
```
strictly follow the design of referenced image [X],
maintain consistent style, color, and pattern with the reference,
do not alter the clothing design from image [X],
keep the exact same garment design as shown,
```

**产品类引用**:
```
strictly follow the product design of referenced image [X],
maintain exact same product appearance, color, and details,
do not modify the product design from image [X],
keep the precise product design as referenced,
```

**配饰类引用**:
```
strictly follow the accessory design of referenced image [X],
maintain exact same accessory appearance and details,
```

### Requirement: 构图保护提示词

**WHEN** 用户进行图生图时
**THEN** 自动添加以下构图保护提示词：

```
correct posture, natural pose, realistic pose,
proper perspective, correct proportions, natural proportions,
no distortion, no deformation, realistic,
```

### Requirement: 质量保护提示词

**WHEN** 用户进行图生图时
**THEN** 自动添加以下质量保护提示词：

```
no artifacts, no jpeg artifacts, no compression artifacts,
clean image, smooth edges, no noise, no grain,
high clarity, clean output, polished result,
```

---

## MODIFIED Requirements

### Requirement: 负面提示词模板扩展

**原负面提示词**:
```
deformed, blurry, bad anatomy, extra limbs, watermark, text, logo, low quality, worst quality
```

**扩展后的负面提示词**:
```
deformed, blurry, bad anatomy, bad proportions,
extra limbs, extra fingers, fused fingers, missing fingers,
deformed hands, unnatural pose, incorrect posture,
ugly, disfigured, poorly drawn face, mutation,
watermark, text, logo, signature,
low quality, worst quality, jpeg artifacts,
monochrome, greyscale, oversaturated,
no artifacts, clean output,
```

---

## REMOVED Requirements

无

---

## 技术约束

1. 保护性提示词根据场景自动选择，无需用户手动添加
2. 图片引用编号需要正确对应（引用图1添加图1的保护词）
3. 多个人物时加强肢体保护提示词
4. 服装/产品引用数量不受限制，但每个引用都需要对应保护
