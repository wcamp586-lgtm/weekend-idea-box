# 周末灵感盒：设计与实现基准

## 1. 视觉概念

- 桌面参考：[`design/desktop-concept.png`](design/desktop-concept.png)
- 手机参考：[`design/mobile-concept.png`](design/mobile-concept.png)

视觉方向是“编辑感的周末票根”：干净、轻松、成年化，不做幼稚卡通，也不做常见的卡片仪表盘。

## 2. 允许出现的首屏文案

- 周末灵感盒
- 添加灵感
- 这个周末，做点什么？
- 收好每个念头，想出发时就不用再纠结。
- 随机抽一个
- 搜索灵感
- 全部、电影、游戏、地方、活动
- 想做、进行中、已完成
- 灵感总览

后续实现不得为“显得丰富”擅自增加营销口号、标签、假指标或导航项。

## 3. 布局规则

### 桌面

- 顶部只保留品牌和一个“添加灵感”主操作。
- 主区域使用开放式双栏：左侧承载票根标题、筛选和列表；右侧承载随机操作与总览。
- 列表使用横向分隔线和稳定列对齐，不改成卡片网格。
- 空状态只在没有数据或没有结果时替换列表，不与有数据列表同时出现。概念图把两种状态并列展示仅用于说明视觉语言。

### 手机

- 按“标题 → 随机操作 → 搜索和筛选 → 列表 → 总览”的顺序纵向重排。
- 分类筛选可以横向滚动，但页面本身不得横向溢出。
- 点击目标不小于约 44px，编辑和删除按钮必须有可访问名称。

## 4. 设计令牌

最终实现以概念图视觉比对为准，初始令牌如下：

```css
--color-background: #ffffff;
--color-ink: #0e1b35;
--color-muted: #67738a;
--color-border: #dbe2ec;
--color-ticket: #fffaf2;
--color-orange: #ff5b0a;
--color-blue: #3f8cff;
--color-green: #20ad60;
--radius-control: 10px;
--radius-panel: 14px;
--shadow-soft: 0 10px 30px rgb(14 27 53 / 8%);
```

中文字体优先使用系统字体：`Microsoft YaHei`、`PingFang SC`、`Noto Sans SC`，最后回退到 `sans-serif`。不依赖远程字体才能显示主要内容。

## 5. 组件清单

- `AppShell`：页面最大宽度和响应式边界。
- `Header`：品牌与添加按钮。
- `HeroTicket`：主标题和说明。
- `RandomPicker`：圆形主操作和抽取结果。
- `FilterBar`：搜索、分类和状态。
- `InspirationList` / `InspirationRow`：开放式列表。
- `Summary`：全部及三个状态数量。
- `EmptyState`：无数据或无结果。
- `InspirationDialog`：新增和编辑表单。

`App` 只负责组合，不堆放所有细节。重复控件使用共享组件，状态差异通过明确的 variant 表达。

## 6. 图标与动效

- 图标统一使用约 2px 的圆角线性风格；新增、搜索、随机、编辑、删除和四种分类必须保持同一家族视觉。
- 随机按钮可以在触发时轻微旋转或弹动；新增成功可以短暂强调新行。
- 所有动效尊重 `prefers-reduced-motion`。

## 7. 视觉验收

每完成一个主要区域都要在真实浏览器截图，与概念图逐项比较：文案、布局、字体层级、颜色、间距、图标、响应式和交互状态。生产构建通过不能代替视觉验收。
