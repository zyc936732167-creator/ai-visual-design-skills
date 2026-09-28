# AI 视觉作图 Skill 合集（优化版）

面向本地 AI 智能体（TRAE / Claude Code 等）的视觉作图技能包：15 个技能覆盖**艺术指导、配色、字体、版式、海报、社媒、电商主图、广告、Banner、卡片、封面、商品样机、图像调色、抠图**。在原版 13 技能的客户交付包基础上做了工具链适配与设计质量升级，目标是让 AI 产出的图**真正有设计感、文字不出错、符合国内平台规范**。

## 这一版优化了什么

1. **新增总控技能 `visual-art-director`（视觉设计总监）**：强制执行"澄清需求 → 3 个差异化艺术方向 → 选定后出图 → 评审迭代"流程，拒绝拿需求直出
2. **工具链落地 TRAE**：原版引用的 Gemini 与不存在的 `image-generation` 技能全部替换为 TRAE 实际可用的文生图端点与调用规范（见 `skills/visual-art-director/references/image-prompt-guide.md`）
3. **文字零错误策略**：AI 只生成无文字底图，中文标题/价格/Logo 全部 HTML 排版叠加，根治文生图乱码
4. **反 AI 味机制**：禁用默认紫蓝渐变、模糊光斑、满屏居中等廉价套路；附完整的三层评审与廉价感修法对照表
5. **20+ 视觉风格谱系**：瑞士国际主义、新中式、国潮、日式、酸性、Y2K、新粗野、高奢黑金等，每个都给了 hex 色板、字体、图形语言和避坑点
6. **中文排版指南**：中文字体栈、免费商用字体清单、宋黑搭配、竖排、标点与避头尾
7. **国内平台尺寸/安全区**：小红书、抖音、视频号、公众号、B站、微博、淘系/京东、印刷尺寸
8. **修复悬空引用**：补齐原版引用但未交付的 `design-context` 技能；13 个技能的描述全部改为中英触发词、适配技能路由器
9. `background-removal` 由单一第三方 API 教程改为工具无关的抠图工作流（API/本地模型/替代方案）

## 技能清单

| 技能 | 职责 |
|------|------|
| `visual-art-director` | **总入口/艺术指导**，调度下面所有技能，产出 3 方向并负责评审 |
| `design-context` | 品牌色板、字体、风格原型、禁忌的统一上下文 |
| `color-palette` | OKLCH 配色、色阶、暗色模式、对比度、中国传统色 |
| `typography` | 西文字体配对、字号系统、中文排版规则 |
| `layout-composition` | 网格、层级、留白、竖版/方形手机构图 |
| `graphic-design` | 综合平面视觉，AI 位图与 HTML/CSS/SVG 双模式 |
| `poster-design` | 海报/传单，中文海报招式与印刷尺寸 |
| `social-media-graphic` | 国内外社媒配图尺寸、安全区、平台风格 |
| `ad-creative-design` | 广告创意、信息流、电商主图、A/B 版本 |
| `banner-design` | 网站 Hero、活动 banner、公众号/店招头图 |
| `card-design` | 信息卡片、商品卡、卡网格与交互 |
| `thumbnail-design` | 视频封面/缩略图，高对比小字可读 |
| `product-mockup` | 设备样机、3D 透视、电商主图场景 |
| `image-treatment` | CSS/SVG 调色、双色调、蒙版、质感 |
| `background-removal` | 抠图、白底图、背景替换工作流（需自备抠图工具/API） |

## 安装

把 `skills/` 下的 15 个文件夹整体复制到当前产品的技能目录，结构必须是 `skills/<技能名>/SKILL.md`。

- **TRAE 国内版（Windows）**：`C:\Users\<你的用户名>\.trae-cn\skills\`
- **TRAE 国内版（macOS/Linux）**：`~/.trae-cn/skills/`
- **Claude Code / 其他通用 Agent**：各自文档约定的 `skills/` 目录

PowerShell 一键复制（Windows）：

```powershell
git clone https://github.com/zyc936732167-creator/ai-visual-design-skills.git
Copy-Item -Recurse .\ai-visual-design-skills\skills\* "$env:USERPROFILE\.trae-cn\skills\"
```

复制后重启智能体，新建对话说"帮我做一张图"即应触发 `visual-art-director`。

## 使用方法

1. 复制 `docs/briefs/01_视觉作图总需求卡.md` 到你的项目，填好用途、尺寸、文案、风格、禁忌
2. 对智能体说：**"按视觉作图需求卡，先给我 3 个视觉方向（构图/配色/字体/元素），选定后再出图"**
3. 选定方向后，AI 会按平台尺寸出底图（或直接产出自包含 HTML 成稿）
4. 按 `docs/guides/02_图片验收标准.md` 验收；中文海报参考 `docs/briefs/03_海报社媒图需求卡.md`

## 出图能力说明

- **AI 位图**：在 TRAE 中通过文生图端点生成，支持 6 种比例（方形、3:4、9:16、4:3、16:9）；端点不支持参考图，带商品合成请走"抠图 + 底图 + HTML 排版"
- **HTML 成稿**：海报/卡片/封面优先，文字 100% 精确、尺寸像素级可控，浏览器打开后用截图工具导出
- **抠图**：需要环境自带抠图能力、第三方 API Key 或本地 rembg 模型，详见 `background-removal` 技能
- 最终文案、价格、日期、Logo 与字体授权需人工复核

## 目录结构

```
skills/            # 15 个可安装技能
  visual-art-director/references/   # 风格谱系/出图提示词/中文排版/平台尺寸/评审清单
docs/
  briefs/          # 作图需求卡模板（总卡、商品图、海报社媒、抠图）
  examples/        # 从需求到成图的案例
  guides/          # 首次使用清单、验收标准、常见问题
```

## 字体与版权

- 推荐字体均为免费商用或系统字体（思源系、HarmonyOS Sans、MiSans、OPPO Sans、阿里巴巴普惠体、霞鹜文楷等）；商用前请到字体官方渠道复核当前授权
- 微软雅黑、苹方、方正/汉仪商业字体未经授权不得嵌入网页或商用出图
- 技能代码沿用原包 MIT 许可，见 [LICENSE](LICENSE)
