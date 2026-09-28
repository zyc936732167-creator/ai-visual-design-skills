---
name: background-removal
description: 抠图与背景替换工作流，输出透明 PNG、换纯色/渐变/场景背景的指令模板与质量控制。当用户说抠图、去背景、透明底、换背景、商品白底图时使用；执行前必须确认当前环境有可用的抠图 API 或本地工具与额度。
---

# Background Removal（抠图与背景替换）

把主体从原图中干净分离，产出透明 PNG、白底商品图，或合成到新场景。**先确认工具，再承诺结果**：抠图必须有可用的抠图能力（第三方 API Key、本地模型或平台内置图像编辑），没有时明确告知用户，并给出 HTML 合成等替代方案。

## Step 0: 确认执行条件

开始前检查：

1. 当前环境是否有抠图工具（平台内置图像编辑、抠图 API Key 例如 remove.bg / eachlabs / 火山等，或本地 rembg / SAM 模型）
2. 输入图片是本地路径还是 URL；API 通常要求公网可访问的 URL 或 base64
3. 是否有联网与调用额度；**绝不把 Key 写进产出文件或仓库**

不满足时，告诉用户缺什么，并退而求其次：用 CSS mix-blend-mode / mask 做视觉抠图效果，或先生成纯色背景图让用户在设计工具中抠图。

---

## Step 1: 明确输出类型

| 目标 | 指令要点 | 典型用途 |
|------|----------|----------|
| 透明 PNG | 完全去背景，保留主体与发丝细节 | 合成素材、贴纸、Logo |
| 纯白底 | 换纯白 #FFFFFF，保留自然投影 | 电商主图、平台白底图 |
| 纯色/品牌色 | 换指定 hex 纯色 | 品牌营销图 |
| 渐变底 | 换柔和渐变并补投影 | 人像、质感主图 |
| 场景替换 | 放入指定场景，光影方向匹配 | 生活方式图、氛围图 |
| 摄影棚底 | 无缝灰/米色背景纸 | 专业产品摄影感 |

---

## Step 2: 高质量抠图指令模板

指令要把"保留什么、去掉什么、边缘怎么处理、影子怎么处理"四件事说全。

### 干净去除

```
Remove the background completely. Keep edges sharp and clean,
especially around hair and fine details. Output on a transparent background.
```

### 电商白底图

```
Remove the background from this product photo and replace it with pure
white #FFFFFF. Keep a soft natural drop shadow beneath the product.
Crisp edges with no background color bleeding.
```

### 人像换底

```
Remove the background from this portrait. Replace it with a soft,
lighter-in-the-center dark gray gradient. Keep hair edges soft and wispy,
not hard-cut. Professional headshot quality.
```

### 场景合成

```
Remove the background and place the subject into [场景描述].
Match the lighting direction and color temperature of the original photo.
Keep correct scale, perspective, and a natural contact shadow.
```

### 影子控制（必须显式说明，否则模型行为随机）

- 保留自然投影："Keep the natural contact shadow beneath the subject"
- 完全去影："Remove all shadows for a clean cutout"
- 人造柔影："Add a soft artificial drop shadow on the new background"

---

## Step 3: 按主体类型补要求

| 主体 | 难点 | 指令补充 |
|------|------|----------|
| 人物发丝 | 细碎发丝被硬切 | Keep hair edges soft and natural |
| 宠物毛发 | 毛绒边缘 | Preserve fur detail around edges |
| 硬边商品 | 反光、透明材质 | Crisp edges, handle reflections naturally |
| 植物叶片 | 叶间镂空 | Preserve leaf gaps and fine silhouettes |
| 玻璃/液体 | 半透明区域 | Handle transparent and translucent areas |
| 多主体 | 误抠次要物体 | Isolate only the main subject |

---

## Step 4: 常见调用形态（工具无关）

抠图能力的调用方式因平台而异，统一遵循以下流程：

1. 拿到图片（本地文件先转 base64 或上传到可访问 URL）
2. 按 Step 2 模板构造指令，附带图片发送给抠图/多模态图像接口
3. 保存返回图为 PNG（透明背景只有 PNG/WebP 支持，不要存 JPG）
4. 文件名与原图区分，例如 `product_cutout.png`；**原图只读保留，不覆盖**

如果环境提供命令行工具（如 rembg），等价流程：

```bash
# 需先安装：pip install rembg（本地推理，首次运行下载模型）
rembg i input.jpg output_cutout.png
rembg p input_dir/ output_dir/   # 批量
```

第三方 API 用 curl/SDK 时，Key 从环境变量读取，命令历史和产出文件中不得出现明文 Key。

---

## Step 5: 验收清单

- [ ] 边缘 100% 放大无白边、毛边、原背景色残留
- [ ] 发丝/毛绒/镂空细节自然，没有刀切感
- [ ] 透明底在深色与浅色棋盘格上都检查过
- [ ] 投影方向、虚实与新场景光源一致
- [ ] 主体比例、透视在新场景中可信
- [ ] 输出为 PNG，原图未被覆盖
- [ ] 用于电商时符合平台白底图规范（纯白、主体占比、无多余文字角标按类目规则）

## 关联技能

- product-mockup：抠图后的商品合成到设备框、场景舞台
- image-treatment：合成后的调色、双色调、质感统一
- visual-art-director：生成新场景底图的提示词工程（references/image-prompt-guide.md）
