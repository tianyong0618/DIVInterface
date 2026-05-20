# 承建方执行视角大屏 - 完全替换实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将 `project-dashboard/chengjianfang.html` 完全替换为 Figma 原型 `ContractorDashboard.tsx` 的 HTML/CSS/JS 实现，复用 `common.css` 设计系统。

**Architecture:** 单文件 HTML：内联 CSS（扩展 common.css）+ 内联 JS（ECharts 图表 + 原生交互），无框架依赖。所有 Mock 数据静态内联。

**Tech Stack:** 纯 HTML5 + CSS3 + JavaScript (ES6) + ECharts 5 (CDN)

---

### Task 1: HTML 结构骨架

**Files:**
- Rewrite: `project-dashboard/chengjianfang.html` (完全替换)

- [ ] **Step 1: 写入 HTML 文档框架 + Header 区域**

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI项目落地执行作战大屏</title>
    <link rel="stylesheet" href="css/common.css">
    <script src="https://cdn.jsdelivr.net/npm/echarts@5/dist/echarts.min.js"></script>
    <style>
        /* (CSS will be added in Task 2) */
    </style>
</head>
<body>
<div class="dashboard-container">
    <!-- Header -->
    <header class="header">
        <div class="header-title">
            <h1>AI项目落地执行作战大屏</h1>
        </div>
        <div class="header-info-left">
            <div class="header-info-item">
                <span class="header-info-label">甲方单位</span>
                <span class="header-info-value">某省政务信息中心</span>
            </div>
            <div class="header-info-item">
                <span class="header-info-label">实施阶段</span>
                <span class="header-info-value">硬件部署 + 软件研发并行</span>
            </div>
            <div class="header-info-item">
                <span class="header-info-label">内部考核</span>
                <span class="header-info-value" style="color: var(--secondary-color);">85分</span>
            </div>
        </div>
        <div class="header-info-right">
            <div class="countdown-box">
                <span class="countdown-label">距离竣工</span>
                <span class="countdown-value">133天</span>
            </div>
            <span class="header-info-label" id="updateTime"></span>
        </div>
    </header>

    <!-- Row 1: AI排程 + 子系统进度 -->
    <div class="main-grid">
        <!-- AI智能排程 - 左2/3 -->
        <div class="card ai-schedule-card">
            <div class="card-title">AI智能排程与进度调度</div>
            <!-- 3指标卡 -->
            <div class="ai-metrics-row">
                <!-- 整体完成率 -->
                <div class="metric-card">
                    <div class="metric-label">整体完成率</div>
                    <div class="metric-value cyan">68.5%</div>
                    <div class="progress-bar"><div class="progress-fill cyan" style="width:68.5%"></div></div>
                </div>
                <!-- 本周任务达成 -->
                <div class="metric-card">
                    <div class="metric-label">本周任务达成</div>
                    <div class="metric-value green">87%</div>
                    <div class="progress-bar"><div class="progress-fill green-fill" style="width:87%"></div></div>
                </div>
                <!-- 滞后工序 -->
                <div class="metric-card">
                    <div class="metric-label">滞后工序</div>
                    <div class="metric-value red">3项</div>
                    <div class="metric-sub">需抢工</div>
                </div>
            </div>
            <!-- AI建议 -->
            <div class="ai-alert blue-alert">
                <strong>AI优化建议:</strong> 系统集成环节滞后，建议将部分开发人员提前介入联调工作，硬件部署与软件开发可并行进行，预计可缩短15天工期。
            </div>
            <!-- 周度任务图表 -->
            <h4 class="section-subtitle">周度任务执行情况</h4>
            <div id="chartSchedule" class="chart-box"></div>
        </div>

        <!-- 子系统进度 - 右1/3 -->
        <div class="card subsystem-card">
            <div class="card-title">各子系统建设进度</div>
            <div id="subsystemList" class="subsystem-list">
                <!-- JS渲染 -->
            </div>
        </div>
    </div>

    <!-- Row 2: 分项业务实施管控 (Tabs) -->
    <div class="card impl-card" style="margin-top: var(--spacing-lg);">
        <div class="card-title">分项业务实施管控</div>
        <div class="tabs-container">
            <div class="tabs-header">
                <button class="tab-btn active" data-tab="software">软件研发</button>
                <button class="tab-btn" data-tab="hardware">硬件实施</button>
                <button class="tab-btn" data-tab="integration">系统集成</button>
            </div>
            <div class="tab-content active" id="tabSoftware">
                <!-- 软件研发内容 -->
                <div class="tab-grid-2">
                    <div>
                        <h4 class="section-subtitle">模块开发进度</h4>
                        <div id="softwareModules" class="progress-list"></div>
                    </div>
                    <div>
                        <h4 class="section-subtitle">Bug修复统计</h4>
                        <div class="bug-stats">
                            <div class="bug-stat-item"><span class="bug-label">待修复Bug</span><span class="bug-value red">12</span></div>
                            <div class="bug-stat-item"><span class="bug-label">本周修复</span><span class="bug-value green">28</span></div>
                            <div class="bug-stat-item"><span class="bug-label">累计修复</span><span class="bug-value cyan">156</span></div>
                        </div>
                        <div class="ai-alert green-alert" style="margin-top:var(--spacing-md)">Bug修复效率良好，建议继续保持代码质量管控。</div>
                    </div>
                </div>
            </div>
            <div class="tab-content" id="tabHardware">
                <!-- 硬件实施表格 -->
                <div class="table-wrapper">
                    <table class="data-table">
                        <thead>
                            <tr><th>实施项目</th><th>计划点位</th><th>已完成</th><th>完成率</th><th>状态</th></tr>
                        </thead>
                        <tbody id="hardwareTableBody"></tbody>
                    </table>
                </div>
            </div>
            <div class="tab-content" id="tabIntegration">
                <div class="tab-grid-2">
                    <div>
                        <h4 class="section-subtitle">集成进度雷达图</h4>
                        <div id="chartRadar" class="chart-box" style="height:280px;"></div>
                    </div>
                    <div>
                        <h4 class="section-subtitle">第三方对接进度</h4>
                        <div id="thirdPartyList" class="progress-list"></div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Row 3: 成本管控 + 人员看板 -->
    <div class="grid-2" style="margin-top: var(--spacing-lg);">
        <!-- 内部成本 -->
        <div class="card">
            <div class="card-title">内部成本管控</div>
            <div id="chartCost" class="chart-box"></div>
            <div class="cost-summary">
                <div class="cost-summary-card">
                    <span class="cost-label">总投入成本</span>
                    <span class="cost-value">5730万</span>
                </div>
                <div class="cost-summary-card">
                    <span class="cost-label">成本使用率</span>
                    <span class="cost-value green">88.5%</span>
                </div>
            </div>
            <div class="ai-alert blue-alert" style="margin-top:var(--spacing-sm)">成本控制良好，预算执行率在合理范围内。</div>
        </div>

        <!-- 人员看板 -->
        <div class="card">
            <div class="card-title">人员班组任务看板</div>
            <div class="table-wrapper">
                <table class="data-table">
                    <thead>
                        <tr><th>姓名</th><th>岗位</th><th>任务</th><th>完成</th><th>效率</th></tr>
                    </thead>
                    <tbody id="teamTableBody"></tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- Row 4: 现场问题&风险化解 -->
    <div class="card" style="margin-top: var(--spacing-lg);">
        <div class="card-title">现场问题&风险化解专区</div>
        <div class="table-wrapper">
            <table class="data-table">
                <thead>
                    <tr><th>问题编号</th><th>类型</th><th>问题描述</th><th>优先级</th><th>AI匹配方案</th></tr>
                </thead>
                <tbody id="issuesTableBody"></tbody>
            </table>
        </div>
        <div class="ai-alert purple-alert" style="margin-top:var(--spacing-md)">
            <strong>AI智能提示:</strong> 系统已从历史项目库匹配到3个相似技术难题的解决方案，建议优先参考"某市政务云项目"的数据库优化经验。
        </div>
    </div>

</div>
<script>
/* (JS will be added in Tasks 3-5) */
</script>
</body>
</html>
```

- [ ] **Step 2: 确认 HTML 结构完整性**

验证要点：
- 所有 chart 容器 div 有唯一 id（chartSchedule, chartRadar, chartCost）
- 所有 JS 动态渲染容器有 id（subsystemList, softwareModules, hardwareTableBody, thirdPartyList, teamTableBody, issuesTableBody）
- Header 布局使用 header-info-left / header-info-right
- Tab 容器结构：tabs-container > tabs-header (button[data-tab]) + tab-content[id]
- grid-2 用于成本+人员看板行

---

### Task 2: CSS 样式

**Files:**
- Modify: `project-dashboard/chengjianfang.html` (在 `<style>` 中追加)

- [ ] **Step 1: 写入全部内联 CSS**

追加到 `<style>` 标签内：

```css
/* ===== 覆盖标题栏为承建方风格 ===== */
.header {
    grid-template-columns: 1fr auto;
}

/* ===== 承建方三列布局覆盖 ===== */
.main-grid {
    display: grid;
    grid-template-columns: 2fr 1fr;
    gap: var(--spacing-lg);
}

/* ===== AI排程指标卡片行 ===== */
.ai-metrics-row {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: var(--spacing-md);
    margin-bottom: var(--spacing-md);
}

.metric-card {
    background: rgba(0, 0, 0, 0.3);
    padding: var(--spacing-md);
    border-radius: var(--radius-md);
    text-align: center;
    border: 1px solid rgba(0, 212, 255, 0.1);
}

.metric-label {
    font-size: 12px;
    color: var(--text-muted);
    margin-bottom: var(--spacing-xs);
}

.metric-value {
    font-size: 28px;
    font-weight: 700;
    margin-bottom: var(--spacing-sm);
}

.metric-value.cyan { color: var(--primary-color); text-shadow: 0 0 20px var(--primary-glow); }
.metric-value.green { color: var(--secondary-color); text-shadow: 0 0 20px rgba(0,255,136,0.5); }
.metric-value.red { color: var(--danger-color); text-shadow: 0 0 20px rgba(255,77,79,0.5); }

.metric-sub {
    font-size: 12px;
    color: var(--text-muted);
    margin-top: var(--spacing-xs);
}

/* 进度条绿色变体 */
.progress-fill.green-fill {
    background: linear-gradient(90deg, var(--secondary-color), #66ffb2);
}

/* ===== AI建议 Alert ===== */
.ai-alert {
    padding: var(--spacing-sm) var(--spacing-md);
    border-radius: var(--radius-md);
    font-size: 12px;
    line-height: 1.6;
}

.ai-alert strong {
    font-weight: 700;
}

.blue-alert {
    background: rgba(0, 212, 255, 0.08);
    border: 1px solid rgba(0, 212, 255, 0.3);
    color: rgba(255, 255, 255, 0.85);
}

.green-alert {
    background: rgba(0, 255, 136, 0.08);
    border: 1px solid rgba(0, 255, 136, 0.3);
    color: rgba(255, 255, 255, 0.85);
}

.purple-alert {
    background: rgba(168, 85, 247, 0.08);
    border: 1px solid rgba(168, 85, 247, 0.3);
    color: rgba(255, 255, 255, 0.85);
}

/* ===== 副标题 ===== */
.section-subtitle {
    font-size: 13px;
    font-weight: 600;
    color: var(--text-secondary);
    margin-bottom: var(--spacing-sm);
    margin-top: var(--spacing-md);
}

/* ===== 子系统进度列表 ===== */
.subsystem-list {
    display: flex;
    flex-direction: column;
    gap: var(--spacing-md);
}

.subsystem-item {
    background: rgba(0, 0, 0, 0.25);
    padding: var(--spacing-sm) var(--spacing-md);
    border-radius: var(--radius-md);
}

.subsystem-header {
    display: flex;
    justify-content: space-between;
    margin-bottom: var(--spacing-xs);
}

.subsystem-name { font-size: 13px; color: var(--text-primary); }
.subsystem-progress-text { font-size: 13px; color: var(--text-muted); }

.subsystem-warning {
    font-size: 11px;
    color: var(--warning-color);
    margin-top: var(--spacing-xs);
}

/* ===== Tab 切换 ===== */
.tabs-container { margin-top: var(--spacing-sm); }

.tabs-header {
    display: flex;
    gap: var(--spacing-xs);
    margin-bottom: var(--spacing-md);
    border-bottom: 1px solid var(--bg-border);
    padding-bottom: var(--spacing-sm);
}

.tab-btn {
    padding: var(--spacing-sm) var(--spacing-lg);
    background: transparent;
    border: 1px solid transparent;
    border-radius: var(--radius-md);
    color: var(--text-muted);
    font-size: 13px;
    cursor: pointer;
    transition: all 0.3s ease;
    font-family: var(--font-family);
}

.tab-btn:hover {
    color: var(--text-primary);
    background: rgba(0, 212, 255, 0.05);
}

.tab-btn.active {
    color: var(--text-primary);
    background: rgba(0, 212, 255, 0.12);
    border-color: rgba(0, 212, 255, 0.3);
}

.tab-content { display: none; }
.tab-content.active { display: block; }

.tab-grid-2 {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: var(--spacing-lg);
}

/* ===== 进度列表 (用于模块/第三方) ===== */
.progress-list { display: flex; flex-direction: column; gap: var(--spacing-md); }

.progress-item {
    background: rgba(0, 0, 0, 0.25);
    padding: var(--spacing-sm) var(--spacing-md);
    border-radius: var(--radius-md);
}

.progress-item-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: var(--spacing-xs);
}

.progress-item-name { font-size: 12px; color: var(--text-primary); }
.progress-item-status { font-size: 10px; }

/* ===== Bug统计 ===== */
.bug-stats {
    display: flex;
    flex-direction: column;
    gap: var(--spacing-md);
    background: rgba(0, 0, 0, 0.25);
    padding: var(--spacing-md);
    border-radius: var(--radius-md);
}

.bug-stat-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.bug-label { font-size: 13px; color: var(--text-muted); }
.bug-value { font-size: 22px; font-weight: 700; }
.bug-value.red { color: var(--danger-color); }
.bug-value.green { color: var(--secondary-color); }
.bug-value.cyan { color: var(--primary-color); }

/* ===== 数据表格 ===== */
.table-wrapper { overflow-x: auto; }

.data-table {
    width: 100%;
    border-collapse: separate;
    border-spacing: 0;
    font-size: 12px;
}

.data-table thead {
    background: rgba(0, 212, 255, 0.06);
}

.data-table th {
    padding: var(--spacing-sm) var(--spacing-md);
    text-align: left;
    font-weight: 600;
    color: var(--primary-color);
    border-bottom: 1px solid var(--bg-border);
    white-space: nowrap;
}

.data-table tbody tr {
    background: rgba(0, 0, 0, 0.2);
    transition: background 0.3s ease;
}

.data-table tbody tr:hover {
    background: rgba(0, 212, 255, 0.06);
}

.data-table td {
    padding: var(--spacing-sm) var(--spacing-md);
    border-bottom: 1px solid rgba(255, 255, 255, 0.04);
    color: rgba(255, 255, 255, 0.85);
}

/* ===== Badge 标签 ===== */
.badge {
    display: inline-block;
    padding: 2px 8px;
    border-radius: 3px;
    font-size: 11px;
    font-weight: 600;
}

.badge-default { background: rgba(0, 212, 255, 0.15); color: var(--primary-color); }
.badge-secondary { background: rgba(255, 255, 255, 0.1); color: var(--text-muted); }
.badge-destructive { background: rgba(255, 77, 79, 0.2); color: var(--danger-color); }
.badge-success { background: rgba(0, 255, 136, 0.15); color: var(--secondary-color); }
.badge-warning { background: rgba(255, 159, 67, 0.2); color: var(--warning-color); }

.badge-icon {
    display: inline-flex;
    align-items: center;
    gap: 4px;
}

/* ===== 成本汇总 ===== */
.cost-summary {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: var(--spacing-md);
    margin-top: var(--spacing-md);
}

.cost-summary-card {
    background: rgba(0, 0, 0, 0.25);
    padding: var(--spacing-md);
    border-radius: var(--radius-md);
    text-align: center;
}

.cost-label { font-size: 12px; color: var(--text-muted); display: block; margin-bottom: var(--spacing-xs); }
.cost-value { font-size: 22px; font-weight: 700; color: var(--text-primary); }
.cost-value.green { color: var(--secondary-color); }

/* ===== 图表容器 ===== */
.chart-box { width: 100%; height: 200px; }

/* ===== 倒计时框 (Header右侧) ===== */
.countdown-box {
    display: flex;
    align-items: center;
    gap: var(--spacing-sm);
    padding: 6px 14px;
    background: rgba(255, 159, 67, 0.12);
    border: 1px solid rgba(255, 159, 67, 0.3);
    border-radius: var(--radius-md);
}

.countdown-label { font-size: 11px; color: var(--text-muted); }
.countdown-value { font-size: 22px; font-weight: 700; color: var(--warning-color); }

/* ===== 卡片淡入动画 ===== */
.card {
    opacity: 0;
    transform: translateY(20px);
    animation: cardFadeIn 0.6s ease forwards;
}
@keyframes cardFadeIn {
    to { opacity: 1; transform: translateY(0); }
}

/* ===== 响应式 ===== */
@media (max-width: 1200px) {
    .main-grid { grid-template-columns: 1fr; }
    .ai-metrics-row { grid-template-columns: repeat(3, 1fr); }
    .tab-grid-2 { grid-template-columns: 1fr; }
}

@media (max-width: 768px) {
    .ai-metrics-row { grid-template-columns: 1fr; }
    .cost-summary { grid-template-columns: 1fr; }
    .header { grid-template-columns: 1fr; }
    .header-info-left { flex-wrap: wrap; gap: var(--spacing-sm); }
}
```

- [ ] **Step 2: 验证 CSS 效果**

检查：
- `.card` 淡入动画避免与 common.css 冲突（common.css 的 `.card` 有背景色，这里只追加动画）
- 所有 className 与 HTML 结构匹配
- 响应式断点覆盖 1200px 和 768px
- `progress-fill.green-fill` 不冲突（common.css 有 `.progress-fill.cyan/orange/red`）

---

### Task 3: JS - Mock 数据 + 工具函数

**Files:**
- Modify: `project-dashboard/chengjianfang.html` (在 `<script>` 中追加)

- [ ] **Step 1: 写入 Mock 数据和工具函数**

```javascript
/* ============================================================
   Mock 数据
   ============================================================ */
const WEEKLY_TASKS = [
    { week: 'W18', planned: 45, completed: 42 },
    { week: 'W19', planned: 50, completed: 48 },
    { week: 'W20', planned: 48, completed: 35 },
    { week: 'W21', planned: 52, completed: 0 },
];

const SYSTEM_PROGRESS = [
    { system: '用户管理', progress: 85 },
    { system: '权限控制', progress: 90 },
    { system: '数据分析', progress: 60 },
    { system: '报表系统', progress: 45 },
    { system: '接口集成', progress: 70 },
];

const SOFTWARE_MODULES = [
    { name: '用户管理模块', progress: 100, status: '已完成', statusType: 'success' },
    { name: '权限控制模块', progress: 100, status: '已完成', statusType: 'success' },
    { name: '数据分析模块', progress: 75, status: '开发中', statusType: 'default' },
    { name: '报表系统', progress: 60, status: '开发中', statusType: 'default' },
    { name: '移动端适配', progress: 30, status: '进行中', statusType: 'secondary' },
];

const HARDWARE_DATA = [
    { item: '服务器部署', planned: 12, completed: 10, rate: 83 },
    { item: '网络设备安装', planned: 45, completed: 42, rate: 93 },
    { item: '终端设备配置', planned: 120, completed: 95, rate: 79 },
    { item: '安全设备部署', planned: 8, completed: 8, rate: 100 },
    { item: '存储设备安装', planned: 6, completed: 5, rate: 83 },
];

const RADAR_DATA = [
    { subject: '前后端联调', value: 85 },
    { subject: '第三方接口', value: 60 },
    { subject: '数据库集成', value: 90 },
    { subject: '认证授权', value: 75 },
    { subject: '消息队列', value: 70 },
];

const THIRD_PARTY = [
    { system: '统一认证平台', status: '已完成', progress: 100, statusType: 'success' },
    { system: 'OA办公系统', status: '联调中', progress: 70, statusType: 'default' },
    { system: '财务管理系统', status: '待对接', progress: 0, statusType: 'secondary' },
    { system: '人事管理系统', status: '联调中', progress: 50, statusType: 'default' },
];

const COST_DATA = [
    { category: '人力成本', amount: 3200, budget: 3500 },
    { category: '设备材料', amount: 1800, budget: 2000 },
    { category: '差旅费用', amount: 450, budget: 500 },
    { category: '其他支出', amount: 280, budget: 400 },
];

const TEAM_MEMBERS = [
    { name: '张三', role: '项目经理', tasks: 12, completed: 10, efficiency: 92 },
    { name: '李四', role: '架构师', tasks: 8, completed: 8, efficiency: 100 },
    { name: '王五', role: '前端开发', tasks: 24, completed: 20, efficiency: 88 },
    { name: '赵六', role: '后端开发', tasks: 28, completed: 22, efficiency: 82 },
    { name: '钱七', role: '测试工程师', tasks: 35, completed: 30, efficiency: 90 },
    { name: '孙八', role: '实施工程师', tasks: 18, completed: 16, efficiency: 94 },
];

const ISSUES = [
    { id: 'TECH-001', type: '技术难点', desc: '大数据量查询性能优化', priority: 'high', priorityLabel: '高', solution: '已匹配历史优化方案，采用分页+缓存策略' },
    { id: 'COORD-001', type: '协调事项', desc: '需甲方提供生产数据库访问权限', priority: 'high', priorityLabel: '高', solution: '已推送至甲方待审批' },
    { id: 'TECH-002', type: '技术难点', desc: '第三方API接口文档不全', priority: 'medium', priorityLabel: '中', solution: '联系厂商补充技术文档中' },
    { id: 'SITE-001', type: '现场问题', desc: '机房温控系统异常影响设备调试', priority: 'medium', priorityLabel: '中', solution: '协调甲方物业部门处理' },
];

/* ============================================================
   工具函数
   ============================================================ */

// 实时时钟
function updateClock() {
    const now = new Date();
    const pad = n => String(n).padStart(2, '0');
    document.getElementById('updateTime').textContent =
        `${now.getFullYear()}-${pad(now.getMonth() + 1)}-${pad(now.getDate())} ${pad(now.getHours())}:${pad(now.getMinutes())}:${pad(now.getSeconds())}`;
}
updateClock();
setInterval(updateClock, 1000);

// 所有 ECharts 实例
const charts = [];

function createChart(domId) {
    const dom = document.getElementById(domId);
    if (!dom) return null;
    const chart = echarts.init(dom, null, { renderer: 'canvas' });
    charts.push(chart);
    return chart;
}

window.addEventListener('resize', () => charts.forEach(c => c.resize()));
```

- [ ] **Step 2: 验证数据结构和工具函数**

检查：
- DOM 元素 `updateTime` 存在
- `createChart()` 函数在所有图表初始化前已定义
- `charts[]` 数组管理所有实例

---

### Task 4: JS - ECharts 图表渲染

**Files:**
- Modify: `project-dashboard/chengjianfang.html` (在 Task 3 JS 之后追加)

- [ ] **Step 1: 写入周度任务柱状图和成本横向柱状图**

```javascript
/* ============================================================
   1. 周度任务 - 分组柱状图
   ============================================================ */
(function() {
    const chart = createChart('chartSchedule');
    if (!chart) return;

    chart.setOption({
        tooltip: {
            trigger: 'axis',
            backgroundColor: 'rgba(10,15,26,0.9)',
            borderColor: 'rgba(0,212,255,0.3)',
            textStyle: { color: '#fff', fontSize: 12 }
        },
        legend: {
            top: 0, right: 0,
            textStyle: { color: 'rgba(255,255,255,0.7)', fontSize: 10 },
            data: ['计划任务', '完成任务']
        },
        grid: { top: 35, right: 10, bottom: 25, left: 40 },
        xAxis: {
            type: 'category', data: WEEKLY_TASKS.map(d => d.week),
            axisLine: { lineStyle: { color: 'rgba(255,255,255,0.15)' } },
            axisLabel: { color: 'rgba(255,255,255,0.6)', fontSize: 10 }
        },
        yAxis: {
            type: 'value',
            axisLine: { show: false },
            splitLine: { lineStyle: { color: 'rgba(255,255,255,0.06)' } },
            axisLabel: { color: 'rgba(255,255,255,0.5)', fontSize: 10 }
        },
        series: [
            {
                name: '计划任务', type: 'bar', barWidth: 12,
                data: WEEKLY_TASKS.map(d => d.planned),
                itemStyle: {
                    color: new echarts.graphic.LinearGradient(0, 1, 0, 0, [
                        { offset: 0, color: 'rgba(139,92,246,0.3)' },
                        { offset: 1, color: '#8b5cf6' }
                    ]),
                    borderRadius: [3, 3, 0, 0]
                }
            },
            {
                name: '完成任务', type: 'bar', barWidth: 12,
                data: WEEKLY_TASKS.map(d => d.completed),
                itemStyle: {
                    color: new echarts.graphic.LinearGradient(0, 1, 0, 0, [
                        { offset: 0, color: 'rgba(34,197,94,0.3)' },
                        { offset: 1, color: '#22c55e' }
                    ]),
                    borderRadius: [3, 3, 0, 0]
                }
            }
        ]
    });
})();

/* ============================================================
   2. 成本管控 - 横向柱状图
   ============================================================ */
(function() {
    const chart = createChart('chartCost');
    if (!chart) return;

    chart.setOption({
        tooltip: {
            trigger: 'axis', axisPointer: { type: 'shadow' },
            backgroundColor: 'rgba(10,15,26,0.9)',
            borderColor: 'rgba(0,212,255,0.3)',
            textStyle: { color: '#fff', fontSize: 12 }
        },
        legend: {
            top: 0, right: 0,
            textStyle: { color: 'rgba(255,255,255,0.7)', fontSize: 10 },
            data: ['实际成本(万)', '预算(万)']
        },
        grid: { top: 35, right: 50, bottom: 10, left: 80 },
        xAxis: {
            type: 'value',
            axisLine: { show: false },
            splitLine: { lineStyle: { color: 'rgba(255,255,255,0.06)' } },
            axisLabel: { color: 'rgba(255,255,255,0.5)', fontSize: 10 }
        },
        yAxis: {
            type: 'category', data: COST_DATA.map(d => d.category),
            axisLine: { lineStyle: { color: 'rgba(255,255,255,0.15)' } },
            axisLabel: { color: 'rgba(255,255,255,0.7)', fontSize: 11 }
        },
        series: [
            {
                name: '实际成本(万)', type: 'bar', barWidth: 10,
                data: COST_DATA.map(d => ({
                    value: d.amount,
                    itemStyle: {
                        color: new echarts.graphic.LinearGradient(0, 0, 1, 0, [
                            { offset: 0, color: 'rgba(34,197,94,0.3)' },
                            { offset: 1, color: '#22c55e' }
                        ]),
                        borderRadius: [0, 3, 3, 0]
                    }
                }))
            },
            {
                name: '预算(万)', type: 'bar', barWidth: 10,
                data: COST_DATA.map(d => ({
                    value: d.budget,
                    itemStyle: {
                        color: new echarts.graphic.LinearGradient(0, 0, 1, 0, [
                            { offset: 0, color: 'rgba(59,130,246,0.3)' },
                            { offset: 1, color: '#3b82f6' }
                        ]),
                        borderRadius: [0, 3, 3, 0]
                    }
                }))
            }
        ]
    });
})();

/* ============================================================
   3. 集成进度 - 雷达图
   ============================================================ */
(function() {
    const chart = createChart('chartRadar');
    if (!chart) return;

    chart.setOption({
        tooltip: {
            backgroundColor: 'rgba(10,15,26,0.9)',
            borderColor: 'rgba(0,212,255,0.3)',
            textStyle: { color: '#fff' }
        },
        radar: {
            indicator: RADAR_DATA.map(d => ({ name: d.subject, max: 100 })),
            shape: 'polygon',
            splitNumber: 4,
            axisName: { color: 'rgba(255,255,255,0.7)', fontSize: 11 },
            splitLine: { lineStyle: { color: 'rgba(255,255,255,0.1)' } },
            splitArea: { areaStyle: { color: ['rgba(139,92,246,0.02)', 'rgba(139,92,246,0.04)'] } },
            axisLine: { lineStyle: { color: 'rgba(255,255,255,0.15)' } }
        },
        series: [{
            type: 'radar',
            data: [{
                value: RADAR_DATA.map(d => d.value),
                name: '集成进度',
                lineStyle: { color: '#8b5cf6', width: 2 },
                areaStyle: { color: 'rgba(139,92,246,0.2)' },
                itemStyle: { color: '#8b5cf6' }
            }]
        }]
    });
})();
```

- [ ] **Step 2: 验证图表渲染**

确认：
- `chartSchedule` 柱状图按周展示计划 vs 完成任务
- `chartCost` 横向柱状图展示成本 vs 预算
- `chartRadar` 雷达图展示5维集成进度
- 所有图表使用深色主题、渐变配色、圆角柱条

---

### Task 5: JS - 动态渲染 + Tab 切换

**Files:**
- Modify: `project-dashboard/chengjianfang.html` (在 Task 4 JS 之后追加)

- [ ] **Step 1: 写入子系统进度渲染**

```javascript
/* ============================================================
   4. 渲染子系统进度
   ============================================================ */
(function() {
    const container = document.getElementById('subsystemList');
    if (!container) return;

    container.innerHTML = SYSTEM_PROGRESS.map(d => {
        const barColor = d.progress >= 80 ? 'cyan' : d.progress >= 60 ? 'cyan' : 'orange';
        return `
            <div class="subsystem-item">
                <div class="subsystem-header">
                    <span class="subsystem-name">${d.system}</span>
                    <span class="subsystem-progress-text">${d.progress}%</span>
                </div>
                <div class="progress-bar"><div class="progress-fill ${barColor}" style="width:${d.progress}%"></div></div>
                ${d.progress < 60 ? '<div class="subsystem-warning">⚠️ 进度滞后，需加快</div>' : ''}
            </div>
        `;
    }).join('');
})();

/* ============================================================
   5. 渲染软件研发模块进度
   ============================================================ */
(function() {
    const container = document.getElementById('softwareModules');
    if (!container) return;

    container.innerHTML = SOFTWARE_MODULES.map(d => {
        const barColor = d.progress === 100 ? 'cyan' : d.progress >= 60 ? 'cyan' : 'orange';
        const badgeClass = d.statusType === 'success' ? 'badge badge-success' :
                          d.statusType === 'default' ? 'badge badge-default' : 'badge badge-secondary';
        return `
            <div class="progress-item">
                <div class="progress-item-header">
                    <span class="progress-item-name">${d.name}</span>
                    <span class="${badgeClass}">${d.status}</span>
                </div>
                <div class="progress-bar"><div class="progress-fill ${barColor}" style="width:${d.progress}%"></div></div>
            </div>
        `;
    }).join('');
})();

/* ============================================================
   6. 渲染硬件实施表格
   ============================================================ */
(function() {
    const tbody = document.getElementById('hardwareTableBody');
    if (!tbody) return;

    tbody.innerHTML = HARDWARE_DATA.map(d => {
        const rateBadge = d.rate >= 90 ? 'badge badge-success' :
                         d.rate >= 80 ? 'badge badge-default' : 'badge badge-destructive';
        const statusBadge = d.rate === 100
            ? '<span class="badge badge-success badge-icon">✓ 已完成</span>'
            : '<span class="badge badge-default badge-icon">⟳ 进行中</span>';
        return `
            <tr>
                <td style="font-weight:500;">${d.item}</td>
                <td style="color:var(--text-muted)">${d.planned}</td>
                <td style="color:var(--text-muted)">${d.completed}</td>
                <td><span class="${rateBadge}">${d.rate}%</span></td>
                <td>${statusBadge}</td>
            </tr>
        `;
    }).join('');
})();

/* ============================================================
   7. 渲染第三方对接列表
   ============================================================ */
(function() {
    const container = document.getElementById('thirdPartyList');
    if (!container) return;

    container.innerHTML = THIRD_PARTY.map(d => {
        const barColor = d.progress === 100 ? 'cyan' :
                         d.progress >= 50 ? 'cyan' : '';
        const badgeClass = d.statusType === 'success' ? 'badge badge-success' :
                          d.statusType === 'default' ? 'badge badge-default' : 'badge badge-secondary';
        return `
            <div class="progress-item">
                <div class="progress-item-header">
                    <span class="progress-item-name">${d.system}</span>
                    <span class="${badgeClass}">${d.status}</span>
                </div>
                ${d.progress > 0 ? `<div class="progress-bar"><div class="progress-fill ${barColor}" style="width:${d.progress}%"></div></div>` : ''}
            </div>
        `;
    }).join('');
})();

/* ============================================================
   8. 渲染人员表格
   ============================================================ */
(function() {
    const tbody = document.getElementById('teamTableBody');
    if (!tbody) return;

    tbody.innerHTML = TEAM_MEMBERS.map(d => {
        const badgeClass = d.efficiency >= 90 ? 'badge badge-success' : 'badge badge-secondary';
        return `
            <tr>
                <td style="font-weight:500;">${d.name}</td>
                <td style="color:var(--text-muted);font-size:12px;">${d.role}</td>
                <td style="color:var(--text-muted);text-align:center;">${d.tasks}</td>
                <td style="color:var(--text-muted);text-align:center;">${d.completed}</td>
                <td style="text-align:center;"><span class="${badgeClass}">${d.efficiency}%</span></td>
            </tr>
        `;
    }).join('');
})();

/* ============================================================
   9. 渲染问题表格
   ============================================================ */
(function() {
    const tbody = document.getElementById('issuesTableBody');
    if (!tbody) return;

    tbody.innerHTML = ISSUES.map(d => {
        const typeBadge = d.type === '技术难点' ? 'badge badge-default' :
                         d.type === '协调事项' ? 'badge badge-warning' : 'badge badge-secondary';
        const priorityBadge = d.priority === 'high' ? 'badge badge-destructive' : 'badge badge-warning';
        return `
            <tr>
                <td style="font-family:monospace;color:var(--primary-color);font-weight:600;">${d.id}</td>
                <td><span class="${typeBadge}">${d.type}</span></td>
                <td style="font-size:12px;">${d.desc}</td>
                <td><span class="${priorityBadge}">${d.priorityLabel}</span></td>
                <td style="font-size:12px;max-width:260px;">${d.solution}</td>
            </tr>
        `;
    }).join('');
})();

/* ============================================================
   10. Tab 切换交互
   ============================================================ */
(function() {
    const tabs = document.querySelectorAll('.tab-btn');
    tabs.forEach(btn => {
        btn.addEventListener('click', function() {
            // 切换按钮状态
            tabs.forEach(b => b.classList.remove('active'));
            this.classList.add('active');
            // 切换内容
            const tabId = this.dataset.tab;
            const contents = {
                'software': 'tabSoftware',
                'hardware': 'tabHardware',
                'integration': 'tabIntegration'
            };
            document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
            document.getElementById(contents[tabId]).classList.add('active');
        });
    });
})();
```

- [ ] **Step 2: 验证所有动态渲染**

检查：
- 浏览器打开页面，所有数据正确渲染
- Tab 切换正常：点击"硬件实施"显示表格，点击"系统集成"显示雷达图+列表
- 时钟每秒更新
- 页面 resize 时图表自适应
- 无 JS 报错

---

### Task 6: 最终验证

- [ ] **Step 1: LSP 诊断检查**

```bash
# 检查文件是否有基础语法问题
lsp_diagnostics filePath="project-dashboard/chengjianfang.html"
```

- [ ] **Step 2: 浏览器打开验证**

```bash
# 在浏览器中打开验证
open project-dashboard/chengjianfang.html
```

手动验证清单：
- [ ] 页面加载无白屏
- [ ] Header 显示正确：标题、甲方单位、实施阶段、考核分、倒计时、时钟
- [ ] AI排程区域：3个指标卡显示 68.5%/87%/3项
- [ ] AI优化建议 Alert 显示
- [ ] 周度任务柱状图正常渲染
- [ ] 子系统进度 5 条进度条，报表系统显示⚠️滞后
- [ ] 分项Tab：软件研发 Tab 显示模块进度 + Bug统计
- [ ] 硬件实施 Tab 显示5行表格
- [ ] 系统集成 Tab 显示雷达图 + 第三方对接列表
- [ ] 成本管控：横向柱状图 + 汇总卡片
- [ ] 人员看板：6名成员表格
- [ ] 问题表格：4条数据 + AI提示
- [ ] 响应式：缩窄浏览器窗口布局适配
- [ ] 页面淡入动画流畅
