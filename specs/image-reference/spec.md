# 图片引用功能扩展规格

## Why

在真实使用场景中，用户进行图生图/图生视频时通常会附加参考图片。这些图片主要分为两类：服装图片（上衣、裤子、套装等）和产品图片（枕头、水杯、小推车、摇椅等）。用户通过在一级提示词中标注来引用这些图片（如"角色身穿图1中的上衣"、"角色手拿图1中的水杯"），但 AI 实际上不需要读取图片内容，只需要根据用户的文字描述在二级提示词中添加相关描述即可。

## What Changes

- **新增图片引用识别**: 在一级提示词中识别用户引用的图片标记（如"图1"、"图2"）
- **新增引用类型分类**: 识别图片属于服装类还是产品类
- **新增引用描述模板**: 根据引用类型生成对应的描述模板
- **更新转换逻辑**: 将图片引用转换为标准描述文本加入二级提示词

## Impact

- Affected specs: prompt-levels 规格
- Affected code: `.trae/skills/prompt-crafter/SKILL.md`

---

## ADDED Requirements

### Requirement: 图片引用识别

系统 SHALL 能够从一级提示词中识别图片引用标记。

**识别模式**:
- "图1" "图2" "图3" 等数字标记
- "图一" "图二" "图三" 等中文数字
- 识别后紧跟的描述性文字

**示例输入**:
```
角色 身穿 图1中的上衣 手拿 图1中的水杯 推着 图2中的小推车 户外 阳光 站立
```

### Requirement: 引用类型分类

系统 SHALL 能够识别图片引用的类型。

| 类型 | 说明 | 引用示例 | 转换后描述模板 |
|------|------|----------|----------------|
| 服装类 | 上衣、裤子、裙子、套装、鞋子、帽子等 | 图1中的上衣 | wearing [图1描述的服装款式] |
| 产品类 | 水杯、枕头、小推车、摇椅、家具等 | 图1中的水杯 | holding [图1描述的产品] |
| 配饰类 | 包包、首饰、手表、眼镜等 | 图1中的包包 | carrying [图1描述的配饰] |

### Requirement: 图片引用转换逻辑

**WHEN** 用户输入包含图片引用的一级提示词
**THEN** 系统执行以下处理：

1. **识别引用标记**: 识别"图X"模式的标记
2. **提取引用描述**: 提取紧跟引用标记的描述词（如"中的上衣"、"中的水杯"）
3. **分类引用类型**: 根据描述词判断是服装类还是产品类
4. **生成引用描述**: 生成格式化的引用描述文本
5. **加入二级提示词**: 将引用描述加入对应的分段（如服装类加入主体/服装描述）

### Requirement: 引用描述模板

**服装类引用描述模板**:
```
wearing [clothing item described in referenced image]
如: wearing a white short-sleeve top from image 1
```

**产品类引用描述模板**:
```
holding/with [product item described in referenced image]
如: holding a blue water cup from image 1
```

**配饰类引用描述模板**:
```
carrying/accessorized with [accessory item described in referenced image]
如: carrying a black leather handbag from image 1
```

### Requirement: 引用标记词汇表

系统 SHALL 维护以下引用标记识别规则：

| 引用标记模式 | 识别示例 | 说明 |
|--------------|----------|------|
| 图 + 数字 | 图1、图2、图3 | 最常见格式 |
| 图 + 中文数字 | 图一、图二、图三 | 中文数字 |
| 照片 + 数字 | 照片1、照片2 | 替代说法 |
| 参考图 + 数字 | 参考图1、参考图2 | 完整说法 |
| IMG + 数字 | IMG1、IMG_2 | 英文格式 |

### Requirement: 关联描述词识别

系统 SHALL 能够识别紧跟引用标记的关联描述词。

| 关联描述词 | 转换为 | 所属类型 |
|------------|--------|----------|
| 中的上衣 | wearing [item] | 服装类 |
| 中的裤子 | wearing [item] | 服装类 |
| 中的裙子 | wearing [item] | 服装类 |
| 中的套装 | wearing [item] | 服装类 |
| 中的鞋子 | wearing [item] | 服装类 |
| 中的水杯 | holding [item] | 产品类 |
| 中的枕头 | with [item] | 产品类 |
| 中的小推车 | pushing [item] | 产品类 |
| 中的摇椅 | sitting on [item] | 产品类 |
| 中的包包 | carrying [item] | 配饰类 |

---

## MODIFIED Requirements

### Requirement: 一级提示词定义扩展

**原定义**: 一级提示词是用户直接输入的、碎片化的词块集合

**扩展**: 一级提示词可包含图片引用标记，用于引用用户附加的参考图片

**扩展示例**:
```
美国 女孩 25岁 身穿 图1中的上衣 手拿 图1中的水杯 推着 图2中的小推车 户外 阳光 站立 横屏
```

### Requirement: 二级提示词输出扩展

**WHEN** 用户输入包含图片引用
**THEN** 系统在二级提示词的对应分段中添加引用描述

**扩展输出格式**:
```markdown
## 📝 二级提示词

### 正面提示词
(主体) young American female, 25 years old, wearing [white short-sleeve top from referenced image 1], pushing [baby stroller from referenced image 2]
(场景) outdoor, sunny day, bright daylight
(构图) full body view, landscape orientation
(技术) sharp focus, natural lighting
(氛围) warm tones, cheerful
(质量) masterpiece, best quality, 8k, highly detailed

### 负面提示词
deformed, blurry, bad anatomy, extra limbs, watermark, text, logo, low quality, worst quality
```

---

## REMOVED Requirements

无

---

## 技术约束

1. AI 不需要实际读取或理解图片内容，仅根据用户文字描述进行转换
2. 图片引用描述应清晰标注来源（如"from referenced image 1"）
3. 多个图片引用应按编号正确对应
4. 如果用户只说"图1"没有后续描述，应保留原引用标记等待用户补充
