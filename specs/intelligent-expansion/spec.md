# 智能扩展优化规格

## Why

用户输入的一级提示词通常是碎片化的描述，缺少细节。例如用户只输入"22岁 美国 白人 女孩 长相漂亮"，AI 需要根据这些关键词自动补充更详细的样貌描述（如眼睛颜色、发型、脸型等）；用户只输入"公园中散步"，AI 需要补充更多场景细节（如公园类型、植被、光照、时间等）和动作细节（如步态、姿态、手臂摆动等）。

## What Changes

- **新增智能扩展机制**: 根据用户输入的关键词自动推断并补充细节描述
- **新增样貌扩展**: 当用户描述人物外貌时，自动补充完整样貌特征
- **新增场景扩展**: 当用户描述场景时，自动补充环境细节
- **新增动作扩展**: 当用户描述动作时，自动补充动作细节
- **更新 SKILL.md**: 将智能扩展规则整合到技能描述中

## Impact

- Affected specs: prompt-levels
- Affected code: `.trae/skills/prompt-crafter/SKILL.md`

---

## ADDED Requirements

### Requirement: 智能扩展原则

**扩展时机**:
- 用户输入的描述过于简略时（如只有"美女"而无具体特征）
- 用户描述场景但缺少细节时（如只有"公园"而无具体环境）
- 用户描述动作但缺少动态细节时（如只有"散步"而无具体姿态）

**扩展原则**:
1. **基于关键词推断**: 根据用户提供的关键词推断可能的完整描述
2. **保持一致性**: 扩展内容需与用户原始描述保持风格和属性一致
3. **自然衔接**: 扩展内容应自然融入，不显得突兀
4. **不过度添加**: 扩展应合理，不过度添加用户未暗示的内容

### Requirement: 样貌扩展规则

**WHEN** 用户描述人物外貌但缺少具体特征时
**THEN** AI 自动补充以下样貌细节：

| 用户输入 | 自动扩展为 | 说明 |
|----------|------------|------|
| 女孩 | young female, teenage girl | 补充年龄层 |
| 美女 | beautiful woman, attractive female, pretty face | 补充具体特征 |
| 长相漂亮 | beautiful face, pretty eyes, delicate features, attractive appearance | 补充具体特征 |
| 白人 | white/Caucasian skin tone, fair complexion, European features | 补充肤色和特征 |
| 金发 | blonde hair, golden hair, wavy hair | 补充发型 |
| 黑人 | Black/African skin tone, dark complexion | 补充肤色 |
| 亚洲 | Asian features, East Asian appearance | 补充种族特征 |
| 老人 | elderly, aged face, wrinkles, gray hair | 补充年龄特征 |
| 小孩 | child, young face, youthful features, innocent look | 补充儿童特征 |
| 帅哥 | handsome man, attractive male, chiseled features | 补充男性特征 |
| 长发 | long hair, flowing hair | 补充发型 |
| 短发 | short hair, bob cut | 补充发型 |

**完整样貌扩展模板**:
```
[基础描述],
[面部特征: eyes, nose, lips, face shape],
[发型发色],
[皮肤肤色],
[表情神态],
[气质风格]
```

### Requirement: 场景扩展规则

**WHEN** 用户描述场景但缺少具体环境细节时
**THEN** AI 自动补充以下场景细节：

| 用户输入 | 自动扩展为 | 说明 |
|----------|------------|------|
| 公园 | public park, green grass, trees, benches, walking paths | 补充公园元素 |
| 海滩 | sandy beach, ocean waves, seashells, clear water | 补充海滩元素 |
| 街道 | city street, buildings, storefronts, pedestrians | 补充街道元素 |
| 咖啡厅 | cozy coffee shop, wooden tables, warm lighting, aroma of coffee | 补充咖啡厅元素 |
| 教室 | classroom, desks, chalkboard, sunlight through windows | 补充教室元素 |
| 室内 | indoor setting, interior space, ambient lighting | 补充室内元素 |
| 室外 | outdoor setting, natural lighting, open space | 补充室外元素 |
| 夜晚 | nighttime, moonlight, city lights, stars | 补充夜景元素 |
| 白天 | daytime, sunlight, bright environment | 补充日光元素 |

**完整场景扩展模板**:
```
[主要场景元素],
[背景元素],
[光照条件],
[时间氛围],
[天气环境]
```

### Requirement: 动作扩展规则

**WHEN** 用户描述动作但缺少具体动态细节时
**THEN** AI 自动补充以下动作细节：

| 用户输入 | 自动扩展为 | 说明 |
|----------|------------|------|
| 散步 | walking leisurely, strolling, relaxed pace, gentle stride | 补充步态细节 |
| 站立 | standing, standing straight, confident posture | 补充姿态细节 |
| 坐着 | sitting, seated, relaxed pose | 补充坐姿细节 |
| 跑步 | running, jogger's pose, dynamic movement | 补充跑步细节 |
| 跳舞 | dancing, graceful movements, rhythmic motion | 补充舞蹈细节 |
| 微笑 | smiling, gentle smile, happy expression | 补充表情细节 |
| 看向远方 | gazing into distance, looking afar, thoughtful expression | 补充视线细节 |
| 回头 | looking back, glancing over shoulder, turning motion | 补充回眸细节 |
| 飞 | flying, soaring through the air, arms spread | 补充飞行细节 |
| 飘 | floating, drifting, weightless sensation | 补充漂浮细节 |

**完整动作扩展模板**:
```
[主要动作],
[动作姿态],
[运动状态],
[面部表情],
[身体语言]
```

### Requirement: 扩展示例

**用户输入**:
```
22岁 美国 白人 女孩 长相漂亮
```

**智能扩展后**:
```
young white American female, 22 years old,
beautiful face, pretty eyes, delicate features, attractive appearance,
fair complexion, European features, clean skin,
gentle expression, soft smile, confident yet approachable aura
```

---

**用户输入**:
```
角色在公园中散步
```

**智能扩展后**:
```
character walking leisurely in a public park,
strolling along the walking path, relaxed pace, gentle stride,
green grass on both sides, tall trees providing shade, benches nearby,
natural daylight, soft breeze, peaceful and serene atmosphere
```

---

## 技术约束

1. 扩展内容必须与用户原始描述保持属性一致（如用户说"白人"，扩展不能变成"亚洲人"）
2. 扩展应保持合理的数量，避免过度堆砌
3. 扩展后的总词数仍需控制在 800 词以内
4. 扩展仅在用户输入过于简略时触发，若用户已提供详细描述则不扩展
