# 一级提示词 / 二级提示词 转换系统规格

## Why

当前 AI 生成模型需要结构化、标准化的提示词才能输出高质量结果，但普通用户习惯以碎片化词块形式表达创作意图（无连接词、无语法结构）。现有提示词技能直接让用户编写完整提示词，对用户门槛过高。本系统通过引入「一级提示词 → 二级提示词」的转换层，使普通用户能够以自然词块方式表达创作意图，由系统自动完成标准化转换。

## What Changes

- **新增概念层**: 定义「一级提示词」和「二级提示词」的双层架构
- **新增转换系统**: 实现词块识别、意图理解、标准化输出功能
- **更新 prompt-crafter 技能**: 将现有技能改造为基于双层架构运行

## Impact

- Affected specs: prompt-crafter 技能
- Affected code: `.trae/skills/prompt-crafter/SKILL.md`

---

## ADDED Requirements

### Requirement: 一级提示词定义

一级提示词是用户直接输入的、碎片化的词块集合。

**特征**:
- 由多个独立词块组成
- 词块之间无显式连接词（无 "和"、"以及"、"并且" 等）
- 无语法结构，无主谓宾顺序要求
- 用户按直觉罗列关键元素
- 顺序不敏感

**示例**:
```
美国 白人 女孩 25岁 上身穿纯白色短袖 下身穿浅蓝色牛仔短裤 脚穿凉鞋 夏威夷海滩 沙滩上漫步 横屏 全景景别 50mm镜头 小光圈拍摄 画面清晰不虚焦 户外阳光充足 天气晴朗
```

### Requirement: 二级提示词定义

二级提示词是模型可理解的标准化、结构化输出。

**特征**:
- 包含明确的分段结构（主体、场景、构图、技术、氛围等）
- 使用标准术语和标签
- 包含必要的技术参数
- 可直接被 AI 模型使用
- 包含质量修饰词

**示例**:
```
正面提示词:
(主体) young white American female, 25 years old, wearing white short-sleeve top and light blue denim shorts, sandals, walking on beach
(场景) Hawaii beach, tropical island, sunny day, clear sky, palm trees in background
(构图) panoramic wide shot, 50mm lens, landscape orientation, full body view
(技术) small aperture (f/11-f/16), sharp focus throughout, no motion blur, natural daylight
(氛围) warm tones, bright and cheerful, summer vibes, golden hour lighting
(质量) masterpiece, best quality, 8k, professional photography, highly detailed

负面提示词:
deformed, blurry, bad anatomy, extra limbs, watermark, text, logo, low quality, worst quality, jpeg artifacts
```

### Requirement: 词块识别与分类

系统 SHALL 能够从一级提示词中自动识别以下词块类型：

| 词块类型 | 识别示例 | 说明 |
|----------|----------|------|
| 主体描述 | 美国、白人、女孩、25岁 | 人物/物体核心属性 |
| 服装描述 | 纯白色短袖、浅蓝色牛仔短裤、凉鞋 | 穿着/配饰 |
| 场景/地点 | 夏威夷海滩、沙滩 | 环境位置 |
| 动作/行为 | 漫步、站立、跳舞 | 主体行为 |
| 构图参数 | 横屏、全景景别、50mm镜头 | 画面布局 |
| 技术参数 | 小光圈拍摄、画面清晰不虚焦 | 拍摄技术 |
| 光照/氛围 | 户外阳光充足、天气晴朗 | 光线和情绪 |
| 风格标签 | 电影感、写实、动漫 | 艺术风格 |

### Requirement: 一级到二级转换逻辑

**WHEN** 用户输入一级提示词
**THEN** 系统执行以下转换步骤：
1. **分词**: 将输入字符串按空格分割为词块列表
2. **词块分类**: 识别每个词块属于哪个类型
3. **意图推断**: 基于词块组合推断整体创作意图
4. **结构重组**: 按二级提示词模板重新组织内容
5. **术语标准化**: 将口语词汇转换为标准术语
6. **质量增强**: 自动添加质量标签和负面提示词

### Requirement: 口语到标准术语映射

系统 SHALL 维护以下映射关系：

| 口语词块 | 标准术语 | 所属分类 |
|----------|----------|----------|
| 横屏 | landscape orientation | 构图 |
| 竖屏 | portrait orientation | 构图 |
| 全景景别 | panoramic wide shot / full body view | 构图 |
| 特写 | close-up shot | 构图 |
| 小光圈 | small aperture (f/11-f/16) | 技术 |
| 大光圈 | wide aperture (f/1.4-f/2.8) | 技术 |
| 不虚焦 | sharp focus, no blur | 技术 |
| 阳光充足 | bright daylight, direct sunlight | 光照 |
| 天气晴朗 | clear sky, sunny | 氛围 |
| 漫步 | walking, strolling | 动作 |
| 美女 | beautiful woman, attractive female | 主体 |
| 美国 | American, USA | 主体属性 |

### Requirement: 输出格式规范

**WHEN** 转换完成
**THEN** 系统输出必须遵循以下格式：

```markdown
## 📝 二级提示词

### 正面提示词
(主体) [主体描述]
(场景) [场景描述]
(构图) [构图参数]
(技术) [技术参数]
(氛围) [氛围描述]
(质量) [质量标签]

### 负面提示词
[负面提示词列表]
```

---

## MODIFIED Requirements

### Requirement: prompt-crafter 技能改造

**原需求**: 直接生成完整提示词

**新需求**:
- 接收用户输入的一级提示词（词块形式）
- 自动转换为二级提示词
- 向用户展示转换结果
- 可选：提供多个变体供选择

---

## REMOVED Requirements

无

---

## 技术约束

1. 词块识别应具有一定的容错性，支持同义词识别
2. 对于无法分类的词块，应归入「附加描述」或直接保留原词
3. 转换过程应对用户透明，可选择性展示转换逻辑
4. 系统应能够处理 3-50 个词块的输入
