# AI 出图提示词指南（Image Prompt Guide）

在 TRAE 环境中直接产出位图的标准做法。目标：让模型生成有设计感的**底图、场景、质感与插画**，精确文字一律留给 HTML 排版叠加。

## 1. 出图端点

```
https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt={URL编码后的提示词}&image_size={尺寸参数}
```

- `prompt`：完整提示词，必须 URL 编码（空格转 %20、中文直接编码）。在 HTML/Markdown 中使用时先编码再拼接
- `image_size` 仅支持这 6 个值：

| 参数 | 画幅 | 用途 |
|------|------|------|
| square_hd | 高清方形 | 电商主图、头像、1:1 海报 |
| square | 方形 | 社媒方图 |
| portrait_4_3 | 3:4 竖 | 小红书封面（1080x1440 同比例） |
| portrait_16_9 | 9:16 竖 | 抖音/视频号/故事/竖版海报 |
| landscape_4_3 | 4:3 横 | 文章配图、演示 |
| landscape_16_9 | 16:9 横 | B站/YouTube/PPT 首屏、横版 banner |

用法：

- HTML 稿：`<img src="该URL" alt="">` 作为底图层，上方叠文字层
- Markdown 交付：`![方案描述](该URL)`
- 3:4 是小红书首选但端点无 3:4 以外的竖比例：1242x1660 与 portrait_4_3（3:4）等比，生成后可按需裁切，构图时把核心内容放在中央安全区
- 端点不支持参考图输入；需要带商品合成时走"抠图 + 生成背景 + HTML 合成"

## 2. 提示词公式（叙事长句，不要堆关键词）

```
[画幅与类型] of [主体/场景], [构图与机位], [环境/道具],
[光源与氛围], [材质与色彩], [风格锚点/设计运动],
[渲染或镜头语言], [画面用途], no text, no watermark
```

一个好的提示词回答 7 件事：**画什么、怎么摆、在哪、什么光、什么材质颜色、像谁的风格、用什么镜头/渲染**。

示例（小红书茶饮 3:4 底图）：

```
A vertical 3:4 commercial background image for a Chinese tea drink poster,
a single glass of iced oolong tea with condensation placed on a pale linen
tablecloth, lower-center composition leaving the upper half as clean
negative space for a headline, soft diffused morning window light from the
left, beige and sage-green color palette with one small rouge-red accent
(brand colors #F5F1E8 #A8B5A0 #9D2933), contemporary Chinese minimalist
editorial style, matte paper texture feeling, shallow depth of field,
50mm lens look, premium lifestyle photography, no text, no logo,
no watermark
```

示例（科技发布会 16:9 背景）：

```
A 16:9 abstract keynote background, deep navy #0F172A matte surface with
a single soft cyan #22D3EE light beam cutting diagonally across the lower
third, generous dark negative space in the center-left for headline text,
fine grain texture, subtle geometric grid lines fading into darkness,
Swiss international typographic poster mood, cinematic rim lighting,
minimal and premium, no text, no letters, no watermark
```

## 3. 文字策略（最重要的一条）

文生图模型渲染中文几乎必然出错，渲染英文也经常拼错。商业稿的分工：

1. 提示词一律写 `no text, no letters, no logo, no watermark`，让画面留白
2. 在提示词里**规划留白区域**（"upper half as clean negative space for a headline"）
3. 文字、价格、Logo、二维码全部用 HTML/CSS 叠在底图上（绝对定位 + 安全区）
4. 若画面确实需要文字感（如招牌、书页），只接受它作为氛围元素，不承载准确信息

## 4. 风格锚点词汇

直接引用设计运动或成熟视觉语言，比写 "high-end, beautiful" 有效得多：

- 平面/构成：Swiss International Style poster、Bauhaus geometric composition、Memphis pattern、neo-brutalist flat design、acid graphic with chrome type、1960s vintage screen print
- 摄影：editorial lifestyle photography、product studio photography、Kinfolk style、Kodak Portra 400 film look、cinematic teal-and-orange
- 东方：contemporary Chinese minimalist (xin zhong shi)、ink wash subtle texture on rice paper、Japanese muji-style still life
- 渲染：matte clay 3D render、soft inflated 3D、isometric C4D render、frosted glass macro
- 光影：soft diffused window light、golden hour rim light、dramatic chiaroscuro、studio softbox top light、neon night with wet reflections

完整风格选择见 style-taxonomy.md。

## 5. 构图词汇

- 机位/视角：top-down flat lay、45-degree angle、eye-level、low angle hero shot、isometric、macro close-up
- 位置：centered with negative space、rule of thirds、lower-third placement、diagonal composition、asymmetric split
- 景深/镜头：shallow depth of field、50mm f/1.8、wide-angle 24mm、tilt-shift miniature
- 层次：foreground prop framing、layered depth、single subject isolation、clean seamless backdrop

## 6. 光与材质词汇

- 光：soft diffused light、hard directional shadow、golden hour、overcast soft light、backlit glow、practical neon light、god rays
- 影调：high-key bright、low-key moody、high contrast、matte finish、soft contact shadow
- 材质：brushed metal、frosted acrylic、silk fabric、raw concrete、linen textile、ceramic glaze、tempered glass、liquid chrome、kraft paper、rice paper fiber
- 质感后处理：fine film grain、subtle noise texture、halftone print texture、vignette

## 7. 颜色写法

写具体色值与占比，不写 "colorful"：

- "color palette of #0F172A navy and #F5F0E6 ivory with 10% gold #C9A96A accent"
- "60 percent warm off-white, 30 percent terracotta, 10 percent charcoal"
- 渐变要说方向与色标："soft gradient from #1E293B at top to #0F172A at bottom"
- 避免说 "vibrant gradient" 而不给颜色——那会得到默认的紫蓝 AI 渐变

## 8. 负面约束

按场景加在结尾：

- 通用：no text, no letters, no watermark, no signature, no logo（除非是规划氛围）
- 商品：no deformation, no extra fingers/objects, realistic proportions, keep product shape unchanged
- 人像：natural skin texture, no plastic skin, symmetric eyes
- 版式：no clutter, no busy background（给文字留底）
- 印刷：no fake QR code, no garbled characters

## 9. 迭代方法

- 每轮只改一个变量：先定构图 → 再定光 → 再定配色 → 最后微调质感
- 用自然语言做相对修改："move the subject to lower-right, keep everything else identical"（若环境不支持图生图，则用新提示词重出，把已满意的描述完整保留，只改一句）
- 连续 2-3 张都不理想时，不要继续抽卡：退回 style-taxonomy.md 重选风格锚点，重写整句
- 出图后立即缩略图测试：缩到 25% 看主体轮廓与留白是否成立

## 10. 常见失败与修法

| 症状 | 修法 |
|------|------|
| 画面出现乱码文字 | 加 no text / no letters / no signage；改为生成纯背景 |
| 默认紫蓝渐变味 | 明确写死两个以上 hex 与渐变方向，加风格锚点 |
| 主体太小/太满 | 用 "occupies 60 percent of frame" / "small in frame with large negative space" 量化 |
| 廉价塑料感 | 改 matte finish + soft diffused light，删掉 glossy、3D render 字样 |
| 元素太多显乱 | 删道具描述，只留一个主体，加 minimalist、clean negative space |
| 商品变形 | 不让模型重绘商品：单独生成场景，商品抠图合成 |
| 画面发灰发脏 | 提高光源描述强度，给一个明确 accent color，contrast 适度 |
| 中文氛围不地道 | 用 contemporary Chinese / Japanese still life 锚点 + 宣纸/棉麻/青瓷等具体材质，而非泛写 oriental |
