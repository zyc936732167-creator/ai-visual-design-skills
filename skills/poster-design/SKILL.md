---
name: poster-design
description: 海报与宣传单设计，含活动/促销/发布会/节日海报的构图框架、字体层级、背景质感、中文海报招式与印刷尺寸。当用户说做海报、宣传单、活动宣传图、讲座演出海报时使用。
---


# Poster Design

Create striking posters for events, promotions, announcements, and product launches. Output as production-ready HTML/CSS/SVG sized for both print and web display.

---

## Step 1: Load Design Context

1. 读取 design-context 技能（或项目根目录的 design-context.md），获取品牌色、字体与风格原型
2. If missing, ask user to run `design-context` skill or proceed with defaults
3. Posters often have more creative freedom than other formats -- but brand colors and fonts should still anchor the design

---

## AI 出图提示词示例

直接出图时，先读 visual-art-director 技能的 references/image-prompt-guide.md（出图端点、提示词公式、迭代方法），再套用以下场景提示词。需要精确文字时让模型只生成无文字底图，文字一律在 HTML 排版阶段叠加。

### Example Prompts

- **Event Poster:** "Create an event poster for [event name]. [style] design with [color palette]. Large display text '[event name]' at top. Date '[date]' and venue '[venue]' at bottom."
- **Concert Poster:** "Create a concert poster for [artist/band] at [venue]. Dark background with [neon/vibrant] accent colors. Artist name in bold condensed type. Distressed texture overlay. Date and ticket info at bottom."
- **Sale Flyer:** "Create a promotional sale poster. '[discount]% OFF' as the dominant element in [color]. Product images below. Urgency text '[deadline]' in a banner strip. Clean, high-energy retail layout."

---

## Step 2: Determine Poster Type and Dimensions

### Standard Dimensions

| Format | Pixels (72dpi web) | Print Size | Use Case |
|--------|-------------------|------------|----------|
| A4 Portrait | 595 x 842 | 210 x 297mm | Standard flyer |
| A3 Portrait | 842 x 1191 | 297 x 420mm | Large poster |
| US Letter | 612 x 792 | 8.5 x 11in | Standard US flyer |
| US Tabloid | 792 x 1224 | 11 x 17in | Large US poster |
| Web Poster | 800 x 1200 | N/A | Digital distribution |
| Wide Web | 1200 x 800 | N/A | Web banner / landscape |
| Social Poster | 1080 x 1350 | N/A | Instagram portrait |
| Square | 1080 x 1080 | N/A | Multi-platform |

For print-quality output at 300dpi, multiply pixel dimensions by ~4.17 (300/72).

---

## Step 3: Choose Composition Framework

### Z-Pattern
The eye naturally scans in a Z shape. Best for posters with a clear reading order.

```
[Logo/Brand].............[Date]
.............................
......[MAIN VISUAL].........
.............................
[Details]...........[CTA/URL]
```

Place:
- Top-left: Brand mark / identifier
- Top-right: Key date or label
- Center: Hero visual / headline
- Bottom-left: Event details
- Bottom-right: CTA or ticket link

### Focal Point (Centered)
All elements radiate from a single center point. Best for bold, graphic posters.

```
         [Decorative Top]
     -------------------------
     |                       |
     |    [MAIN HEADLINE]    |
     |    [Visual Element]   |
     |    [Subheadline]      |
     |                       |
     -------------------------
         [Details / Footer]
```

### Layered / Overlap
Elements overlap and stack to create depth. Best for dynamic, modern posters.

```
Background layer:  Full-bleed gradient or image
Middle layer:      Large transparent text or shape
Content layer:     Headline + details on semi-transparent card
Top layer:         Decorative accents, floating elements
```

### Split Composition
Canvas divided into two distinct zones (vertical or diagonal split).

```
[Image/Visual Half] | [Text/Info Half]
```

Or diagonal:
```css
.poster { position: relative; }
.visual-half {
  clip-path: polygon(0 0, 65% 0, 45% 100%, 0 100%);
  position: absolute; inset: 0;
}
.text-half {
  clip-path: polygon(65% 0, 100% 0, 100% 100%, 45% 100%);
  position: absolute; inset: 0;
}
```

### Grid-Based
Structured layout using a visible or invisible grid. Best for information-dense posters.

```css
.poster-grid {
  display: grid;
  grid-template-rows: auto 1fr auto;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  padding: 40px;
}
.poster-grid .headline { grid-column: 1 / -1; }
.poster-grid .visual { grid-column: 1; }
.poster-grid .details { grid-column: 2; }
.poster-grid .footer { grid-column: 1 / -1; }
```

---

## Step 4: Typography Hierarchy for Posters

Posters need extreme typographic hierarchy. The headline must be readable from a distance (physical or scroll distance).

### Four-Level Hierarchy

```css
/* Level 1: Display / Headline -- the hook */
.poster-headline {
  font-family: var(--font-heading);
  font-size: clamp(48px, 8vw, 96px);
  font-weight: 800;
  line-height: 0.95;
  letter-spacing: -0.03em;
  text-transform: uppercase; /* optional, depends on style */
}

/* Level 2: Subheadline -- context */
.poster-subheadline {
  font-family: var(--font-heading);
  font-size: clamp(20px, 3vw, 32px);
  font-weight: 400;
  line-height: 1.3;
  letter-spacing: 0.02em;
}

/* Level 3: Body / Details -- the specifics */
.poster-body {
  font-family: var(--font-body);
  font-size: clamp(14px, 2vw, 18px);
  font-weight: 400;
  line-height: 1.5;
}

/* Level 4: Fine print / Meta -- secondary info */
.poster-meta {
  font-family: var(--font-body);
  font-size: clamp(10px, 1.2vw, 14px);
  font-weight: 500;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}
```

### Typography Treatments for Posters

```css
/* Stacked headline -- each word on its own line */
.stacked-headline {
  display: flex;
  flex-direction: column;
  line-height: 0.9;
}
.stacked-headline span {
  display: block;
}
.stacked-headline span:nth-child(odd) {
  font-style: italic;
  font-weight: 300;
}

/* Mixed weight headline */
.mixed-weight span.light { font-weight: 300; }
.mixed-weight span.bold { font-weight: 800; }

/* Oversized first letter */
.drop-cap::first-letter {
  font-size: 3em;
  float: left;
  line-height: 0.8;
  margin-right: 8px;
  font-weight: 800;
}

/* Knockout text (text reveals background) */
.knockout-text {
  background: url('texture.jpg') center/cover;
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
}
```

---

## Step 5: Background Techniques

### Gradient Mesh
```css
.gradient-mesh {
  background:
    radial-gradient(at 0% 0%, rgba(99, 102, 241, 0.4) 0%, transparent 50%),
    radial-gradient(at 100% 0%, rgba(236, 72, 153, 0.4) 0%, transparent 50%),
    radial-gradient(at 100% 100%, rgba(34, 197, 94, 0.3) 0%, transparent 50%),
    radial-gradient(at 0% 100%, rgba(245, 158, 11, 0.3) 0%, transparent 50%),
    linear-gradient(180deg, #0F172A, #1E293B);
}
```

### Photo Overlay with Gradient
```css
.photo-bg {
  background:
    linear-gradient(180deg, rgba(0,0,0,0.7) 0%, rgba(0,0,0,0.2) 40%, rgba(0,0,0,0.8) 100%),
    url('event-photo.jpg') center/cover;
}
```

### Geometric Pattern
```css
.geo-pattern {
  background-color: var(--primary);
  background-image:
    linear-gradient(30deg, rgba(255,255,255,0.05) 12%, transparent 12.5%, transparent 87%, rgba(255,255,255,0.05) 87.5%),
    linear-gradient(150deg, rgba(255,255,255,0.05) 12%, transparent 12.5%, transparent 87%, rgba(255,255,255,0.05) 87.5%),
    linear-gradient(30deg, rgba(255,255,255,0.05) 12%, transparent 12.5%, transparent 87%, rgba(255,255,255,0.05) 87.5%),
    linear-gradient(150deg, rgba(255,255,255,0.05) 12%, transparent 12.5%, transparent 87%, rgba(255,255,255,0.05) 87.5%);
  background-size: 80px 140px;
  background-position: 0 0, 0 0, 40px 70px, 40px 70px;
}
```

### Noise Texture Overlay
```html
<svg style="position:absolute;inset:0;width:100%;height:100%;opacity:0.06;pointer-events:none;">
  <filter id="noiseFilter">
    <feTurbulence type="fractalNoise" baseFrequency="0.65" numOctaves="3" stitchTiles="stitch"/>
  </filter>
  <rect width="100%" height="100%" filter="url(#noiseFilter)"/>
</svg>
```

### Halftone Effect (Retro)
```html
<svg style="position:absolute;inset:0;width:100%;height:100%;mix-blend-mode:multiply;opacity:0.15;">
  <pattern id="halftone" x="0" y="0" width="6" height="6" patternUnits="userSpaceOnUse">
    <circle cx="3" cy="3" r="1.5" fill="currentColor"/>
  </pattern>
  <rect width="100%" height="100%" fill="url(#halftone)"/>
</svg>
```

---

## Step 6: Event Information Layout

### Essential Event Info Block
```html
<div class="event-details">
  <div class="detail-row">
    <span class="detail-icon">&#x1F4C5;</span>
    <div>
      <strong>Saturday, March 28, 2026</strong>
      <span>Doors open 7:00 PM</span>
    </div>
  </div>
  <div class="detail-row">
    <span class="detail-icon">&#x1F4CD;</span>
    <div>
      <strong>The Grand Hall</strong>
      <span>123 Main Street, San Francisco, CA</span>
    </div>
  </div>
  <div class="detail-row">
    <span class="detail-icon">&#x1F3AB;</span>
    <div>
      <strong>$25 Early Bird / $35 Door</strong>
      <span>tickets.example.com</span>
    </div>
  </div>
</div>
```

```css
.event-details {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 24px;
  background: rgba(255,255,255,0.05);
  border-left: 3px solid var(--accent);
  border-radius: 0 8px 8px 0;
}
.detail-row {
  display: flex;
  align-items: flex-start;
  gap: 12px;
}
.detail-icon {
  font-size: 20px;
  width: 24px;
  text-align: center;
  flex-shrink: 0;
}
```

### CTA Button for Posters
```css
.poster-cta {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 14px 32px;
  background: var(--accent);
  color: #000;
  font-family: var(--font-heading);
  font-size: 16px;
  font-weight: 700;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  text-decoration: none;
  border-radius: 4px;
  transition: transform 0.2s, box-shadow 0.2s;
}
.poster-cta:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(0,0,0,0.3);
}
```

---

## Step 7: Style Variations

### Concert / Music Poster
- Dark background (black, deep purple, navy)
- High-contrast neon or vibrant accent colors
- Large artist name as the dominant element
- Distressed or grungy textures
- Decorative elements: sound waves, speakers, vinyl records
- Typography: bold condensed sans-serif or display fonts

### Tech Event / Conference
- Clean, modern gradient backgrounds
- Geometric shapes and abstract patterns
- Speaker headshots in circular frames
- Technology-themed decorative elements (circuit patterns, code snippets)
- Typography: clean sans-serif, monospace for details
- Color: blues, purples, teals

### Sale / Promotion
- Bright, attention-grabbing colors (red, yellow, orange)
- Discount percentage as the largest element
- "SALE" text with bold treatment
- Urgency indicators ("Limited Time", countdown)
- Clean product images
- Typography: impact/condensed bold for numbers, clean sans for details

### Product Launch
- Product image as hero (large, well-lit)
- Minimal text -- let the product speak
- Premium gradient background
- Launch date prominent
- Subtle glow effects around product
- Typography: elegant, spaced, modern

### Minimalist Event
- Single dominant color + white/black
- Lots of whitespace
- Typography-driven -- the text IS the design
- Thin lines and geometric accents
- Essential info only, no decoration
- Sophisticated, gallery-like aesthetic

### Vintage / Retro Event
- Warm color palette (#C84B31, #F4E8C1, #2D1B00)
- Halftone dot patterns
- Serif or slab-serif typography
- Aged paper texture backgrounds
- Border frames and ornamental dividers
- Distressed/rough edge effects

---

## Step 8: Decorative Elements

### Dividers and Rules
```css
/* Gradient rule */
.divider {
  height: 2px;
  background: linear-gradient(90deg, transparent, var(--accent), transparent);
  margin: 24px 0;
}

/* Ornamental divider */
.ornament-divider {
  display: flex;
  align-items: center;
  gap: 16px;
}
.ornament-divider::before,
.ornament-divider::after {
  content: '';
  flex: 1;
  height: 1px;
  background: currentColor;
  opacity: 0.3;
}
```

### Floating Shapes
```css
.floating-shape {
  position: absolute;
  border-radius: 50%;
  filter: blur(60px);
  opacity: 0.2;
  pointer-events: none;
}
.shape-1 { width: 300px; height: 300px; background: var(--primary); top: -100px; right: -50px; }
.shape-2 { width: 200px; height: 200px; background: var(--accent); bottom: 10%; left: -30px; }
```

### Border Frame
```css
.poster-frame {
  position: absolute;
  inset: 16px;
  border: 1px solid rgba(255,255,255,0.15);
  border-radius: 4px;
  pointer-events: none;
}
```

---

## Step 9: 中文海报实战补充

- 国内常用印刷尺寸：A4 210x297mm（300dpi 约 2480x3508px）、A3 297x420mm；电子邀请函/微信群海报 1080x1920；易拉宝 80x200cm。
- 中文标题常用招式：超大单字或词组出血、宋体大字配黑体小字注解、标题拆行错位、色块压底反白、竖排标题配印章红点缀。
- 活动海报四要素缺一不可：什么事、什么时间、在哪、怎么参加。
- 促销海报的价格数字用西文 900 字重加 tabular-nums，原价加删除线，折扣数字面积可达正文 5 倍以上。
- 商用出图不要随手使用微软雅黑/方正系字体，免费商用选择见 visual-art-director/references/chinese-typography.md。

## Quality Checklist

- [ ] Headline readable from a distance (large, bold, high contrast)
- [ ] Clear visual hierarchy: Headline > Subheadline > Details > Meta
- [ ] Essential info present: What, When, Where, How (for events)
- [ ] Background sets the right mood and tone
- [ ] Brand colors and fonts from design-context are applied
- [ ] Composition follows a clear framework (Z, focal, split, or grid)
- [ ] Whitespace is sufficient -- nothing feels cramped
- [ ] Decorative elements enhance without competing with content
- [ ] Dimensions match the intended output format
- [ ] Code is self-contained HTML/CSS
