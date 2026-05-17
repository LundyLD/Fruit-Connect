# DESIGN.md — Web 端连连看游戏设计规范

**版本：** v0.1  
**日期：** 2026-05-16  
**对应 PRD：** PRD-连连看.md v0.1 MVP

---

## 1. 设计气质

**三个关键词：清爽、亲切、即时**

| 关键词 | 含义 | 设计决策依据 |
|--------|------|-------------|
| 清爽 | 视觉噪音极少，留白充足，让玩家视线快速落在棋盘上 | 目标场景是碎片时间，用户不需要"读懂"界面，要零秒理解 |
| 亲切 | 色调温暖不冷硬，圆角偏圆，字体不过分正式 | 核心用户是上班族和有情怀的玩家，情绪上要放松而非竞技紧张 |
| 即时 | 每一个操作都要在 300ms 内有视觉回应，状态变化明确 | PRD P0-4/P0-5 明确要求连线动画和错误抖动，反馈是核心体验 |

**不选择的气质：**
- 不选"暗黑游戏风"：目标用户不是硬核玩家，高对比暗色调会增加眼睛疲劳
- 不选"扁平极简纯白"：游戏需要"牌格"有质感，纯白背景让格子失去层次感
- 不选"卡通萌系"：会降低中老年次要用户的亲近感，且与"经典规则"定位矛盾

---

## 2. 色板

### 2.1 主色

| Token | Hex | 使用场景 |
|-------|-----|---------|
| `--color-primary` | `#4A90D9` | 选中状态高亮边框、连线颜色、主要操作按钮背景 |
| `--color-primary-light` | `#EBF4FF` | 选中格子的背景填充（半透明蓝底） |
| `--color-primary-dark` | `#2C6FAC` | 主按钮悬停/按下状态 |

**主色选择理由：** 经典连连看的视觉记忆是蓝色连线（QQ游戏大厅传统），且蓝色在有色棋盘背景上对比度充足，连线动画辨识度高。选 `#4A90D9` 而非纯蓝 `#0000FF`，是为了降低视觉刺激度，保持"清爽"气质。

### 2.2 辅色

| Token | Hex | 使用场景 |
|-------|-----|---------|
| `--color-accent` | `#F5A623` | 提示功能高亮（hint 闪烁）、得分数字强调、"提示"按钮图标色 |
| `--color-accent-light` | `#FFF8EC` | 提示高亮格子的背景填充 |

**辅色选择理由：** 琥珀橙与主色蓝形成冷暖互补，不会混淆"选中"和"提示"两种状态。限用一个辅色，避免色彩噪音。

### 2.3 中性色（9阶灰）

| Token | Hex | 使用场景 |
|-------|-----|---------|
| `--gray-50` | `#F8F9FA` | 页面背景色 |
| `--gray-100` | `#F1F3F5` | 棋盘背景底色 |
| `--gray-200` | `#E9ECEF` | 格子边框（默认态）、分割线 |
| `--gray-300` | `#DEE2E6` | 禁用状态按钮背景 |
| `--gray-400` | `#CED4DA` | 占位符文字、次要图标 |
| `--gray-500` | `#ADB5BD` | 辅助文字（如"剩余次数"标签） |
| `--gray-600` | `#6C757D` | 次级操作按钮文字 |
| `--gray-700` | `#495057` | 正文文字、游戏说明 |
| `--gray-900` | `#212529` | 标题、得分数字、主要文字 |

### 2.4 语义色

| Token | Hex | 使用场景 |
|-------|-----|---------|
| `--color-success` | `#28A745` | 通关弹窗标题色、消除成功短暂闪绿 |
| `--color-danger` | `#DC3545` | 无效配对时格子边框红色、超时弹窗标题 |
| `--color-warning` | `#FFC107` | 倒计时最后 30 秒计时器变色警告 |
| `--color-info` | `#17A2B8` | 洗牌功能操作中的状态提示文字 |

---

## 3. 字体系统

### 3.1 字族

```css
/* 正文/界面文字 */
--font-sans: "PingFang SC", "Microsoft YaHei", "Helvetica Neue", Arial, sans-serif;

/* 得分/计时器数字（等宽，防止数字跳动） */
--font-mono: "SF Mono", "Consolas", "Courier New", monospace;

/* 英文备用（图案名称标注如有） */
--font-en: "Inter", "Segoe UI", sans-serif;
```

**说明：** 不引入外部字体文件，零加载开销，满足 PRD "页面加载 ≤ 2 秒"的要求。

### 3.2 字号阶梯

| Token | px | rem | 用途 |
|-------|-----|-----|------|
| `--text-xs` | 12px | 0.75rem | 版权、次要标签、"剩余X次"角标 |
| `--text-sm` | 14px | 0.875rem | 按钮文字、工具栏标签、辅助说明 |
| `--text-base` | 16px | 1rem | 弹窗正文、得分数值标签 |
| `--text-lg` | 20px | 1.25rem | 计时器数字、工具栏得分数字 |
| `--text-xl` | 24px | 1.5rem | 弹窗标题（"恭喜通关！"） |
| `--text-2xl` | 32px | 2rem | 游戏主标题（页面顶部） |

**说明：** 共 6 阶，实际使用中约束在 4 阶以内（xs 和 2xl 各场景专用）。棋盘内图案不使用文字，由图案图形承载，无需为此单独设字号。

### 3.3 字重

| Token | 值 | 用途 |
|-------|-----|------|
| `--font-regular` | 400 | 正文、说明文字 |
| `--font-medium` | 500 | 按钮文字、标签 |
| `--font-bold` | 700 | 得分数字、弹窗标题、计时器 |

---

## 4. 间距系统

基于 **4px 基准单位**，token 使用 4 的倍数：

| Token | px | 使用场景 |
|-------|-----|---------|
| `--space-1` | 4px | 图标与文字的间距、角标内边距 |
| `--space-2` | 8px | 按钮内横向 padding（紧凑场景）、格子内 padding |
| `--space-3` | 12px | 输入框/小按钮内边距 |
| `--space-4` | 16px | 卡片内边距（标准）、工具栏项目间距 |
| `--space-5` | 20px | 区块之间的垂直间距 |
| `--space-6` | 24px | 弹窗内边距、主要区块间距 |
| `--space-8` | 32px | 页面边缘留白（mobile）、棋盘与工具栏间距 |
| `--space-10` | 40px | 页面边缘留白（desktop）|
| `--space-12` | 48px | 弹窗最大内边距 |

**棋盘格子尺寸：**
- Mobile（<768px）：格子 56px × 56px，格间距 4px
- Desktop（≥768px）：格子 72px × 72px，格间距 6px

棋盘总宽（8列，desktop）：72 × 8 + 6 × 7 = 576px + 42px = 618px

---

## 5. 圆角与阴影

### 5.1 圆角（3阶）

| Token | 值 | 使用场景 |
|-------|-----|---------|
| `--radius-sm` | 4px | 格子（牌格）圆角、角标、标签 |
| `--radius-md` | 8px | 按钮、工具栏容器、计分板 |
| `--radius-lg` | 16px | 弹窗（Modal）、棋盘外框容器 |

**说明：** 格子选最小圆角 4px，保留"方牌"的经典形态；弹窗选最大圆角 16px，增加现代感和亲切感。

### 5.2 阴影（3阶）

| Token | 值 | 使用场景 |
|-------|-----|---------|
| `--shadow-sm` | `0 1px 3px rgba(0,0,0,0.10), 0 1px 2px rgba(0,0,0,0.06)` | 牌格默认态（轻微浮起感） |
| `--shadow-md` | `0 4px 12px rgba(0,0,0,0.12), 0 2px 4px rgba(0,0,0,0.08)` | 工具栏容器、计分板卡片 |
| `--shadow-lg` | `0 16px 48px rgba(0,0,0,0.16), 0 4px 12px rgba(0,0,0,0.10)` | 弹窗（Modal）浮层 |

**Overlay 遮罩：** `background: rgba(0,0,0,0.50); backdrop-filter: blur(2px);`

---

## 6. 核心组件规范

### 6.1 牌格（Tile）

牌格是游戏的核心元素，设计优先级最高。

**尺寸**

| 断点 | 格子尺寸 | 图案区域 | 格间距 |
|------|---------|---------|-------|
| Mobile < 768px | 56 × 56px | 40 × 40px（居中） | 4px |
| Desktop ≥ 768px | 72 × 72px | 52 × 52px（居中） | 6px |

**状态规范**

| 状态 | 背景 | 边框 | 阴影 | 变换 | 说明 |
|------|------|------|------|------|------|
| 默认（idle） | `--gray-100` (#F1F3F5) | 1px solid `--gray-200` | `--shadow-sm` | none | 正常显示图案 |
| 悬停（hover） | `#E8F0FE` | 1px solid `--color-primary` (#4A90D9) | `--shadow-sm` | `translateY(-1px)` | 鼠标经过，轻微上浮 |
| 选中（selected） | `--color-primary-light` (#EBF4FF) | 2px solid `--color-primary` (#4A90D9) | `0 0 0 3px rgba(74,144,217,0.25)` | `scale(1.04)` | 已点击等待配对 |
| 提示（hint） | `--color-accent-light` (#FFF8EC) | 2px solid `--color-accent` (#F5A623) | `0 0 0 3px rgba(245,166,35,0.30)` | 闪烁动画 | 系统提示可消除对 |
| 消除中（matched） | `--color-primary-light` | 2px solid `--color-primary` | — | `scale(1.0)` | 连线动画播放期间保持选中态 |
| 已消除（empty） | transparent | none | none | — | 格子为空，不渲染任何内容 |
| 无效抖动（invalid） | `#FFF5F5` | 2px solid `--color-danger` (#DC3545) | `0 0 0 3px rgba(220,53,69,0.20)` | `shakeX 300ms` | 无效配对 |

**CSS 关键属性（默认态）**

```css
.tile {
  width: 72px;              /* desktop；mobile 改为 56px */
  height: 72px;
  background: var(--gray-100);
  border: 1px solid var(--gray-200);
  border-radius: var(--radius-sm);   /* 4px */
  box-shadow: var(--shadow-sm);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: transform 120ms ease, box-shadow 120ms ease, border-color 120ms ease;
  user-select: none;
}

.tile:hover {
  transform: translateY(-1px);
  border-color: var(--color-primary);
}

.tile--selected {
  background: var(--color-primary-light);
  border: 2px solid var(--color-primary);
  box-shadow: 0 0 0 3px rgba(74, 144, 217, 0.25);
  transform: scale(1.04);
}

.tile--hint {
  border: 2px solid var(--color-accent);
  box-shadow: 0 0 0 3px rgba(245, 166, 35, 0.30);
  animation: hintPulse 600ms ease-in-out 3;
}

.tile--invalid {
  border: 2px solid var(--color-danger);
  animation: shakeX 300ms ease;
}

@keyframes shakeX {
  0%, 100% { transform: translateX(0); }
  20%       { transform: translateX(-6px); }
  40%       { transform: translateX(6px); }
  60%       { transform: translateX(-4px); }
  80%       { transform: translateX(4px); }
}

@keyframes hintPulse {
  0%, 100% { opacity: 1; }
  50%       { opacity: 0.5; }
}
```

**图案渲染说明：**
- MVP 推荐使用 SVG 图标组（16种图案），每张图案 SVG 居中显示在图案区域内
- 备选方案：使用系统 Emoji（注意跨平台渲染差异，参见 PRD Q1）
- 图案大小：desktop 48px，mobile 36px

---

### 6.2 连线（Path Line）

连线是消除操作的核心视觉反馈，使用 Canvas 或 SVG overlay 绘制。

**视觉规格**

| 属性 | 值 |
|------|-----|
| 线宽 | 3px |
| 颜色 | `--color-primary` (#4A90D9) |
| 线端样式 | `lineCap: round`, `lineJoin: round` |
| 起止端圆点 | 半径 5px，填充色同线色 |
| 动画时长 | 300ms（路径逐段绘制 + 淡出消失） |
| 层叠 | Canvas/SVG 覆盖在棋盘之上，`pointer-events: none` |

**动画阶段**

```
0ms      连线开始从格A中心向折点/格B方向延伸绘制（stroke-dashoffset 动画）
200ms    连线完全绘制完毕
200-300ms 连线整体淡出（opacity: 1 → 0）
300ms    格A、格B执行消除（scale(0) + opacity(0)，100ms）
400ms    格子占位消失，棋盘稳定
```

**CSS 关键属性**

```css
.path-canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 10;
}
```

---

### 6.3 按钮（Button）

**三种规格**

**主按钮（Primary）**

| 属性 | 值 |
|------|-----|
| 高度 | 44px |
| 内边距 | 0 24px |
| 背景 | `--color-primary` (#4A90D9) |
| 文字 | `--gray-50` (#F8F9FA)，14px，500 |
| 圆角 | `--radius-md` (8px) |
| 悬停 | background `--color-primary-dark` (#2C6FAC)，`translateY(-1px)` |
| 按下 | `translateY(0)`, 亮度 -5% |
| 禁用 | background `--gray-300`, color `--gray-500`, cursor not-allowed |

```css
.btn-primary {
  height: 44px;
  padding: 0 var(--space-6);
  background: var(--color-primary);
  color: var(--gray-50);
  font-size: var(--text-sm);
  font-weight: var(--font-medium);
  border: none;
  border-radius: var(--radius-md);
  cursor: pointer;
  transition: background 150ms ease, transform 120ms ease;
}
.btn-primary:hover  { background: var(--color-primary-dark); transform: translateY(-1px); }
.btn-primary:active { transform: translateY(0); }
.btn-primary:disabled { background: var(--gray-300); color: var(--gray-500); cursor: not-allowed; }
```

**次级按钮（Secondary）**

| 属性 | 值 |
|------|-----|
| 高度 | 44px |
| 内边距 | 0 20px |
| 背景 | `--gray-50` |
| 边框 | 1.5px solid `--gray-300` |
| 文字 | `--gray-700`，14px，500 |
| 悬停 | border-color `--gray-400`，background `--gray-100` |

**工具栏图标按钮（Icon Button）**

| 属性 | 值 |
|------|-----|
| 尺寸 | 40 × 40px |
| 图标 | 20 × 20px SVG |
| 圆角 | `--radius-md` (8px) |
| 默认背景 | `--gray-100` |
| 悬停背景 | `--gray-200` |
| 次数角标 | 右上角，12px，`--color-primary`文字，白底 |

---

### 6.4 计分板 / 工具栏（HUD）

工具栏是游戏状态的中控区，包含：计时器、得分、提示按钮、洗牌按钮。

**布局规格**

```
总高度：64px（mobile）/ 72px（desktop）
内边距：0 16px
背景：白色 (#FFFFFF)
阴影：--shadow-md
圆角：--radius-lg (16px)，作为卡片浮在棋盘上方
```

**各元素规格**

| 元素 | 宽度 | 字号/字重 | 颜色 |
|------|------|---------|------|
| 计时器图标 | 20px | — | `--gray-500` |
| 计时器数字 | auto | 20px / bold / --font-mono | `--gray-900`（正常）/ `--color-warning` #FFC107（≤30s） |
| 得分标签 | auto | 12px / regular | `--gray-500` |
| 得分数字 | auto | 20px / bold / --font-mono | `--gray-900` |
| 提示按钮 | 40px | 图标20px | 见 Icon Button |
| 洗牌按钮 | 40px | 图标20px | 见 Icon Button |

**倒计时警告态（≤30秒）**

```css
.timer--warning {
  color: var(--color-warning);      /* #FFC107 */
  animation: timerPulse 1s ease-in-out infinite;
}
@keyframes timerPulse {
  0%, 100% { opacity: 1; }
  50%       { opacity: 0.6; }
}
```

**倒计时危险态（≤10秒）**

```css
.timer--danger {
  color: var(--color-danger);       /* #DC3545 */
  animation: timerPulse 500ms ease-in-out infinite;
}
```

---

### 6.5 弹窗（Modal）

**结构规格**

| 属性 | 值 |
|------|-----|
| 最大宽度 | 400px |
| 内边距 | `--space-8` (32px) |
| 圆角 | `--radius-lg` (16px) |
| 阴影 | `--shadow-lg` |
| 遮罩背景 | `rgba(0,0,0,0.50)` |
| 出现动画 | `scaleUp 200ms cubic-bezier(0.34,1.56,0.64,1)`（弹簧感） |

```css
.modal {
  max-width: 400px;
  width: calc(100% - 32px);
  padding: var(--space-8);
  background: #FFFFFF;
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-lg);
  animation: modalIn 200ms cubic-bezier(0.34, 1.56, 0.64, 1);
}
@keyframes modalIn {
  from { opacity: 0; transform: scale(0.88); }
  to   { opacity: 1; transform: scale(1); }
}
```

**内部层级**

```
[图标/emoji 大图 48px]  ← 通关用 ✓，超时用 ✕，可用SVG替代
[标题]                  text-xl (24px), bold, success/danger色
[副标题/说明行]         text-base (16px), --gray-600
[数据行：用时 / 得分]   text-lg (20px), bold, --gray-900
[主按钮：再来一局]
[次级按钮：（可选）]
```

---

## 7. 页面结构与 ASCII 线框

### 7.1 游戏主页（唯一页面）

响应式：mobile-first，≥768px 居中显示棋盘区域。

**Mobile 布局（< 768px，宽度375px为基准）**

```
┌─────────────────────────────────┐  <- 视口顶部
│                                 │
│     连连看         [音效 🔊]    │  <- 顶部导航栏，高度 52px
│                                 │    标题 text-2xl, bold; 音效按钮右侧
├─────────────────────────────────┤
│  ┌───────────────────────────┐  │
│  │ ⏱ 04:32          分数     │  │  <- HUD 工具栏卡片，高64px
│  │         1200  [💡3][🔀2]  │  │    计时器左，得分+按钮右
│  └───────────────────────────┘  │
│                                 │
│  ┌─┬─┬─┬─┬─┬─┬─┬─┐            │  <- 棋盘容器
│  │▣│▣│▣│▣│▣│▣│▣│▣│            │    8列 × 8行，格子56px，间距4px
│  ├─┼─┼─┼─┼─┼─┼─┼─┤            │    棋盘容器居中，两侧16px边距
│  │▣│▣│▣│▣│▣│▣│▣│▣│            │
│  ├─┼─┼─┼─┼─┼─┼─┼─┤            │
│  │▣│▣│▣│▣│▣│▣│▣│▣│            │
│  ├─┼─┼─┼─┼─┼─┼─┼─┤            │
│  │▣│▣│▣│▣│▣│▣│▣│▣│            │
│  ├─┼─┼─┼─┼─┼─┼─┼─┤            │
│  │▣│▣│▣│▣│▣│▣│▣│▣│            │
│  ├─┼─┼─┼─┼─┼─┼─┼─┤            │
│  │▣│▣│▣│▣│▣│▣│▣│▣│            │
│  ├─┼─┼─┼─┼─┼─┼─┼─┤            │
│  │▣│▣│▣│▣│▣│▣│▣│▣│            │
│  ├─┼─┼─┼─┼─┼─┼─┼─┤            │
│  │▣│▣│▣│▣│▣│▣│▣│▣│            │
│  └─┴─┴─┴─┴─┴─┴─┴─┘            │
│                                 │
│        [重新开始按钮]           │  <- 次级按钮，宽度160px
│                                 │
└─────────────────────────────────┘

图例：▣ = 牌格（含图案）
      💡 = 提示按钮（角标显示剩余次数3）
      🔀 = 洗牌按钮（角标显示剩余次数2）
```

**Desktop 布局（≥ 1024px）**

```
┌──────────────────────────────────────────────────────┐
│              连连看                        [🔊]       │  <- 顶部栏，72px
├──────────────────────────────────────────────────────┤
│                                                      │
│      ┌─────────────────────────────────────────┐    │
│      │  ⏱ 04:32          分数: 1200  [💡3][🔀2]│    │  <- HUD，72px高
│      └─────────────────────────────────────────┘    │
│                                                      │
│      ┌─┬─┬─┬─┬─┬─┬─┬─┐                            │
│      │▣│▣│▣│▣│▣│▣│▣│▣│                            │
│      ├─┼─┼─┼─┼─┼─┼─┼─┤                            │
│      │▣│▣│▣│▣│▣│▣│▣│▣│                            │
│      ├─┼─┼─┼─┼─┼─┼─┼─┤                            │
│      │▣│▣│▣│▣│▣│▣│▣│▣│                            │
│      ├─┼─┼─┼─┼─┼─┼─┼─┤                            │
│      │▣│▣│▣│▣│▣│▣│▣│▣│                            │
│      ├─┼─┼─┼─┼─┼─┼─┼─┤                            │  <- 棋盘618px宽，
│      │▣│▣│▣│▣│▣│▣│▣│▣│                            │     格子72px，间距6px
│      ├─┼─┼─┼─┼─┼─┼─┼─┤                            │
│      │▣│▣│▣│▣│▣│▣│▣│▣│                            │
│      ├─┼─┼─┼─┼─┼─┼─┼─┤                            │
│      │▣│▣│▣│▣│▣│▣│▣│▣│                            │
│      ├─┼─┼─┼─┼─┼─┼─┼─┤                            │
│      │▣│▣│▣│▣│▣│▣│▣│▣│                            │
│      └─┴─┴─┴─┴─┴─┴─┴─┘                            │
│                                                      │
│               [重新开始]                             │
│                                                      │
└──────────────────────────────────────────────────────┘

整体内容居中，最大宽度 800px，页面背景 --gray-50
```

---

### 7.2 通关弹窗（Victory Modal）

```
┌────────────────────────────────┐
│             遮罩               │
│  ╔══════════════════════════╗  │
│  ║                          ║  │
│  ║       [✓ 图标 48px]      ║  <- success绿色圆底圆形图标
│  ║                          ║  │
│  ║     恭喜通关！           ║  <- text-xl, --color-success
│  ║   清除了全部图案         ║  <- text-sm, --gray-600
│  ║                          ║  │
│  ║  ┌────────┬───────────┐  ║  │
│  ║  │  用时  │   最终得分 │  ║  <- 数据展示区，两列
│  ║  │ 03:28  │   1680    │  ║     text-lg, bold, --gray-900
│  ║  └────────┴───────────┘  ║  │
│  ║                          ║  │
│  ║  ┌────────────────────┐  ║  │
│  ║  │    再来一局        │  ║  <- 主按钮，full-width
│  ║  └────────────────────┘  ║  │
│  ║                          ║  │
│  ╚══════════════════════════╝  │
└────────────────────────────────┘

弹窗宽度：min(400px, 100vw - 32px)
图标区域：48px圆形，背景#E8F5E9，图标--color-success
```

---

### 7.3 超时/失败弹窗（Timeout Modal）

```
┌────────────────────────────────┐
│             遮罩               │
│  ╔══════════════════════════╗  │
│  ║                          ║  │
│  ║       [✕ 图标 48px]      ║  <- danger红色，圆底
│  ║                          ║  │
│  ║     时间到了！           ║  <- text-xl, --color-danger
│  ║   还剩 12 个图案未消除   ║  <- text-sm, --gray-600
│  ║                          ║  │
│  ║  ┌────────┬───────────┐  ║  │
│  ║  │  本局  │  历史最高  │  ║  <- 数据展示区
│  ║  │  得分  │   得分     │  ║  │
│  ║  │  980   │   1680     │  ║  │
│  ║  └────────┴───────────┘  ║  │
│  ║                          ║  │
│  ║  ┌────────────────────┐  ║  │
│  ║  │    再来一局        │  ║  <- 主按钮
│  ║  └────────────────────┘  ║  │
│  ║                          ║  │
│  ╚══════════════════════════╝  │
└────────────────────────────────┘
```

---

### 7.4 无路可走提示条（No Moves Toast）

非模态弹窗，以 Toast 形式出现在棋盘上方，不强制阻断游戏：

```
┌─────────────────────────────────────┐
│  ⚠  无路可走    [洗牌(剩2次)]  [✕]  │
└─────────────────────────────────────┘
高度：48px
背景：#FFFBF0
边框：1px solid #FFC107
圆角：--radius-md (8px)
位置：棋盘顶部，绝对定位，向下滑入
自动消失：8秒后，或用户点击✕
```

---

## 8. 关键交互规范

### 8.1 选中与配对流程

| 步骤 | 用户动作 | 视觉反馈 | 时长 |
|------|---------|---------|------|
| 1 | 点击格A | 格A进入 `selected` 态（蓝框 + scale(1.04)） | 即时（<16ms） |
| 2a | 点击同一格A | 取消选中，回到 idle 态 | 即时 |
| 2b | 点击格B（不同图案） | 格B抖动（shakeX 300ms），格A保持选中 | 300ms |
| 2c | 点击格B（同图案，不连通） | 格A、格B同时抖动（shakeX 300ms） | 300ms |
| 2d | 点击格B（同图案，连通） | 绘制连线动画 → 格A/B 消除动画 | 300ms连线 + 100ms消除 |

### 8.2 连线动画细节

```
技术方案：SVG overlay + stroke-dasharray/stroke-dashoffset 动画
路径：A中心 → 折点1（如有）→ 折点2（如有）→ B中心
绘制方向：从A到B，顺路径方向
线条颜色：--color-primary (#4A90D9)
线宽：3px

时间轴（总300ms）：
  0   - 200ms : 路径逐段绘制（ease-out）
  200 - 300ms : 整体 opacity 1 → 0（ease-in）
  300ms同时   : 格A、格B scale(1) → scale(0) + opacity(0)，100ms
  400ms       : DOM 中格子内容清除，恢复 empty 态
```

### 8.3 提示功能（Hint）

```
触发：点击💡按钮，剩余次数 > 0
效果：系统计算一对可消除格，两格同时进入 hint 态（橙色闪烁3次，约1800ms）
消失：1800ms 后自动回到 idle 态（无论玩家是否点击）
次数更新：点击即扣减，不论玩家是否成功利用提示
剩余0次：按钮 disabled 态，角标显示"0"
```

### 8.4 洗牌功能（Shuffle）

```
触发：点击🔀按钮，剩余次数 > 0
动画：
  1. 所有格子同时 opacity 1 → 0（150ms）
  2. 重新排列图案位置（0ms，后台计算）
  3. 所有格子 opacity 0 → 1，伴随 scale(0.9)→scale(1)（200ms, stagger 10ms）
总动画时长：约 400ms
洗牌期间禁止点击棋盘
```

### 8.5 倒计时警告

| 剩余时间 | 计时器状态 | 额外反馈 |
|---------|---------|---------|
| > 30秒 | 正常色 `--gray-900` | 无 |
| ≤ 30秒 | `--color-warning` (#FFC107)，timerPulse 动画 1s周期 | 无 |
| ≤ 10秒 | `--color-danger` (#DC3545)，timerPulse 动画 500ms周期 | 数字加粗（已是bold，可考虑字号升一级至24px） |
| 0秒 | 停止 | 超时弹窗弹出（modalIn 动画） |

### 8.6 空状态（棋盘生成中）

```
场景：页面首次加载或重新开始时棋盘生成（纯前端，应 < 100ms）
处理：棋盘区域显示骨架屏（格子位置用 --gray-200 占位，无图案）
     骨架屏显示不超过500ms，超出则说明性能问题
骨架屏格子样式：background: --gray-200，无边框，无阴影，无动画
```

### 8.7 错误状态

**场景1：棋盘无解（生成算法失败）**
```
处理：静默重新生成（对用户不可见），不展示错误信息
最大重试3次，3次后显示 Toast：
"棋盘生成失败，请刷新页面"
Toast样式：background #FFF5F5, border --color-danger, 不自动消失
```

**场景2：无路可走（游戏进行中检测到）**
```
处理：显示 No Moves Toast（见 7.4）
系统不强制洗牌，由玩家决定（参见 PRD Q3 开放问题）
```

### 8.8 响应式断点行为

| 断点 | 宽度 | 棋盘格尺寸 | 布局变化 |
|------|------|---------|---------|
| xs（mobile） | < 480px | 52px | 棋盘两侧 8px 边距，HUD 单行紧凑 |
| sm（mobile+）| 480–767px | 56px | 棋盘两侧 16px 边距 |
| md（tablet） | 768–1023px | 64px | 内容居中，最大宽度 720px |
| lg（desktop）| ≥ 1024px | 72px | 内容居中，最大宽度 800px |

**小屏优化（< 480px）：**
- HUD 工具栏计时器和得分并排，提示/洗牌按钮收到右侧
- "重新开始"按钮文字缩短为"重玩"
- Toast 宽度 100%，固定在视口底部

---

## 9. 设计 Token 速查表

```css
:root {
  /* 主色 */
  --color-primary:       #4A90D9;
  --color-primary-light: #EBF4FF;
  --color-primary-dark:  #2C6FAC;

  /* 辅色 */
  --color-accent:        #F5A623;
  --color-accent-light:  #FFF8EC;

  /* 中性色 */
  --gray-50:  #F8F9FA;
  --gray-100: #F1F3F5;
  --gray-200: #E9ECEF;
  --gray-300: #DEE2E6;
  --gray-400: #CED4DA;
  --gray-500: #ADB5BD;
  --gray-600: #6C757D;
  --gray-700: #495057;
  --gray-900: #212529;

  /* 语义色 */
  --color-success: #28A745;
  --color-danger:  #DC3545;
  --color-warning: #FFC107;
  --color-info:    #17A2B8;

  /* 字体 */
  --font-sans:    "PingFang SC", "Microsoft YaHei", "Helvetica Neue", Arial, sans-serif;
  --font-mono:    "SF Mono", "Consolas", "Courier New", monospace;
  --font-regular: 400;
  --font-medium:  500;
  --font-bold:    700;

  /* 字号 */
  --text-xs:   12px;
  --text-sm:   14px;
  --text-base: 16px;
  --text-lg:   20px;
  --text-xl:   24px;
  --text-2xl:  32px;

  /* 间距 */
  --space-1:  4px;
  --space-2:  8px;
  --space-3:  12px;
  --space-4:  16px;
  --space-5:  20px;
  --space-6:  24px;
  --space-8:  32px;
  --space-10: 40px;
  --space-12: 48px;

  /* 圆角 */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 16px;

  /* 阴影 */
  --shadow-sm: 0 1px 3px rgba(0,0,0,0.10), 0 1px 2px rgba(0,0,0,0.06);
  --shadow-md: 0 4px 12px rgba(0,0,0,0.12), 0 2px 4px rgba(0,0,0,0.08);
  --shadow-lg: 0 16px 48px rgba(0,0,0,0.16), 0 4px 12px rgba(0,0,0,0.10);
}
```

---

## 10. 设计总结

**气质定位：** 这款连连看定位"清爽、亲切、即时"——以浅灰暖底色降低视觉压力，让玩家在碎片时间快速放松；圆角和阴影保持克制但不失层次，让"棋盘"有实体感而非纸片感。

**主色选择理由：** `#4A90D9` 中蓝色延续了 QQ 连连看时代的集体记忆，唤起"经典规则"的情感认同，同时饱和度适中不刺眼，在连线动画中（3px蓝线）辨识度极高，与橙色 Accent 形成明确的"配对成功"与"提示引导"的视觉分野。

**最关键的三个组件决策：**

1. **牌格的 scale(1.04) 选中态**：不用纯色底填充做选中，而是叠加蓝色边框 + 外发光 + 轻微放大，既在密集棋盘中突出，又不覆盖图案导致辨识度下降。这是"零门槛点击反馈"的核心。

2. **连线使用 SVG overlay + stroke-dashoffset 动画**：比 Canvas 更易实现路径动画且 CSS 可控，300ms 总时长（200ms绘制 + 100ms淡出）在"即时感"和"能看清发生了什么"之间取得平衡；之后格子消除动画100ms，总时长控制在400ms内，不让玩家等待。

3. **倒计时两阶段变色（橙 → 红 + 脉冲动画）**：不用声音（MVP 可关闭音效），改用颜色和动画节奏来传递紧迫感的变化层次。30秒缓慢橙色脉冲是"提醒"，10秒快速红色脉冲是"警报"，两者频率差异让玩家下意识感受到压力递增，无需阅读任何文字说明。
