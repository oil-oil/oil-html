---
name: oil-html
description: oil 个人专用 HTML 分享文档 skill。只有当用户明确点名「oil-html」、说「使用 oil-html / 用 oil-html 这个 skill」、或通过 skill 选择器明确指定本 skill 时才调用。用户只是说「帮我做一份 HTML」「写一个介绍页」「做一个演示文档」「做可分享 HTML」时不要自动调用本 skill。被明确指定后，本 skill 用于制作滚动阅读的单页 HTML 分享文档，风格为白底黑字、网格纹理背景、拟真 UI 模拟、拟人化插画、oil-tone 文案风格；要做成 16:9 翻页幻灯片 / PPT 用 oil-slides，不用本 skill。
---

# oil-html

给人看的 HTML 分享文档。用来介绍功能、展示产品、做对外演示。

## 什么时候用

**只有在用户明确指定 `oil-html` 时才使用本 skill。** 明确指定包括：用户写出「oil-html」「使用 oil-html」「用 oil-html 这个 skill」，或通过 skill 选择器点名本 skill。

以下表达不再触发本 skill：用户只是说「帮我做一份 HTML」「写一个介绍页」「做一个演示文档」「做可分享 HTML」「做一个产品展示页」。这些请求按普通任务处理，或交给当次上下文里更合适的 skill。

要做一份 HTML 拿出去给别人看的时候——功能介绍、skill 演示、产品展示、方案说明。这些文档的读者不是开发者，是要了解「这个东西能帮我做什么」的人。

不用这个 skill 的情况：

- 纯内部技术文档、API 参考、架构说明 — 用 Markdown 就够
- 需要交互表单、实时状态的界面 — 那是前端工程，不是文档
- 给另一个 Agent 读的输出 — 用纯文本或 JSON

## 设计基底

白底黑字。背景有微妙的网格纹理和径向渐变，增加质感但不抢内容。整体感觉干净、有呼吸感、像好的产品 landing page。

### 排版

- 正文 16px，行高 1.85，字间距 0.02em
- 中文字体 `Noto Sans SC` / `PingFang SC`，英文 `Inter`
- 标题用 600-700 weight，正文 400
- 一定要舒服可读，宁可大也不要挤
- **最小可读字号 14px**。拟真 UI 卡片内的字段值、标签内容等也不能低于 14px。12px 在实际页面上看不清，宁可卡片大一点也不缩字

### 配色

黑白灰为底，暖黄是体系里唯一的常驻色——插画里边牧的毛色、插画色块容器（`--c-illo`）、荧光笔高亮（`--c-hl`）用的是同一个暖黄色相，贯穿整份文档形成呼应。其他颜色从内容语义出发——平台品牌色、状态色（成功绿、警告黄）。暖黄之外，同一份文档彩色不超过 2 个色相。

### CSS 设计系统

本 skill 目录下的 `oil-html.css` 是内置的设计系统。写 HTML 时把这个 CSS 内联到 `<style>` 里，然后用里面的 class 来组合页面。

核心 class 分类：

```text
布局: .oil-page, .oil-section, .oil-grid-2/3/4, .oil-flex, .oil-flex-col
背景: .oil-bg-grid, .oil-bg-gradient
排版: .oil-h1, .oil-h2, .oil-h3, .oil-body, .oil-caption, .oil-mono, .oil-hl(荧光笔高亮)
卡片: .oil-card, .oil-card-flat, .oil-card-elevated
插画容器: .oil-illo + .oil-illo-br/bl/bc(蒙版落位), .oil-illo-pop + .oil-illo-frame(探出), .oil-dots(网点装饰)
大数字: .oil-stat-num, .oil-stat-label
模块: .oil-module, .oil-module-dark, .oil-module-character, .oil-module-mask
聊天模拟: .oil-chat, .oil-chat-header, .oil-chat-body, .oil-chat-msg, .oil-chat-user, .oil-chat-ai
平台卡片: .oil-platform-card, .oil-platform-bar, .oil-platform-body, .oil-platform-field
终端模拟: .oil-terminal, .oil-terminal-header, .oil-terminal-body
标签: .oil-tag, .oil-tag-accent, .oil-tag-success, .oil-tag-blue
状态: .oil-status, .oil-status-ready, .oil-status-pending
步骤: .oil-steps, .oil-step, .oil-step-num, .oil-step-connector
其他: .oil-pill, .oil-divider, .oil-center
```

写文档时先用已有的 class，不够再加页面级自定义样式。这样所有 oil-html 文档视觉一致，token 消耗也少。

## 内容铁律

### 只放读者需要看的东西

每一块内容必须回答：**看完这个，读者能做什么或者明白什么？**

过滤器：「如果我当面给人演示，我会打开这个画面给他看吗？」会的就放，不会的就删。

删掉这些：

```text
- 文件结构树、代码架构图 — 读者不需要知道你的代码怎么组织
- 内部设计决策 — 「不存表去平台看」「用 CDP 注入」对读者毫无意义
- 运行模式、配置参数 — 这是开发者的事
- 生成时间、接口/模型选型、重新生成的命令、「没有写入 Key」这类交代 — 工作记录不是内容，跟读者没关系
- 重复说同一件事 — 如果上面已经讲了，下面不要换个说法再讲一遍
- 安全机制的详细列表 — 一句话说「发布前会停下来等你确认」就够了
```

### 用拟真 UI 代替文字描述

能用模拟界面展示的，不要用文字解释。

```text
BAD: 「支持小红书、抖音、B站、视频号四个平台，每个平台有不同的标签格式和封面规格」
GOOD: 做四个拟真的平台创作者后台卡片，里面已经填好了标题、标签和封面状态
```

```text
BAD: 「用户说一句话，skill 会自动识别视频、拟定标题和标签、选择平台、填写表单」
GOOD: 做一个模拟的聊天界面，展示用户和 skill 的对话，一步步看到标题生成、平台选择、表单填写的过程
```

拟真 UI 不是截图，是手搓 HTML/CSS。用 oil-html.css 里的 `.oil-chat`、`.oil-platform-card`、`.oil-terminal` 等 class 来搭。

### 插画规范

插画用来给抽象概念（Agent、自动化、安全机制）加一层直觉理解，不是装饰。

#### 风格：漫画墨水 + 半调网点

风格参考：日系漫画独立志 / @鸭鸭垅 portfolio 风。黑白灰为主，极少量彩色点缀。

```text
线条: 干净利落、粗细分明的黑色墨水勾线，像蘸水笔或针管笔画的。线条自信流畅，
      不是抖动涂鸦线也不是光滑数字矢量线。粗线做轮廓，细线做内部结构。
填色: 大面积留白 + 黑色实填 + 半调网点（screentone）做中间灰。
      不用渐变，不用柔和过渡，灰色全靠网点密度控制。
网点: 经典圆形半调网点阵列（comic halftone screentone dots），规则排列，
      用于角色皮肤阴影、衣服质感、背景灰面。这是风格的核心视觉元素。
光影: 黑白对比为主，用网点 screentone 做中间调。局部可用黄色色块暗示暖光。
```

#### 配色铁律

```text
九成画面无彩色: 黑 + 白 + 灰（网点）
黄色: 只用在边牧同伴的毛色、局部暖光色块、少量星星装饰。暖黄不是亮黄。
其他颜色: 除平台品牌色（红/绿等）外不引入新色相。品牌色只出现在图标本身，
          不扩散到整张画面。
```

写 prompt 时加这句：「Color usage is extremely restrained — 90% of the image is black, white, and grey halftone screentone. Warm yellow only on the chubby Border Collie companion, small light patches, and sparse star decorations. No other colors except where platform brand icons require them.」

#### 角色体系

每份文档围绕一个**火柴人主角 + 黄色边牧同伴**的角色组合，保持一致性。

```text
主角: 可爱火柴人风格——圆形头（白色填充，screentone 网点做阴影），
      简笔画身体（细线条四肢），戴圆框细边眼镜，两个圆点眼睛，
      弯弯的微笑嘴。极简造型，没有复杂服装细节。
      可加小配件（耳机、小帽子）突出主题，但基础是火柴人不变。
同伴: 黄色胖嘟嘟的小边牧犬，圆滚滚的体型，黑色墨水轮廓，
      毛色暖黄 + 白色胸口，小短腿，表情憨厚可爱，约主角 40-50% 大小。
点缀: 黄色星星（少量）、音符、感叹号等。数量克制，每张 3-5 个就够。
```

#### Codex prompt 模板

Prompt 结构 = **主体描述 + 风格锚定**。

- 主体描述用中文,写清画面主角、动作、道具、构图。例:「火柴人主角坐在书桌前敲键盘,黄色边牧同伴趴在椅子脚边,桌上有耳机和马克杯,视角略微俯视」。一句话画完一张图,别塞多个场景。
- 结尾追加下面这段英文风格锚定,固定不改(是这套 skill 的画风指纹):

```text
Style: professional manga/comic ink illustration. Clean confident ink outlines
with varying line weights (thick for contours, thin for details), NOT wobbly
or sketchy. Heavy use of classic circular halftone screentone dot patterns for
all grey/shadow areas — this is the signature visual element. Flat black fills
for hair and dark areas. White for skin/body with screentone shading. The main character is a cute
stick figure with a round head, thin round glasses, dot eyes, simple smile,
and thin line-drawn limbs — minimal and charming, not detailed.
Color usage is extremely restrained — 90% of the image is black, white, and
grey halftone screentone. Warm yellow only on the chubby Border Collie
companion dog, small warm light patches, and sparse star decorations.
Rich detail and visual complexity like a printed comic page or indie zine
illustration, not a simple mascot icon.
CRITICAL — Background must be PERFECTLY UNIFORM flat grey (#808080) with
ZERO gradient, ZERO texture, ZERO noise, ZERO speckles. Every single
background pixel must be the exact same grey value. Do NOT let screentone
dots, halftone patterns, ink splatter, or any visual element bleed into the
background area. The background is a clean solid rectangle of #808080 —
treat it like a green screen. PNG format, square composition.
```

#### 生成流程

**默认路径:Codex image_gen(灰底 #808080 管线)。** 没有 Codex 环境时用 zenmux 绿幕管线 fallback:`python3 ~/.claude/skills/oil-slides/scripts/gen_art.py --subject "<主体描述>" --out <输出路径>`,同角色同画风,只是把灰底换成绿幕、抠图内置一步到位。两条路径别混用——灰底图跑绿幕键控会留一大块灰。

1. **执行 Agent(Codex,或 gen_art.py 进程)生成 PNG**——背景必须干净:灰底管线是纯色灰 #808080,绿幕管线是纯绿,都不能带纸张纹理、渐变或噪点。
2. **PNG 落到 `/tmp/` 或文档输出同级目录**,不要放进 Skill 目录(图片是文档的产物,不是 Skill 的资产)。
   - Codex 生成后由 Codex 自己 `mv` / `cp` 出来;gen_art.py 直接用 `--out` 指定路径。
3. **Claude 接手检查背景是否干净均匀**——有渐变、散点、纹理溢出必须重新生成,否则 cutout 会留噪点。
4. **抠图得到透明 PNG**:
   - 灰底管线 → 跑 `~/.claude/skills/codex/scripts/cutout.py`。
   - 绿幕管线 → `gen_art.py` 已经内置绿幕键控,输出就是透明 PNG,不用再跑第二个脚本。
5. **透明 PNG 怎么放进 HTML**(二选一):
   - **要单文件可分发**(邮件/微信/丢群/上传静态站) → base64 内联。
   - **要方便替换单张、或多张多篇复用** → 放在文档同级目录,用相对路径引用。

#### 蒙版色块容器（默认的插画摆法）

抠好的透明 PNG 不要裸放在白底上——默认放进圆角色块容器里，让图一部分露出、一部分被容器边裁掉，像贴纸被蒙版裁过。这个手法是整套设计感的主要来源。

```html
<!-- 蒙版式：图从右下探入容器，超出容器的部分被圆角边裁掉 -->
<div class="oil-illo">
  <div class="oil-dots" style="width:100px;height:100px;left:24px;top:24px;"></div>
  <img class="oil-illo-br" src="character.png" alt="">
</div>

<!-- 探出式：色块只垫下半，角色头顶冒出色块顶边 -->
<div class="oil-illo-pop">
  <div class="oil-illo-frame"></div>
  <img src="character.png" alt="">
</div>
```

容器底色默认暖黄 `--c-illo`，一份文档里容器色保持统一。常见排法：`.oil-grid-2` 一栏文字一栏插画容器，或 Hero 区侧边放一个。网点装饰块（`.oil-dots`）放容器的空白角落，一个容器最多一块。

#### 插画用在哪里

```text
适合: 概念解释（多 Agent 协作、安全机制）、Hero 视觉焦点、图文对排模块
不适合: 已经有拟真 UI 演示的地方（聊天模拟、平台卡片旁边不需要再加插画）
```

一份文档 2-3 张插画就够。插画是辅助，不是主角——拟真 UI 才是核心展示手段。

### 流程可视化

当功能有明确步骤时，用流程图/步骤条让读者一眼看到全貌。

```text
BAD: 只有聊天模拟，读者要从对话里自己推断流程
GOOD: 先用一个 3-5 步的步骤条总览流程，再用聊天模拟展示细节
```

用 oil-html.css 的 `.oil-steps` 组件，或者手搓简洁的横向流程图。步骤条放在核心演示之前，让读者带着全局理解去看细节。

### 文案跟 oil-tone 走

写面向读者的文字之前，先读 `~/.claude/skills/oil-tone/SKILL.md`，文案整体按它来：真诚平实，像在说话，不浮夸不编造。落到这类文档里最要紧的几条：

- 标题要具体、有信息量，不是话题标签
- 不堆技术术语解释功能——用人话说「能帮你做什么」
- 不写「强大的」「高效的」「一键式的」这类废话
- 中英文之间空格，标点跟语言走

### 长度控制

2-3 屏滚动能看完。如果超过 3 屏，先砍内容而不是压缩字号。
一份文档最多 4-5 个模块，每个模块聚焦一件事。

## 文档结构

一份典型的 oil-html 文档：

```text
1. Hero — 一句话说清这是什么、能帮你做什么
2. 核心演示 — 拟真 UI 展示最关键的使用场景（最大的模块，占主要篇幅）
3. 补充展示 — 1-2 个拟真卡片组，展示其他维度（平台、效果、对比）
4. 一句话收尾 — 怎么用 / 在哪用
```

不是模板，是默认节奏。内容不适合就调整，但总共不超过 5 个模块。

## 产出

输出一个完整的 `.html` 文件，CSS 内联在 `<style>` 里（从 `oil-html.css` 复制需要的部分），不依赖外部文件（角色图片用 base64 或独立的 `.png` 放旁边）。

文件放在 `~/Desktop/` 或用户指定的路径。

交付前用 `open <file>.html` 打开过一眼：没有小于 14px 的字、总长不超过 3 屏、模块不超过 5 个、插画没压住文字、彩色不超过 2 个色相。哪条超了回头改，别带病交付。
