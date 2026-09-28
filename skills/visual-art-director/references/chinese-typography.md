# 中文排版指南（Chinese Typography）

中文不是英文的翻译排版。字宽、标点、行距、授权规则都不同，直接套西文套路会立刻露怯。

## 1. 字体栈（CSS font-family 必须带兜底）

```css
/* 现代黑体（默认首选） */
--font-sans-cn: 'PingFang SC', 'HarmonyOS Sans SC', 'MiSans', 'Noto Sans SC',
  'Source Han Sans SC', 'Microsoft YaHei', sans-serif;

/* 文艺宋体 */
--font-serif-cn: 'Source Han Serif SC', 'Noto Serif SC', 'Songti SC',
  'STSong', 'SimSun', serif;

/* 楷体（国风、文化类） */
--font-kai-cn: 'LXGW WenKai', 'Kaiti SC', 'STKaiti', 'KaiTi', serif;
```

西文搭配：英文/数字单独指定西文字体（Inter、Helvetica、Playfair Display），中文回落到上述栈。混排写法：

```css
font-family: 'Inter', 'PingFang SC', 'Noto Sans SC', sans-serif;
/* 浏览器会自动把拉丁字符分给 Inter，汉字回落到 PingFang/Noto */
```

## 2. 免费商用字体清单（授权会变化，商用前复核）

- 思源黑体 / 思源宋体（Adobe × Google，SIL OFL，首选）
- Noto Sans/Serif SC（与思源同源，Google Fonts 可直接引入）
- HarmonyOS Sans SC（华为，免费商用）
- MiSans（小米，免费商用）
- OPPO Sans（OPPO，免费商用）
- 阿里巴巴普惠体（免费商用，电商类常见）
- 霞鹜文楷 LXGW WenKai（OFL，温和手写楷书，适合文化/生活方式）
- 站酷系列（站酷快乐体、庆科黄油体等，免费商用但需从官方渠道获取文件）

**红线**：微软雅黑（方正版权）、方正/汉仪大部分商业字体、苹方（Apple 系统字体）——可在本机设计稿里自用，但**不要在网页里 @font-face 嵌入、不要用于对外商用出图**，否则有侵权风险。

## 3. 字号、字重、行距、字距

| 项目 | 规则 |
|------|------|
| 字号关系 | 主标题至少是正文 2.5-3 倍；海报主视觉可以 5 倍以上 |
| 字重 | 中文黑体用 500/700/900 三级足够，正文 400；宋体正文不要用细体发虚 |
| 行距 | 正文 1.6-1.8；副标题 1.4；大标题 1.1-1.3（可压到 1.05） |
| 字距 | **中文正文/标题都不要负字距**（与英文大标题相反，负字距会让汉字粘连）；大字标题 0 到 -0.01em 极限即可 |
| 全大写英文标签 | letter-spacing 0.08-0.15em |
| 段宽 | 每行 14-24 个汉字最舒服；超长拆栏 |

## 4. 标点与数字

- 中文正文用全角标点；连续标点注意挤压视觉空隙（可手动收窄）
- 避头尾：行首不要出现 ，。、！？）】」等，行尾不要出现 （【「等——纯 CSS 用 `line-break: strict; overflow-wrap: break-word;`，精细排版手动断行
- 两端对齐中文段落：`text-align: justify; text-justify: inter-ideograph;`
- 数字、价格、日期用西文字体并加 `font-variant-numeric: tabular-nums`（价格表、倒计时尤其重要）
- 中英文之间留 1/4 空（网页可手动空格，或成熟排版引擎自动处理）
- 省略号用 `……`（六个点），破折号用 `——`，不要拿 `...` 和 `--` 凑合

## 5. 中文标题招式（设计感落地点）

1. **宋大黑小**：思源宋体超大标题 + 黑体小字注解，是最稳的"高级感"公式
2. **拆行错位**：标题按语义切成 2-3 行，左对齐，行宽错落；或某一行整体右移
3. **色块压底**：关键词用纯色块衬底反白（红底黄字/黑底白字），促销与国潮都好用
4. **大字出血**：主字超出版心 5-10%，制造张力；注意安全区
5. **描边与叠压**：中文描边 2-4px（或 -webkit-text-stroke）配合实心字叠压，neo-brutalism 常用
6. **竖排**：`writing-mode: vertical-rl` 用于新中式/文化类，标点用竖排规范，数字横排用 `text-combine-upright`
7. **单字海报**：一个大字（如"展""喜""卖"）占 40% 画面，小字做信息注解
8. **混排对比**：一行之内宋体细体 + 黑体粗体形成词重音，不要整句同粗度

## 6. 层级模板（可直接改）

```css
.h-title { font-family: var(--font-serif-cn); font-size: clamp(40px, 8vw, 96px);
  font-weight: 700; line-height: 1.12; letter-spacing: 0; }
.h-sub   { font-family: var(--font-sans-cn); font-size: clamp(18px, 2.4vw, 28px);
  font-weight: 400; line-height: 1.5; opacity: .85; }
.h-body  { font-family: var(--font-sans-cn); font-size: 16-18px;
  line-height: 1.7; }
.h-meta  { font-family: var(--font-sans-cn); font-size: 12-14px; font-weight: 500;
  letter-spacing: .1em; }  /* 英文标签走全大写+宽字距 */
.price   { font-family: 'Inter', var(--font-sans-cn); font-weight: 900;
  font-variant-numeric: tabular-nums; }
```

## 7. 场景化建议

- **国潮/新中式**：宋体或楷体标题，竖排可用；正文仍用黑体保证可读；印章只用一个小色块
- **电商促销**：超粗黑标题 + 西文 900 数字；标题可以压字、错位，但价格必须最清楚
- **小红书**：标题 40-64px（1242 宽画幅下），一句话不超 12 字，关键词单独上色或加底纹块
- **科技/B端**：黑体 + Inter，全大写英文小标签拉开字距，克制使用宋体
- **日式/生活方式**：细宋或细黑，整体字号偏小、行距偏松，靠留白出气质
