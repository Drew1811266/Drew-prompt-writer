# 写实风格统一规格

## Why

用户要求所有生成的图片/视频都统一为类似专业相机或电影机拍摄的写实质感画面。这意味着无论用户输入什么内容，最终输出都应具有专业摄影/ cinematography 的质感特点。

## What Changes

- **统一写实风格**: 所有输出统一为专业相机/电影机拍摄的写实质感
- **定义写实标签**: 明确专业相机和电影机的质感特征标签
- **更新输出模板**: 在质量标签中固定添加写实风格描述
- **更新 SKILL.md**: 将写实风格统一规则整合到技能描述中

## Impact

- Affected specs: 所有现有规格
- Affected code: `.trae/skills/prompt-crafter/SKILL.md`

---

## ADDED Requirements

### Requirement: 写实风格定义

**核心风格**: 专业相机/电影机拍摄的写实质感画面

**特征**:
- 细腻的纹理和细节
- 自然的肤色渲染
- 真实的光影效果
- 适度的动态范围
- 电影感的色调和对比度

### Requirement: 写实风格质量标签

**WHEN** 输出任何类型的提示词时（图生图或图生视频）
**THEN** 必须包含以下写实风格标签：

**图生图质量标签**:
```
professional photography, DSLR quality, mirrorless camera,
sharp focus, high resolution, 8K or 4K,
natural skin tones, realistic textures, detailed fabric,
cinematic color grading, film-like quality,
sharp detail, clean noise profile, professional lens quality
```

**图生视频质量标签**:
```
cinematic film quality, ARRI Alexa style, RED camera quality,
professional cinematography, film grain, cinematic tone,
natural motion blur, cinema-grade color science,
shallow depth of field, anamorphic look
```

### Requirement: 统一风格规则

**WHEN** 生成任何提示词时
**THEN** 遵循以下规则：

1. **固定写实前缀**: 主体描述前添加写实质感描述
2. **自然光优先**: 光照描述优先使用自然光效
3. **真实纹理**: 强调真实材质的纹理细节
4. **电影色调**: 使用电影感的色彩分级

### Requirement: 输出格式修正

**WHEN** 输出二级提示词时
**THEN** 质量分段必须包含写实风格标签

**修正后的质量标签分段模板**:
```
(质量) [用户指定的质量要求], professional photography, DSLR quality, realistic textures, natural skin tones, cinematic color grading, sharp focus, high detail, film-like quality
```

---

## 技术约束

1. 写实风格标签是固定添加的，无论用户是否指定
2. 如果用户指定了其他风格（如动漫、插画），则根据用户设定调整
3. 写实风格标签位于质量分段的末尾
4. 保持与用户输入的一致性，不强制覆盖用户设定
