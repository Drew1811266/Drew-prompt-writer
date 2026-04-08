---
name: "Drew-prompt-writer"
description: "专为图生图/图生视频场景设计提示词。触发条件：用户请求书写、优化或转换图像/视频生成提示词。"
---

# Drew-prompt-writer - 提示词书写技能

本技能专为图生图（img2img）和图生视频（img2video）场景设计，帮助用户通过「一级提示词 → 二级提示词」的双层架构快速生成高质量 AI 生成提示词。

## 核心概念

### 什么是一级提示词

一级提示词是用户直接输入的、碎片化的词块集合。

**特征**:
- 由多个独立词块组成，词块之间无显式连接词
- 无语法结构，无主谓宾顺序要求
- 用户按直觉罗列关键元素
- 顺序不敏感
- 可包含图片引用标记
- 可包含摄影术语

**示例**:
```
美国 白人 女孩 25岁 上身穿纯白色短袖 下身穿浅蓝色牛仔短裤 脚穿凉鞋 夏威夷海滩 沙滩上漫步 横屏 全景景别 50mm焦段 小光圈 户外阳光充足 天气晴朗
```

**带摄影术语的示例**:
```
美女 升格镜头 大光圈 85mm焦段 特写 仰视角度 浅景深 电影感
```

**带图片引用的示例**:
```
美国 女孩 25岁 身穿 图1中的上衣 手拿 图1中的水杯 推着 图2中的小推车 户外 阳光 站立 横屏
```

### 什么是二级提示词

二级提示词是模型可理解的标准化、结构化输出。

**示例**:
```
正面提示词:
(主体) young white American female, 25 years old, wearing white short-sleeve top and light blue denim shorts, sandals, walking on beach
(场景) Hawaii beach, tropical island, sunny day, clear sky, palm trees in background
(构图) wide shot, full body view, landscape orientation
(镜头) 50mm lens, standard perspective
(技术) small aperture (f/11-f/16), deep focus, sharp focus throughout
(光照) bright daylight, direct sunlight
(氛围) warm tones, bright and cheerful, summer vibes
(质量) masterpiece, best quality, 8k, professional photography, highly detailed

负面提示词:
deformed, blurry, bad anatomy, extra limbs, watermark, text, logo, low quality, worst quality, jpeg artifacts
```

---

## 适用工具

Stable Diffusion, ComfyUI, Midjourney, DALL-E, Runway, Pika, SVD (Stable Video Diffusion), Kling, Sora 等

---

## 一、使用指南

### 触发条件

当用户出现以下情况时，本技能自动激活：
- 用户以词块形式输入创作需求（一级提示词）
- 请求"帮我生成图片提示词"
- 请求"写一个视频提示词"
- 提供粗糙描述，需要转化为结构化提示词
- 上传参考图，需要描述风格或进行风格迁移
- **用户输入末尾包含暗号 `i`（空格 + i，触发图生图）**
- **用户输入末尾包含暗号 `v`（空格 + v，触发图生视频）**

### 暗号触发规则

| 暗号 | 触发技能 | 说明 |
|------|----------|------|
| `i` 或 `I` | 图生图提示词生成 | 输入末尾加空格 + i |
| `v` 或 `V` | 图生视频提示词生成 | 输入末尾加空格 + v |
| `c` 或 `C` | 提示词整改编辑 | 输入末尾加空格 + c，修改当前提示词 |
| `end` 或 `结束` | 清空上下文记忆 | 结束当前对话，准备新需求 |

**识别示例**:
| 用户输入 | 识别结果 |
|----------|----------|
| `美女 大光圈 85mm焦段 i` | 触发图生图 |
| `女人 转头 微笑 升格镜头 v` | 触发图生视频 |
| `景别改为中景 c` | 触发提示词整改 |
| `场景改为校园 c` | 触发提示词整改 |
| `end` | 清空上下文，开始新对话 |
| `结束` | 清空上下文，开始新对话 |

**暗号处理流程**:
1. 检测输入末尾的暗号字母或结束指令
2. 如果是 `end` 或 `结束`，清空上下文记忆
3. 如果是 `c`，进入整改编辑模式
4. 如果是 `i` 或 `v`，执行标准转换流程

### 响应流程

**当收到用户的一级提示词时**：

1. **识别词块**: 将输入字符串按空格分割为词块列表
2. **分类整理**: 识别每个词块属于哪个类型（主体/服装/场景/动作/构图/技术/光照/风格/摄影）
3. **图片引用检查**: 检查是否有"图X"格式的图片引用标记
4. **摄影术语检查**: 检查是否有镜头运动/景别/角度等摄影术语
5. **智能扩展**: 根据关键词自动补充细节描述（样貌/场景/动作）
6. **意图推断**: 基于词块组合推断整体创作意图
7. **术语转换**: 将口语词汇转换为标准术语
8. **结构重组**: 按二级提示词模板重新组织内容
9. **质量增强**: 自动添加质量标签和负面提示词

### 智能扩展规则

当用户输入描述过于简略时，自动补充细节：

**样貌扩展**（用户描述人物外貌时）:
| 用户输入 | 自动扩展为 |
|----------|------------|
| 女孩 | young female, teenage girl |
| 美女 | beautiful woman, attractive female, pretty face |
| 长相漂亮 | beautiful face, pretty eyes, delicate features |
| 白人 | white/Caucasian skin tone, fair complexion, European features |
| 金发 | blonde hair, golden hair, wavy hair |
| 亚洲 | Asian features, East Asian appearance |
| 老人 | elderly, aged face, wrinkles, gray hair |
| 小孩 | child, young face, youthful features, innocent look |
| 帅哥 | handsome man, attractive male, chiseled features |
| 长发 | long hair, flowing hair |
| 短发 | short hair, bob cut |

**场景扩展**（用户描述场景时）:
| 用户输入 | 自动扩展为 |
|----------|------------|
| 公园 | public park, green grass, trees, benches, walking paths |
| 海滩 | sandy beach, ocean waves, seashells, clear water |
| 街道 | city street, buildings, storefronts, pedestrians |
| 咖啡厅 | cozy coffee shop, wooden tables, warm lighting |
| 教室 | classroom, desks, chalkboard, sunlight through windows |
| 夜晚 | nighttime, moonlight, city lights, stars |
| 白天 | daytime, sunlight, bright environment |

**动作扩展**（用户描述动作时）:
| 用户输入 | 自动扩展为 |
|----------|------------|
| 散步 | walking leisurely, strolling, relaxed pace, gentle stride |
| 站立 | standing, standing straight, confident posture |
| 坐着 | sitting, seated, relaxed pose |
| 跑步 | running, jogger's pose, dynamic movement |
| 跳舞 | dancing, graceful movements, rhythmic motion |
| 微笑 | smiling, gentle smile, happy expression |
| 看向远方 | gazing into distance, looking afar, thoughtful expression |
| 回头 | looking back, glancing over shoulder, turning motion |
| 飞 | flying, soaring through the air, arms spread |
| 飘 | floating, drifting, weightless sensation |

**扩展示例**:
- 输入: `22岁 美国 白人 女孩 长相漂亮` → 扩展: `young white American female, 22 years old, beautiful face, pretty eyes, delicate features, fair complexion, gentle expression`
- 输入: `公园中散步` → 扩展: `walking leisurely in public park, strolling along path, green grass, tall trees, natural daylight`

### 导演思维创作

AI 在创作提示词时应像导演一样主动思考画面，将简单的描述打造成完整的、有故事感的画面。

**导演创作维度**:

| 创作维度 | 说明 | 示例 |
|----------|------|------|
| 光影设计 | 主动设计光线方向、色温、对比度 | golden hour sunlight, dramatic shadows, rim light |
| 色彩基调 | 主动设定整体色彩风格 | warm tones, cool palette, high contrast |
| 情绪氛围 | 主动营造画面情绪 | nostalgic, peaceful, mysterious, energetic |
| 环境细节 | 主动添加环境元素 | wind, grass, water reflections, particles |
| 镜头语言 | 主动设定视角和距离 | wide establishing shot, close-up detail |
| 故事感 | 主动添加叙事元素 | catching a moment, in mid-action |

**创作约束**:
- 不违背用户设定：所有创作必须与用户的一级提示词保持一致
- 不添加冲突元素：如用户设定"夜晚"就不能添加"阳光"
- 合理扩展：扩展内容应符合场景逻辑
- 用户可控：用户描述详细时不过度创作

**创作示例**:

用户输入: `女孩 公园 散步`

被动转换: `young girl walking in park`

导演思维创作后:
```
young girl, red dress, blonde curly hair, walking leisurely through sunlit park,
golden hour sunlight casting warm glow through autumn leaves,
catches a falling maple leaf, curious expression, gentle smile,
background of oak trees, green grass, stone bench nearby,
cinematic atmosphere, nostalgic and peaceful mood,
wide establishing shot, depth of field, soft bokeh
```

**创作时机**:

| 用户输入复杂度 | AI 创作程度 |
|----------------|-------------|
| 只有主体描述 | 全面创作，补充场景、氛围、光影 |
| 主体+场景 | 重点创作氛围，光影、动态细节 |
| 主体+场景+动作 | 适度创作，补充光影、环境细节 |
| 描述详细（含摄影术语） | 最小创作，保持用户设定为主 |

### 输出格式

转换完成后，按以下格式输出：

**重要**: 英文提示词段落必须完全使用英文，不得包含任何中文内容。图片引用使用英文括号和英文描述。

**风格统一**: 所有输出统一为专业相机/电影机拍摄的写实质感画面，包含细腻纹理、自然肤色、真实光影效果。

```markdown
## 📝 二级提示词

### 英文提示词
(主体) [English subject description, image references use format: item from image X]
(场景) [English scene description]
(构图) [English composition/framing]
(镜头) [English lens/shot description]
(技术) [English technical parameters]
(光照) [English lighting description]
(氛围) [English atmosphere/mood]
(保护) [English protective prompts]
(质量) [user specified quality], professional photography, DSLR quality, realistic textures, natural skin tones, cinematic color grading, sharp focus, high detail, film-like quality

### 负面提示词
[English negative prompts]

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
(质量) [用户指定质量], 专业摄影, DSLR画质, 写实纹理, 自然肤色, 电影色调, 清晰对焦, 高细节, 胶片质感

### 负面提示词
[中文负面提示词]
```

**图片引用格式**: `wearing [jeans from image 1]` 而非 `wearing [参考图1中的牛仔裤]`

**词数限制**: 英文提示词控制在 800 词以内（按空格分割的单词计数）

**写实风格标签**:
- 图生图: `professional photography, DSLR quality, mirrorless camera, realistic textures, natural skin tones, cinematic color grading, film-like quality`
- 图生视频: `cinematic film quality, ARRI Alexa style, RED camera quality, professional cinematography, film grain, cinematic tone`
```

---

## 二、图片引用功能

### 图片引用识别

系统能够从一级提示词中识别图片引用标记。

**识别模式**:

| 引用模式 | 示例 | 说明 |
|----------|------|------|
| 图 + 数字 | 图1、图2、图3 | 最常见格式 |
| 图 + 中文数字 | 图一、图二、图三 | 中文数字 |
| 照片 + 数字 | 照片1、照片2 | 替代说法 |
| 参考图 + 数字 | 参考图1、参考图2 | 完整说法 |
| IMG + 数字 | IMG1、IMG_2 | 英文格式 |

### 引用类型分类

| 类型 | 说明 | 关联描述词 | 转换模板 |
|------|------|------------|----------|
| 服装类 | 上衣、裤子、裙子、套装、鞋子等 | 中的上衣、中的裤子、中的裙子、中的套装、中的鞋子、中的帽子 | wearing [item] from image X |
| 产品类 | 水杯、枕头、小推车、摇椅、家具等 | 中的水杯、中的枕头、中的小推车、中的摇椅、中的家具 | holding/with [item] from image X |
| 配饰类 | 包包、首饰、手表、眼镜等 | 中的包包、中的首饰、中的手表、中的眼镜 | carrying [item] from image X |

### 关联描述词识别表

| 关联描述词 | 转换为 | 所属类型 |
|------------|--------|----------|
| 中的上衣 | wearing [item] | 服装类 |
| 中的裤子 | wearing [item] | 服装类 |
| 中的裙子 | wearing [item] | 服装类 |
| 中的套装 | wearing [item] | 服装类 |
| 中的鞋子 | wearing [item] | 服装类 |
| 中的袜子 | wearing [item] | 服装类 |
| 中的帽子 | wearing [item] | 服装类 |
| 中的外套 | wearing [item] | 服装类 |
| 中的水杯 | holding [item] | 产品类 |
| 中的枕头 | with [item] | 产品类 |
| 中的小推车 | pushing [item] | 产品类 |
| 中的摇椅 | sitting on [item] | 产品类 |
| 中的椅子 | sitting on [item] | 产品类 |
| 中的桌子 | with [item] | 产品类 |
| 中的包包 | carrying [item] | 配饰类 |
| 中的首饰 | accessorized with [item] | 配饰类 |
| 中的手表 | wearing [item] | 配饰类 |
| 中的眼镜 | wearing [item] | 配饰类 |

### 图片引用转换示例

**一级提示词输入**:
```
美国 女孩 25岁 身穿 图1中的上衣 手拿 图1中的水杯 推着 图2中的小推车 户外 阳光 站立 横屏
```

**图片引用转换过程**:
1. 识别到 `图1` → 关联描述 `中的上衣` → 类型：服装类 → `wearing [top from image 1]`
2. 识别到 `图1` → 关联描述 `中的水杯` → 类型：产品类 → `holding [water cup from image 1]`
3. 识别到 `图2` → 关联描述 `中的小推车` → 类型：产品类 → `pushing [stroller from image 2]`

**二级提示词输出**:

### 英文提示词
(主体) young American female, 25 years old, wearing [white short-sleeve top from image 1], holding [blue water cup from image 1], pushing [gray baby stroller from image 2]
(场景) outdoor, sunny day, bright daylight
(构图) full body view, landscape orientation
(技术) sharp focus, natural lighting
(氛围) warm tones, cheerful, natural
(保护) (strictly follow the design of image 1, maintain consistent style, color, and pattern), (strictly follow the product design of image 2, maintain exact same appearance)
(质量) masterpiece, best quality, 8k, highly detailed, realistic

### 负面提示词
deformed, blurry, bad anatomy, bad proportions, extra limbs, extra fingers, fused fingers, missing fingers, deformed hands, unnatural pose, incorrect posture, ugly, disfigured, poorly drawn face, mutation, watermark, text, logo, signature, low quality, worst quality, jpeg artifacts, monochrome, greyscale, oversaturated, no artifacts, clean output

---

### 中文翻译
(主体) 年轻美国女性，25岁，穿着参考图1中的白色短袖上衣，手拿参考图1中的蓝色水杯，推着参考图2中的灰色婴儿车
(场景) 户外，晴天，明亮日光
(构图) 全身景别，横屏
(技术) 清晰对焦，自然光
(氛围) 暖色调，愉快，自然
(保护) 严格遵循参考图1的设计，保持一致的风格、颜色和图案，严格遵循参考图2的产品设计，保持完全相同的外观
(质量) 杰作，最高画质，8K，高细节，真实

### 负面提示词
变形，模糊，解剖结构错误，比例失调，多余肢体，多余手指，融合手指，缺失手指，变形手，姿势不自然，姿态不正确，丑陋，面部毁容，面部绘制差，突变，水印，文字，标志，签名，低质量，最差质量，jpeg伪影，单色，灰度，过饱和，无伪影，干净输出

### 重要说明

1. **AI 不需要读取图片内容**：仅根据用户的文字描述进行转换
2. **图片引用描述标注来源**：使用 "from referenced image X" 格式
3. **多个同一图片引用**：可以多次引用同一张图片的不同元素
4. **未完整描述时保留原标记**：如果用户只说"图1"没有后续描述，保留原标记等待补充

### 保护性提示词

当用户使用图片引用时，系统自动添加以下保护性提示词：

**肢体保护（人物主体时添加）**:
```
perfect anatomy, correct number of fingers, no extra fingers,
no fused fingers, no missing fingers, no deformed hands,
proper hand structure, natural pose, correct body proportions,
```

**引用一致性保护**:
```
(strictly follow the design of referenced image 1, maintain consistent style, color, and pattern),
(strictly follow the product design of referenced image 2, maintain exact same appearance),
```

**构图/质量保护**:
```
correct posture, natural pose, realistic pose,
proper perspective, correct proportions,
no artifacts, clean image, smooth edges,
```

---

## 三、词块分类体系

### 词块类型总览

| 类型 | 说明 | 示例 |
|------|------|------|
| 主体描述 | 人物/物体核心属性 | 美国、白人、女孩、25岁、美女 |
| 服装描述 | 穿着/配饰 | 纯白色短袖、浅蓝色牛仔短裤、凉鞋 |
| 场景/地点 | 环境位置 | 夏威夷海滩、沙滩、咖啡厅 |
| 动作/行为 | 主体行为 | 漫步、站立、跳舞、微笑 |
| 构图/景别 | 画面布局、景别 | 横屏、特写、全景、近景、中景 |
| 镜头/焦段 | 焦段参数 | 50mm焦段、85mm镜头、广角 |
| 技术参数 | 拍摄技术 | 小光圈、大光圈、对焦 |
| 光照/氛围 | 光线和情绪 | 阳光充足、天气晴朗、柔光 |
| 风格标签 | 艺术风格 | 电影感、写实、动漫 |
| 摄影术语 | 镜头运动、角度 | 升格镜头、推镜头、仰视角度 |
| 图片引用 | 参考图片标记 | 图1中的上衣、图2中的水杯 |

---

## 四、口语到标准术语映射

### 摄影术语 - 镜头运动

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

### 摄影术语 - 景别

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

### 摄影术语 - 拍摄角度

| 用户输入 | 标准术语 | 说明 |
|----------|----------|------|
| 俯视角度 | high angle, from above, bird's eye view, overhead shot | 居高临下 |
| 平视角度 | eye level, straight on, neutral angle | 水平视角 |
| 仰视角度 | low angle, from below, worm's eye view | 仰视 |
| 正面拍摄 | front view, straight-on, facing camera | 正对镜头 |
| 侧面拍摄 | side view, profile, three-quarter view | 侧面 |
| 背面拍摄 | back view, rear view, from behind | 背面 |
| 斜角度 | angled shot, Dutch angle, canted angle | 倾斜角度 |
| 轻微俯视 | slightly from above, gentle high angle | 轻微俯视 |
| 轻微仰视 | slightly from below, heroic angle | 轻微仰视 |

### 摄影术语 - 焦段

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

### 摄影术语 - 光圈与景深

| 用户输入 | 标准术语 | 说明 |
|----------|----------|------|
| 大光圈 | wide aperture, shallow depth of field, f/1.4-f/2.8 | 背景虚化 |
| 小光圈 | small aperture, deep focus, f/11-f/16 | 前后清晰 |
| 中等光圈 | medium aperture, f/5.6-f/8 | 均衡 |
| 浅景深 | shallow depth of field, bokeh, blurry background | 虚化背景 |
| 深景深 | deep focus, everything in sharp focus | 全部清晰 |

### 摄影术语 - 风格

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

### 构图类

| 口语词块 | 标准术语 |
|----------|----------|
| 横屏 | landscape orientation |
| 竖屏 | portrait orientation |
| 正面 | front view |
| 侧面 | side view |
| 背面 | back view |

### 光照/氛围类

| 口语词块 | 标准术语 |
|----------|----------|
| 阳光充足 | bright daylight, direct sunlight |
| 天气晴朗 | clear sky, sunny, cloudless |
| 阴天 | overcast, diffused light, soft lighting |
| 傍晚 | dusk, twilight, golden hour |
| 夜晚 | night, nighttime, moonlight |
| 强光 | harsh lighting, strong contrast |
| 柔光 | soft lighting, diffused light |
| 逆光 | backlit, rim light, silhouette |
| 氛围感 | moody atmosphere, atmospheric |

### 动作类

| 口语词块 | 标准术语 |
|----------|----------|
| 漫步 | walking, strolling |
| 站立 | standing, standing pose |
| 坐着 | sitting, seated |
| 跑步 | running, jogging |
| 跳舞 | dancing |
| 微笑 | smiling, gentle smile |
| 看向远方 | looking into distance, gazing |
| 回眸 | looking back, glancing over shoulder |
| 飞 | flying, soaring |
| 飘 | floating, drifting |

### 主体属性类

| 口语词块 | 标准术语 |
|----------|----------|
| 美女 | beautiful woman, attractive female |
| 帅哥 | handsome man, attractive male |
| 欧美 | Western, Caucasian, European |
| 亚洲 | Asian, East Asian |
| 黑人 | African, Black |
| 老人 | elderly, old man/woman, senior |
| 小孩 | child, little girl/boy, kid |
| 婴儿 | baby, infant |

---

## 五、转换示例

### 示例 1：用户输入（无图片引用）

**一级提示词**:
```
美国 白人 女孩 25岁 上身穿纯白色短袖 下身穿浅蓝色牛仔短裤 脚穿凉鞋 夏威夷海滩 沙滩上漫步 横屏 全景 50mm焦段 小光圈 户外阳光充足 天气晴朗
```

**二级提示词**:

### 英文提示词
(主体) young white American female, 25 years old, wearing white short-sleeve top and light blue denim shorts, sandals
(场景) Hawaii beach, tropical island, sunny day, clear sky, palm trees in background, ocean shore
(动作) walking on beach, strolling along the shore
(构图) wide shot, full body view, landscape orientation
(镜头) 50mm lens, standard lens, natural perspective
(技术) small aperture (f/11-f/16), deep focus, sharp focus throughout
(光照) bright daylight, direct sunlight
(氛围) warm tones, bright and cheerful, summer vibes
(保护) perfect anatomy, correct number of fingers, no extra fingers, no deformed hands, proper hand structure
(质量) masterpiece, best quality, 8k, professional photography, highly detailed, realistic

### 负面提示词
deformed, blurry, bad anatomy, bad proportions, extra limbs, extra fingers, fused fingers, missing fingers, deformed hands, unnatural pose, incorrect posture, ugly, disfigured, poorly drawn face, mutation, watermark, text, logo, signature, low quality, worst quality, jpeg artifacts, monochrome, greyscale, oversaturated, no artifacts, clean output

---

### 中文翻译
(主体) 年轻白人女性，25岁，穿着白色短袖上衣和浅蓝色牛仔短裤，脚穿凉鞋
(场景) 夏威夷海滩热带岛屿，晴天，晴朗天空，棕榈树背景，海岸
(动作) 在海滩上行走，沿着海岸漫步
(构图) 宽镜头，全身景别，横屏
(镜头) 50mm镜头，标准镜头，自然视角
(技术) 小光圈(f/11-f/16)，深景深，全程清晰对焦
(光照) 明亮日光，直射阳光
(氛围) 暖色调，明亮愉快，夏日氛围
(保护) 完美解剖结构，手指数量正确，无多余手指，无变形手，正确手部结构
(质量) 杰作，最高画质，8K，专业摄影，高细节，真实

### 负面提示词
变形，模糊，解剖结构错误，比例失调，多余肢体，多余手指，融合手指，缺失手指，变形手，姿势不自然，姿态不正确，丑陋，面部毁容，面部绘制差，突变，水印，文字，标志，签名，低质量，最差质量，jpeg伪影，单色，灰度，过饱和，无伪影，干净输出

---

### 示例 2：用户输入（带摄影术语）

**一级提示词**:
```
美女 升格镜头 大光圈 85mm焦段 特写 仰视角度 浅景深 电影感
```

**二级提示词**:

### 英文提示词
(主体) beautiful woman, attractive female, elegant pose, stunning features
(构图) close-up shot, tight frame, CU
(镜头) 85mm lens, portrait lens, short telephoto
(技术) wide aperture (f/1.4-f/2.8), shallow depth of field, bokeh
(角度) low angle, from below, heroic angle
(氛围) cinematic, film-like, dramatic, heroic mood
(速度) slow motion, high frame rate (60fps+), cinematic slow motion
(保护) perfect anatomy, correct number of fingers, no extra fingers, no deformed hands, proper hand structure
(质量) masterpiece, best quality, 8k, professional photography, highly detailed, cinematic film grain

### 负面提示词
deformed, blurry, bad anatomy, bad proportions, extra limbs, extra fingers, fused fingers, missing fingers, deformed hands, unnatural pose, incorrect posture, ugly, disfigured, poorly drawn face, mutation, watermark, text, logo, signature, low quality, worst quality, jpeg artifacts, monochrome, greyscale, oversaturated, no artifacts, clean output

---

### 中文翻译
(主体) 美丽女性，有吸引力的女性，优雅姿势，惊艳特征
(构图) 特写镜头，紧凑画面
(镜头) 85mm镜头，人像镜头，短焦
(技术) 大光圈(f/1.4-f/2.8)，浅景深，背景虚化
(角度) 低角度，从下方看，英雄角度
(氛围) 电影感，胶片感，戏剧性，英雄气概
(速度) 慢动作，高帧率(60fps+)，电影级慢动作
(保护) 完美解剖结构，手指数量正确，无多余手指，无变形手，正确手部结构
(质量) 杰作，最高画质，8K，专业摄影，高细节，电影胶片颗粒

### 负面提示词
变形，模糊，解剖结构错误，比例失调，多余肢体，多余手指，融合手指，缺失手指，变形手，姿势不自然，姿态不正确，丑陋，面部毁容，面部绘制差，突变，水印，文字，标志，签名，低质量，最差质量，jpeg伪影，单色，灰度，过饱和，无伪影，干净输出

---

### 示例 3：用户输入（带图片引用）

**一级提示词**:
```
亚洲 女孩 22岁 身穿 图1中的连衣裙 手拿 图2中的奶茶 坐在 图3中的椅子上 咖啡厅 午后 柔光 侧拍
```

**二级提示词**:

### 英文提示词
(主体) young Asian female, 22 years old, wearing [floral print dress from referenced image 1]
(动作) sitting on chair, holding [bubble tea from referenced image 2]
(场景) cozy coffee shop, afternoon light, warm interior
(构图) side view, medium shot, centered composition
(技术) soft diffused lighting, shallow depth of field, natural tones
(氛围) relaxed, peaceful, warm atmosphere, bokeh background
(保护) (strictly follow the design of referenced image 1, maintain consistent style, color, and pattern), (strictly follow the product design of referenced image 2, maintain exact same appearance)
(质量) masterpiece, best quality, 8k, highly detailed, realistic

### 负面提示词
deformed, blurry, bad anatomy, bad proportions, extra limbs, extra fingers, fused fingers, missing fingers, deformed hands, unnatural pose, incorrect posture, ugly, disfigured, poorly drawn face, mutation, watermark, text, logo, signature, low quality, worst quality, jpeg artifacts, monochrome, greyscale, oversaturated, no artifacts, clean output

---

### 中文翻译
(主体) 年轻亚洲女性，22岁，穿着参考图1中的印花连衣裙
(动作) 坐在椅子上，手拿参考图2中的奶茶
(场景) 温馨咖啡厅，午后光线，温暖室内
(构图) 侧面，中景，居中构图
(技术) 柔和散射光，浅景深，自然色调
(氛围) 放松，平静，温暖氛围，背景虚化
(保护) 严格遵循参考图1的设计，保持一致的风格、颜色和图案，严格遵循参考图2的产品设计，保持完全相同的外观
(质量) 杰作，最高画质，8K，高细节，真实

### 负面提示词
变形，模糊，解剖结构错误，比例失调，多余肢体，多余手指，融合手指，缺失手指，变形手，姿势不自然，姿态不正确，丑陋，面部毁容，面部绘制差，突变，水印，文字，标志，签名，低质量，最差质量，jpeg伪影，单色，灰度，过饱和，无伪影，干净输出

---

### 示例 4：用户输入（暗号触发 - 图生图）

**用户输入**:
```
美国 女孩 25岁 身穿 图1中的连衣裙 手拿 图2中的奶茶 咖啡厅 柔光 侧拍 i
```

**处理说明**: 检测到末尾暗号 `i`，自动触发图生图提示词生成技能

**二级提示词**:

### 英文提示词
(主体) young American female, 25 years old, wearing [floral dress from image 1], holding [bubble tea from image 2]
(场景) cozy coffee shop, warm interior, afternoon light
(构图) side view, medium shot, centered composition
(技术) soft diffused lighting, shallow depth of field, natural tones
(氛围) relaxed, peaceful, warm atmosphere, bokeh background
(保护) (strictly follow the design of image 1, maintain consistent style, color, and pattern), (strictly follow the product design of image 2, maintain exact same appearance)
(质量) masterpiece, best quality, 8k, highly detailed, realistic

### 负面提示词
deformed, blurry, bad anatomy, bad proportions, extra limbs, extra fingers, fused fingers, missing fingers, deformed hands, unnatural pose, incorrect posture, ugly, disfigured, poorly drawn face, mutation, watermark, text, logo, signature, low quality, worst quality, jpeg artifacts, monochrome, greyscale, oversaturated, no artifacts, clean output

---

### 中文翻译
(主体) 年轻美国女性，25岁，穿着参考图1中的连衣裙，手拿参考图2中的奶茶
(场景) 温馨咖啡厅，温暖室内，午后光线
(构图) 侧面，中景，居中构图
(技术) 柔和散射光，浅景深，自然色调
(氛围) 放松，平静，温暖氛围，背景虚化
(保护) 严格遵循参考图1的设计，保持一致的风格、颜色和图案，严格遵循参考图2的产品设计，保持完全相同的外观
(质量) 杰作，最高画质，8K，高细节，真实

### 负面提示词
变形，模糊，解剖结构错误，比例失调，多余肢体，多余手指，融合手指，缺失手指，变形手，姿势不自然，姿态不正确，丑陋，面部毁容，面部绘制差，突变，水印，文字，标志，签名，低质量，最差质量，jpeg伪影，单色，灰度，过饱和，无伪影，干净输出

---

### 示例 5：用户输入（暗号触发 - 图生视频）

**用户输入**:
```
女人 转头 微笑 升格镜头 特写 柔光 5秒 v
```

**处理说明**: 检测到末尾暗号 `v`，自动触发图生视频提示词生成技能

**二级提示词**:

### 英文提示词
(主体) beautiful woman, elegant pose, natural makeup
(起始状态) facing slightly away from camera
(动作) slow turn to camera, soft gentle smile, eyes slowly opening, slight head tilt
(构图) close-up shot, tight frame, centered composition
(技术) soft diffused lighting, shallow depth of field, cinematic 24fps
(速度) slow motion, high frame rate (60fps+), cinematic slow motion
(时长) 5 seconds duration
(氛围) dreamy, romantic, tender emotion, bokeh background
(质量) masterpiece, best quality, cinematic, film grain

### 负面提示词
jump cut, cut, abrupt transition, shakey, motion blur, low fps, stuttering, deformed, unnatural movement, robotic motion

---

### 中文翻译
(主体) 美丽女性，优雅姿势，自然妆容
(起始状态) 稍微侧对镜头
(动作) 缓慢转向镜头，柔和微笑，眼睛缓慢睁开，轻微头部倾斜
(构图) 特写镜头，紧凑画面，居中构图
(技术) 柔和散射光，浅景深，电影24fps
(速度) 慢动作，高帧率(60fps+)，电影级慢动作
(时长) 5秒
(氛围) 梦幻，浪漫，温柔情感，背景虚化
(质量) 杰作，最高画质，电影感，胶片颗粒

### 负面提示词
跳切，剪切，突然转场，抖动，运动模糊，低帧率，卡顿，变形，不自然运动，机械运动

---

## 六、图生视频扩展

### 视频特有词块类型

| 类型 | 说明 | 示例 |
|------|------|------|
| 镜头运动 | 相机移动方式 | 推进、拉远、摇镜、环绕 |
| 主体运动 | 主体动作变化 | 转身、走路、表情变化 |
| 时长 | 视频长度 | 5秒、10秒 |
| 帧率 | 画面流畅度 | 24fps、30fps、60fps |

### 镜头运动映射

| 口语词块 | 标准术语 |
|----------|----------|
| 推进 | zoom in, push in, dolly in |
| 拉远 | zoom out, pull back, dolly out |
| 环绕 | orbit, 360 degree rotation, revolving |
| 摇镜 | pan left, pan right, tilt up, tilt down |
| 跟拍 | tracking shot, following shot |
| 固定 | static, fixed frame, locked off |
| 手持 | handheld, documentary style |

### 视频转换示例

**一级提示词**:
```
女人 转头 微笑 升格镜头 特写 柔光 5秒
```

**二级提示词**:
```
正面提示词:
(主体) beautiful woman, elegant pose, natural makeup
(起始状态) facing slightly away from camera
(动作) slow turn to camera, soft gentle smile, eyes slowly opening, slight head tilt
(构图) close-up shot, tight frame, centered composition
(技术) soft diffused lighting, shallow depth of field, cinematic 24fps
(速度) slow motion, high frame rate (60fps+), cinematic slow motion
(时长) 5 seconds duration
(氛围) dreamy, romantic, tender emotion, bokeh background
(质量) masterpiece, best quality, cinematic, film grain

负面提示词:
jump cut, cut, abrupt transition, shakey, motion blur, low fps, stuttering, deformed, unnatural movement, robotic motion
```

### 分镜头创作功能

当用户输入包含分镜头指令时（如"设计三个镜头"、"设计两个镜头（特写、全景）"），AI 自动发挥导演思维，帮助用户设计符合叙事的好莱坞专业分镜头脚本。

**分镜头识别规则**:

| 指令模式 | 示例 | 说明 |
|----------|------|------|
| 设计 + 数量 + 镜头 | 设计三个镜头、设计两个镜头 | 仅指定数量 |
| 设计 + 数量 + 镜头（类型） | 设计两个镜头（特写、全景） | 指定数量和类型 |
| 生成 + 数量 + 镜头 | 生成三个镜头 | 替代说法 |

**机位类型**:

| 机位 | 说明 | 英文 |
|------|------|------|
| 平视 | 相机与主体视线齐平 | Eye level |
| 俯视 | 相机从高处向下拍 | High angle, Bird's eye |
| 仰视 | 相机从低处向上拍 | Low angle, Worm's eye |
| 正面 | 相机正对主体 | Front view, Straight-on |
| 侧面 | 相机与主体侧面成90度 | Side view, Profile |
| 背面 | 相机在主体背后 | Back view, Rear |
| 斜角度 | 相机与主体成45度 | Dutch angle, Canted |

**景别定义**:

| 景别 | 说明 | 英文 |
|------|------|------|
| 极远景 | 建立大环境 | Extreme Wide Shot (EWS) |
| 远景 | 交代场景 | Wide Shot (WS), Establishing Shot |
| 全景 | 显示全部主体 | Full Shot (FS) |
| 中全景 | 膝盖以上 | Medium Full Shot (MFS) |
| 中景 | 腰部以上 | Medium Shot (MS) |
| 近景 | 胸部以上 | Close-up (CU) |
| 特写 | 局部细节 | Extreme Close-up (ECU) |

**相机运动**:

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

**角色动作描述维度**:

| 维度 | 说明 | 示例 |
|------|------|------|
| 姿态 | 身体的整体姿势 | standing, sitting, crouching |
| 位置 | 身体在空间中的位置 | center frame, left third, foreground |
| 方向 | 身体面对的方向 | facing camera, profile left, back to camera |
| 动作 | 具体的行为 | walking, turning, reaching |
| 表情 | 面部的情绪表达 | smiling, looking away, eyes widening |
| 细节 | 手部、眼部等细节 | fingers touching chin, eyes glancing |

**构图原则**:

| 原则 | 说明 | 应用 |
|------|------|------|
| 三分法 | 将画面横竖各分三等份 | 主体放在交叉点上 |
| 头部空间 | 眼睛方向留出空间 | Looking right时，右边留白 |
| 负空间 | 利用空白区域 | 增强情绪表达 |
| 前景/背景 | 画面层次 | 前景物体增加深度 |
| 视线引导 | 引导观众视线 | 沿对角线放置元素 |

**分镜头格式要求**:

每个分镜头以一整段连贯文字输出，包含机位、景别、运动、画面内容、角色动作、构图、光照等所有信息。所有分镜头并入总提示词中作为镜头描述部分。

**分镜头段落示例**:
```
(分镜头1) SHOT 01, Eye level / Wide, Wide Shot (WS) establishing environment, Static camera with slight subtle tracking, teaching building exterior with sunny campus atmosphere, boy on skateboard approaching from left side, standing on skateboard with one foot pushing off ground preparing to glide, casual confident stance looking forward, rule of thirds composition with subject on left third and building on right providing depth, natural daylight with soft shadows, 4-5 seconds duration, slow motion 60fps+

(分镜头2) SHOT 02, Eye level / Side, Medium Shot (MS) knee-up framing, slow subtle tracking following subject's skateboarding motion, boy gliding past teaching building with white short-sleeve top catching breeze and blue denim jeans from referenced image 1 visible, smooth skating motion with arms slightly balanced outward and one hand in pocket, slight Dutch angle enhancing dynamic feeling, golden hour sunlight with key light from upper right and rim light on hair, 5-6 seconds duration, slow motion with slight camera movement

(分镜头3) SHOT 03, Eye level / Side-low angle, Close-up (CU) waist-up framing, static camera with slight breathing movement acceptable, intimate close-up of boy's face and upper body with confident smile forming and blonde hair slightly messy from activity, eyes focused ahead with subtle head tilt and gentle smile, face following rule of thirds with shallow depth of field emphasizing expressive face, soft frontal key light with subtle catch lights in eyes and warm tones, 3-4 seconds duration, cinematic film-like quality
```

**分镜头创作示例**:

用户输入: `女人 转头 微笑 设计三个镜头 v`

```
(主体) beautiful woman, elegant pose, natural makeup, turning motion
(场景) interior with large window, soft golden hour light, warm tones
(构图) 16:9 aspect ratio, cinematic framing
(分镜头1) SHOT 01, Eye level / Wide, Wide Shot (WS) establishing environment, Static camera with tripod mounted, woman standing by large window with soft golden hour light streaming through curtains and elegant interior with warm tones, standing in profile with serene expression and slight sway beginning to turn toward camera, rule of thirds composition with subject on left third and window providing context and depth, golden hour sunlight through window with warm tones and soft shadows, 4-5 seconds duration, slow motion 60fps+, professional cinematography

(分镜头2) SHOT 02, Eye level / Front, Medium Shot (MS) waist-up framing, slow dolly in tracking with subject's turning motion, woman turning toward camera with hair cascading with movement and gentle flowing dress, smooth turning motion with长发随风飘动 and eyes meeting camera lens and soft gentle smile forming, center framed with slight breathing room above head and tracking movement left to right, Rembrandt-style key light from window with soft fill on opposite side and glowing rim light on hair, 5-6 seconds duration, slow motion with subtle camera movement, shallow depth of field, bokeh background

(分镜头3) SHOT 03, Eye level / Straight-on, Close-up (CU) face and upper chest framing, static camera with slight natural breathing movement acceptable, intimate close-up of woman's face with captivating smile and eyes with subtle sparkle, gentle smile fully developed with eyes opening slightly wider and subtle head tilt looking directly into lens, face following rule of thirds with eyes on upper horizontal line and slight negative space below chin, soft frontal key light with subtle catch lights in eyes and shallow depth of field emphasizing eyes, 3-4 seconds duration, film-like quality, professional color grading, romantic mood

(技术) small aperture (f/11-f/16), deep focus, sharp focus throughout, slow motion, high frame rate (60fps+)
(光照) natural daylight, warm tones, cinematic lighting
(氛围) dreamy, romantic, tender emotion, elegant atmosphere
(质量) 4k, ultra high definition, cinematic film quality, ARRI Alexa style, professional cinematography, film grain, cinematic color grading
```

---

## 七、常用质量标签

### 图生图质量标签
```
masterpiece, best quality, highly detailed, ultra-detailed,
8k, 4k, high resolution, sharp focus,
cinematic lighting, professional photography,
perfect anatomy, beautiful face, detailed eyes,
raw photo, unprocessed, original, photorealistic
```

### 图生视频质量标签
```
cinematic, film quality, 4K, 60fps, smooth motion,
professional cinematography, smooth camera movement,
seamless loop, no interpolation
```

### 通用负面提示词

**图生图**:
```
deformed, blurry, bad anatomy, bad proportions,
extra limbs, extra fingers, fused fingers, missing fingers, deformed hands,
unnatural pose, incorrect posture,
ugly, disfigured, poorly drawn face, mutation,
watermark, text, logo, signature,
low quality, worst quality, jpeg artifacts,
monochrome, greyscale, oversaturated,
no artifacts, clean output,
```

**图生视频**:
```
jump cut, cut, abrupt transition, shakey,
motion blur, low fps, stuttering, frame drops,
noise, grain, artifacts, compression artifacts,
deformed, unnatural movement, robotic motion
```

---

## 九、多轮对话编辑功能

### 上下文记忆机制

系统维护以下上下文记忆：

| 记忆字段 | 说明 |
|----------|------|
| 原始需求 | 用户最初的一级提示词 |
| 当前提示词 | 最近一次生成的二级提示词 |
| 创作类型 | 图生图(i) / 图生视频(v) |
| 分镜头信息 | 如果有分镜头，记录分镜头内容 |

### 结束暗号

当用户输入 `end` 或 `结束` 时，系统执行以下操作：

1. 清空所有上下文记忆
2. 准备好接受新的提示词需求
3. 不输出任何内容，直接进入等待状态

### 整改暗号识别

当用户输入以单个字母 `c` 结尾（前面有空格）时，系统进入整改编辑模式。

**整改需求特点**:
- 用户已有关联的上下文记忆（之前生成过提示词）
- 用户输入表达修改意图，但表达方式灵活多样
- 不限于特定关键词，任何修改描述都可以

**常见整改表达方式**（仅供参考，不限于这些）:
| 表达类型 | 示例 |
|----------|------|
| 重新设计 | 重新设计三个镜头、重新设计分镜头 |
| 替换元素 | 改为中景、换成长裤、换成公园、场景改为校园 |
| 修改描述 | 角色的裤子换成长裤、角色的行为改为散步 |
| 其他修改 | 景别大一点、镜头改远一点、角色换成男孩 |

**重要**: 整改需求识别仅依赖末尾暗号 `c`，不限制用户使用的修改词汇。系统根据用户描述的修改意图，结合当前上下文记忆，自动解析并应用修改。

### 整改处理流程

1. **解析整改需求**: 从用户输入中提取整改意图和目标元素
2. **定位修改位置**: 在当前提示词中找到需要修改的部分
3. **应用修改**: 根据整改要求更新提示词内容
4. **保持一致性**: 如果修改涉及某个图片引用，确保关联的保护性提示词也被更新
5. **输出完整提示词**: 整改后输出完整的二级提示词（非差异）

### 整改输出格式

整改后的输出格式与首次生成完全一致：

```markdown
## 📝 二级提示词（整改后）

### 英文提示词
(主体) [完整的英文主体描述]
(场景) [完整的英文场景描述]
...（所有部分）

### 中文翻译
(主体) [完整的中文主体描述]
...（所有部分）
```

### 整改示例

#### 示例 1：重新设计镜头数量

**当前提示词**: 用户已有一个3镜头分镜头脚本

**用户输入**: `重新设计两个镜头 c`

**整改结果**: 生成全新的2镜头分镜头脚本

#### 示例 2：修改景别

**当前提示词**: 包含全景(wide shot)描述

**用户输入**: `景别改为中景 c`

**整改结果**: 将所有镜头景别更新为中景(medium shot)

#### 示例 3：修改服装

**当前提示词**: 包含"浅蓝色牛仔短裤"描述

**用户输入**: `角色的裤子换成长裤 c`

**整改结果**: 将裤子描述更新为"深色长牛仔裤"或用户指定的长裤类型

#### 示例 4：修改场景

**当前提示词**: 场景为"夏威夷海滩"

**用户输入**: `场景改为校园 c`

**整改结果**: 场景更新为校园环境，其他元素（如果有分镜头则同步更新）

#### 示例 5：修改角色行为

**当前提示词**: 角色动作为"站立"

**用户输入**: `角色的行为改为滑滑板 c`

**整改结果**: 角色动作更新为"滑滑板"，并添加相关的动作细节描述

#### 示例 6：连续整改

**步骤1**: 用户输入 `景别改为中景 c` → 系统应用景别修改
**步骤2**: 用户输入 `角色的行为改为散步 c` → 系统在修改后的基础上应用动作修改
**输出**: 完整的整改后提示词，包含景别和动作的双重修改

### 多轮对话管理原则

1. **上下文累积**: 连续整改时，新的修改叠加在之前修改的基础上
2. **新需求清空**: 当用户输入新的完整需求（带i或v暗号）时，清空旧上下文
3. **结束清空**: `end` 或 `结束` 彻底清空所有记忆
4. **图片引用保留**: 整改时保留原有的图片引用标记

---

## 十、最佳实践

### 转换原则

1. **保持原意**: 转换时不丢失用户的任何原始意图
2. **结构清晰**: 二级提示词按固定结构输出，便于用户理解和修改
3. **自动补全**: 对于用户未指定但必要的技术参数（如质量标签），自动补充
4. **容错处理**: 对于无法识别的词块，归入「附加描述」保留原词
5. **图片引用标注**: 图片引用清晰标注来源，便于用户理解和后续修改
6. **摄影术语整合**: 镜头运动、景别、角度等术语整合到对应分段

### 权重使用建议

```
(word:1.2) - 推荐增强权重范围 0.5-1.5
((word)) - 双重括号 = 1.2 倍权重
(word) - 单层括号 = 1.1 倍权重
[word] - 中括号不增加权重
```

### 变体生成

转换完成后，可选提供 2-3 个风格变体供用户选择：
- **变体 A**: 保持原意，强化写实摄影风格
- **变体 B**: 添加电影感氛围
- **变体 C**: 强调艺术化表达
