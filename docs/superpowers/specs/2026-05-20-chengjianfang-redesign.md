# 承建方执行视角大屏 - 完全替换设计规格

## 1. 概述

将 `project-dashboard/chengjianfang.html` 完全替换为基于 Figma 原型 `ContractorDashboard.tsx` 的新版承建方执行视角大屏。保留现有 `common.css` 设计系统，将 React + shadcn/ui + Recharts 组件转换为纯 HTML + CSS + ECharts。

## 2. 布局结构

```
┌─────────────────────────────────────────────────────────┐
│  Header: 项目信息 + 倒计时 + 考核分                      │
├────────────────────────┬────────────────────────────────┤
│  AI智能排程与进度调度    │  各子系统建设进度               │
│  3指标卡 + AI建议 + 图表 │  5条进度条 + 滞后标识           │
├────────────────────────┴────────────────────────────────┤
│  分项业务实施管控 (Tab: 软件研发 | 硬件实施 | 系统集成)    │
│  软件: 模块进度条 + Bug统计                              │
│  硬件: 实施点位表格                                      │
│  集成: 雷达图 + 第三方对接列表                            │
├─────────────────────────┬──────────────────────────────┤
│  内部成本管控             │  人员班组任务看板              │
│  横向柱状图 + 汇总卡片    │  成员任务表格 + 效率标注       │
├─────────────────────────┴──────────────────────────────┤
│  现场问题&风险化解专区 (问题表格 + AI智能提示)             │
└─────────────────────────────────────────────────────────┘
```

## 3. 模块分解

### 3.1 顶部指挥栏 (Header)
- **项目标题**: "AI项目落地执行作战大屏"
- **甲方单位**: 某省政务信息中心
- **实施阶段**: 硬件部署 + 软件研发并行
- **内部考核**: 85分
- **倒计时**: 距离竣工 133天（橙色框）
- **更新时间**: 实时时钟

### 3.2 AI智能排程与进度调度中心
- **3个指标卡**:
  - 整体完成率: 68.5%（带进度条，青色）
  - 本周任务达成: 87%（带进度条，绿色）
  - 滞后工序: 3项（红色警示）
- **AI优化建议 Alert**: 系统集成环节滞后，建议并行开发
- **周度任务执行情况柱状图** (ECharts): 分组柱状图，展示W18-W21的计划任务与完成任务

### 3.3 各子系统建设进度
- **5条进度条** (HTML/CSS):
  - 用户管理 85%（绿）
  - 权限控制 90%（绿）
  - 数据分析 60%（蓝）
  - 报表系统 45%（黄，⚠️滞后）
  - 接口集成 70%（蓝）
- 进度 < 60% 显示滞后警示

### 3.4 分项业务实施管控
- **Tab 切换**: 软件研发 / 硬件实施 / 系统集成
- **软件研发 Tab**:
  - 模块开发进度列表（5模块，带Badge状态）
  - Bug修复统计（待修复12/本周修复28/累计修复156）
  - 效率提示 Alert
- **硬件实施 Tab**:
  - 实施点位表格: 项目/计划/已完成/完成率/状态
  - 5行数据: 服务器部署/网络设备/终端设备/安全设备/存储设备
- **系统集成 Tab**:
  - 雷达图 (ECharts): 前后端联调/第三方接口/数据库/认证授权/消息队列
  - 第三方对接列表（4系统，带进度条）

### 3.5 内部成本管控
- **横向柱状图** (ECharts): 人力成本/设备材料/差旅费用/其他支出 vs 预算
- **汇总卡片**: 总投入成本 5730万 / 成本使用率 88.5%
- **AI分析 Alert**: 成本控制良好

### 3.6 人员班组任务看板
- **表格**: 姓名/岗位/任务数/完成数/效率
- 6名成员: 项目经理/架构师/前端/后端/测试/实施
- 效率 >= 90% 绿色Badge，其余灰色Badge

### 3.7 现场问题&风险化解专区
- **问题表格**: 编号/类型/描述/优先级/AI方案
- 4条问题数据（技术难点/协调事项/现场问题）
- 高优先级红色Badge，中优先级黄色Badge
- **AI智能提示 Alert**: 从历史项目库匹配到3个相似方案

## 4. 图表转换映射 (Recharts → ECharts)

| Figma原型 (Recharts) | 本项目 (ECharts) | 容器ID |
|---|---|---|
| BarChart 周度任务 | 分组柱状图 | chartSchedule |
| BarChart 成本 (layout=vertical) | 横向柱状图 | chartCost |
| RadarChart 集成进度 | 雷达图 | chartRadar |
| Progress 进度条 (多次) | HTML/CSS 进度条 | 内联 |

## 5. 数据模型

所有数据使用 Mock 静态数据，与 Figma 原型一致：

```javascript
// 周度任务
weeklyTasks = [
  { week: "W18", planned: 45, completed: 42 },
  { week: "W19", planned: 50, completed: 48 },
  { week: "W20", planned: 48, completed: 35 },
  { week: "W21", planned: 52, completed: 0 },
]

// 子系统进度
systemProgress = [
  { system: "用户管理", progress: 85 },
  { system: "权限控制", progress: 90 },
  { system: "数据分析", progress: 60 },
  { system: "报表系统", progress: 45 },
  { system: "接口集成", progress: 70 },
]

// 成本数据
costBreakdown = [
  { category: "人力成本", amount: 3200, budget: 3500 },
  { category: "设备材料", amount: 1800, budget: 2000 },
  { category: "差旅费用", amount: 450, budget: 500 },
  { category: "其他支出", amount: 280, budget: 400 },
]

// 团队成员
teamMembers = [
  { name: "张三", role: "项目经理", tasks: 12, completed: 10, efficiency: 92 },
  { name: "李四", role: "架构师", tasks: 8, completed: 8, efficiency: 100 },
  { name: "王五", role: "前端开发", tasks: 24, completed: 20, efficiency: 88 },
  { name: "赵六", role: "后端开发", tasks: 28, completed: 22, efficiency: 82 },
  { name: "钱七", role: "测试工程师", tasks: 35, completed: 30, efficiency: 90 },
  { name: "孙八", role: "实施工程师", tasks: 18, completed: 16, efficiency: 94 },
]

// 问题数据
issues = [
  { id: "TECH-001", type: "技术难点", desc: "大数据量查询性能优化", priority: "high", solution: "已匹配历史优化方案，采用分页+缓存策略" },
  { id: "COORD-001", type: "协调事项", desc: "需甲方提供生产数据库访问权限", priority: "high", solution: "已推送至甲方待审批" },
  { id: "TECH-002", type: "技术难点", desc: "第三方API接口文档不全", priority: "medium", solution: "联系厂商补充技术文档中" },
  { id: "SITE-001", type: "现场问题", desc: "机房温控系统异常影响设备调试", priority: "medium", solution: "协调甲方物业部门处理" },
]
```

## 6. 交互设计

- **实时时钟**: 每秒更新，显示当前时间
- **卡片淡入动画**: 按顺序延迟（0.1s - 0.35s）淡入
- **Tab切换**: 点击切换软件研发/硬件实施/系统集成，内容平滑切换
- **图表自适应**: 窗口resize时ECharts自动调整大小
- **进度条动画**: hover时放大效果

## 7. 样式规范

- **基础**: 复用 `common.css` 的所有设计变量
- **新增样式块**: 内联在 `chengjianfang.html` 的 `<style>` 中
- **卡片**: 使用 `.card` + `.card-title` 基础类
- **进度条**: 使用 `.progress-bar` + `.progress-fill` 基础类
- **标签**: 使用 `.tag-primary/tag-success/tag-warning/tag-danger`
- **表格**: 参照 `jiafang.html` 的表格样式（`.issue-table` 体系）
- **Badge**: 独立实现圆角标签样式
- **色系**: 青色(#00d4ff)主色调 + 绿色完成 + 橙色预警 + 红色高优

## 8. 响应式断点

- **>1400px**: 完整三列/两列布局
- **1024-1400px**: 部分列合并
- **<1024px**: 单列堆叠
- Tab内容在小屏自适应

## 9. 技术决策

| 项目 | 选择 | 原因 |
|---|---|---|
| 图表库 | ECharts 5 (CDN) | 项目已有，功能完备 |
| 样式 | common.css + 内联page CSS | 复用现有设计系统 |
| 交互 | 原生 JS | 无框架依赖 |
| 图标 | Unicode/HTML实体 | 无额外依赖 |
| 字体 | 系统字体栈 (common.css) | 无需额外加载 |

## 10. 涉及文件

- **修改**: `project-dashboard/chengjianfang.html` — 完全替换内容
- **无修改**: `project-dashboard/css/common.css` — 复用
- **无修改**: 其他文件不受影响
