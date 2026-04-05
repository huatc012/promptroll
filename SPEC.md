# 📜 PromptRoll — 手机提词器

## Concept & Vision

一款为短视频编导设计的专业提词器。不是通用工具箱里抽出来的功能，而是专为"对着镜头说话"这个场景打磨的产品感工具。

**核心体验：** 打开就能用，文案贴进去，台词滚起来，镜头前你就是主角。

**氛围关键词：** 专业感、暗色系、沉浸式、低调但精准

---

## Design Language

### Aesthetic Direction
**深空黑 + 琥珀金点缀** — 深色界面减少屏幕对眼睛的刺激，琥珀色作为交互反馈色，传递"专业影视设备"的气质，区别于普通的工具类App。

### Color Palette
```css
--bg-primary: #0A0A0C;        /* 深空黑背景 */
--bg-surface: #141416;        /* 卡片/面板背景 */
--bg-glass: rgba(20, 20, 22, 0.85);  /* 毛玻璃面板 */
--text-primary: #F5F5F7;       /* 主文字 */
--text-secondary: #8E8E93;     /* 次要文字 */
--accent: #E8A445;             /* 琥珀金 - 交互色 */
--accent-glow: rgba(232, 164, 69, 0.3);
--border: rgba(255, 255, 255, 0.08);
--danger: #FF453A;
```

### Typography
- **提词文字：** `"PingFang SC", "SF Pro Display", "Helvetica Neue", sans-serif`
  - 字重 500-700 可调
  - 默认 48px（可缩放 28px-96px）
  - 行高 1.6-2.0 可调
- **UI 文字：** `"SF Pro Text", -apple-system, sans-serif`
  - 12px / 14px / 16px

### Spatial System
- 边距安全区：20px
- 控制面板圆角：16px
- 按钮圆角：12px（小）/ 20px（大）
- 阴影：`0 8px 32px rgba(0,0,0,0.6)`

### Motion Philosophy
- 控制面板：淡入淡出 + 轻微上移，300ms ease-out
- 速度调节：实时响应，无延迟
- 滚动：60fps 平滑滚动，CSS scroll-behavior + requestAnimationFrame
- 按钮反馈：scale(0.95) → scale(1)，150ms

---

## Layout & Structure

### 主界面布局

```
┌─────────────────────────────────┐
│  ░░░░░░░░░░░░░░░░░░░░░░░░░░░░  │  ← 状态栏占位
│                                 │
│     （巨大的提词文字区域）         │
│                                 │
│     文案内容持续向上滚动           │
│                                 │
│                                 │
├─────────────────────────────────┤
│  ┌─────────────────────────┐   │
│  │ 🎬 开始  ⏸ 暂停  ⏹ 重置  │   │  ← 底部控制面板（可隐藏）
│  │ 速度━━●━━━  镜像  字号    │   │
│  └─────────────────────────┘   │
└─────────────────────────────────┘
```

### 状态
- **待机态：** 显示文案输入区 + 编辑按钮
- **提词态：** 全屏沉浸，文字滚动，控制面板可隐藏
- **暂停态：** 滚动停止，文字定格

### 响应策略
- 竖屏优先（手机拍摄场景）
- 横屏支持（iPad 或专业场景）
- 安全区适配（刘海/HomeBar）

---

## Features & Interactions

### 1. 文案输入
- 点击主区域弹出文案输入弹窗（textarea）
- 支持直接粘贴（从剪映/CapCut/备忘录复制）
- 字数统计显示
- 保存/加载历史文案（localStorage，最多10条）

### 2. 滚动控制
- **播放/暂停：** 中央大按钮，点击切换
- **速度：** 范围 20-200 px/秒，默认 60
  - 滑块调节，实时预览
  - 也支持 + / - 按钮微调
- **重置：** 滚动归零，回到开头

### 3. 镜像模式
- 开关切换
- 开启后文字水平翻转（CSS transform: scaleX(-1)）
- 适合对着玻璃/反射屏拍摄的场景
- 开关状态图标：👁️ / 👁️‍🗨️

### 4. 字号调节
- 滑块：28px - 96px，默认 48px
- 预览文字实时更新
- 行高同步调整

### 5. 控制面板显示/隐藏
- 点击任意空白区域切换显示/隐藏
- 隐藏后全屏沉浸，文字填满整屏
- 显示时有微妙的毛玻璃效果

### 6. 快捷操作
- 双击屏幕中央：播放/暂停
- 双击屏幕左侧：速度 -10
- 双击屏幕右侧：速度 +10
- 长按屏幕底部：显示控制面板

---

## Component Inventory

### TeleprompterView（提词主视图）
- 全屏文字容器
- 滚动区域 + 平滑滚动动画
- 镜像翻转支持
- 当前阅读位置指示线（半透明琥珀色）

### ControlPanel（控制面板）
- 毛玻璃背景
- 播放控制按钮组
- 速度滑块
- 镜像开关
- 字号滑块
- 编辑文案按钮
- 收起/展开切换

### ScriptInputModal（文案输入弹窗）
- 全屏覆盖
- 大 textarea
- 字数统计
- 历史记录列表
- 确认/取消按钮

### SpeedIndicator（速度指示）
- 当前速度数字显示
- 播放时显示，暂停时淡出

---

## Technical Approach

### 技术栈
- **纯 HTML + CSS + JavaScript** — 无框架依赖，单文件可运行
- **CSS 变量** — 主题色和动态样式
- **localStorage** — 文案历史记录持久化
- **requestAnimationFrame** — 60fps 滚动
- **Touch Events** — 手势支持
- **CSS transform: scaleX(-1)** — 镜像模式

### 关键实现
1. 滚动使用 `transform: translateY()` + `requestAnimationFrame`，避免 layout thrashing
2. 镜像模式仅需一行 CSS，性能无损
3. 毛玻璃效果使用 `backdrop-filter: blur(20px)`
4. PWA 支持：manifest.json + service worker（可选）

### 文件结构
```
teleprompter/
├── SPEC.md
├── index.html      ← 主文件，全部内联
└── manifest.json   ← PWA 配置（可选）
```

---

## Content Defaults

首次打开时的示例文案：

> 大家好，我是赵赫。
> 今天想跟大家聊聊，为什么我认为中国企业的下一个机会，在非洲。
> 不是情怀，不是冒险，是数据和判断。
> 我在埃塞俄比亚待了四个月，看到了很多国内看不到的东西。
> ...

（提示：记得替换成你自己的真实内容）
