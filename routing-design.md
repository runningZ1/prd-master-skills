# PRD Master 智能路由设计方案

## 核心设计原则

### 1. 智能模式检测
根据用户输入自动判断所需模式，避免用户选择困难。

### 2. 渐进式暴露
根据用户需求逐步暴露功能，不一次性展示所有选项。

### 3. 向后兼容
保留原有 prd-generator 和 prd-taskmaster 的调用方式。

## 模式检测逻辑

### 检测关键词和短语

#### 🎓 学习模式 (Learning Mode)
**触发条件：用户询问知识性问题**
```
关键词：
- "什么是PRD"、"PRD是什么"
- "如何写"、"怎么写"
- "最佳实践"、"方法论"
- "指南"、"规范"
- "学习"、"了解"、"入门"
- "协作"、"流程"
- "文档质量"
- "技术文档"

示例：
- "我想学习如何写PRD"
- "什么是PRD的核心结构？"
- "给我介绍文档协作的最佳实践"
```

#### 🔧 通用生成模式 (General Mode)
**触发条件：用户明确要求生成PRD，但不涉及TaskMaster**
```
关键词：
- "创建PRD"、"生成PRD"
- "写产品需求"
- "document a feature"
- "产品规范"
- "需求文档"

排除条件：
- 不包含 taskmaster 相关词
- 不包含执行相关词（"execute", "run", "autonomous"）

示例：
- "为用户认证功能创建PRD"
- "生成一个移动应用暗色模式的产品需求文档"
- "我需要一个新产品的PRD模板"
```

#### 🚀 TaskMaster模式 (TaskMaster Mode)
**触发条件：用户明确提及TaskMaster或要求执行**
```
关键词：
- "taskmaster"、"task-master"
- "任务管理"、"执行"
- "autonomous"、"自动执行"
- "run tasks"、"execute tasks"
- "工作流"、"workflow"
- "generate and execute"

示例：
- "为这个功能生成PRD并用TaskMaster执行"
- "使用taskmaster创建任务列表"
- "我想自动执行PRD中的任务"
```

#### 🔀 模糊模式 (Ambiguous)
**触发条件：无法明确判断用户意图**
```
处理策略：
- 显示智能引导界面
- 询问用户具体需求
- 提供三种模式的简介和选择
```

## 智能路由流程图

```
用户输入
    ↓
模式检测器分析
    ↓
┌─────────────┬─────────────┬─────────────┐
│   🎓 学习   │   🔧 通用   │   🚀 TM模式 │
│   模式      │   模式      │             │
└─────────────┴─────────────┴─────────────┘
    ↓           ↓           ↓
显示对应模块   5步生成流程   12步工作流
    ↓           ↓           ↓
退出/返回      任务完成      可选执行
```

## 三层使用路径

### 路径1：纯学习路径
```
用户："我想学习如何写PRD"
    ↓
检测：学习模式
    ↓
展示：
- PRD核心思路与方法论
- 整体设计技巧
- 文档协作指南
- 技术写作规范
    ↓
退出
```

### 路径2：通用PRD生成路径
```
用户："为暗色模式创建PRD"
    ↓
检测：通用模式
    ↓
展示：
- 5步生成工作流
- 13部分标准模板
- 用户故事最佳实践
- 成功指标框架
    ↓
输出：完整PRD文档
```

### 路径3：TaskMaster完整路径
```
用户："生成PRD并用TaskMaster执行"
    ↓
检测：TaskMaster模式
    ↓
展示：
- 12步完整工作流
- TaskMaster检测与初始化
- PRD生成（11部分）
- 13项自动化验证
- 任务分解与展开
- 用户测试检查点
- 执行模式选择
    ↓
输出：PRD + 任务列表 + 可选自动执行
```

## 模式检测实现

### 检测函数伪代码

```python
def detect_user_mode(user_input):
    """
    分析用户输入，返回对应的模式
    返回值: 'learning', 'general', 'taskmaster', 'ambiguous'
    """

    input_lower = user_input.lower()

    # 学习模式关键词
    learning_keywords = [
        '什么是', '如何写', '怎么写', '最佳实践', '方法论',
        '指南', '规范', '学习', '了解', '入门', '协作',
        '流程', '文档质量', '技术文档', 'prd是什么',
        'why', 'how to', 'best practice', 'guide', 'learn'
    ]

    # TaskMaster模式关键词
    taskmaster_keywords = [
        'taskmaster', 'task-master', '任务管理', '执行',
        'autonomous', '自动执行', 'run tasks', 'execute',
        '工作流', 'workflow', 'generate and execute'
    ]

    # 通用生成关键词
    general_keywords = [
        '创建prd', '生成prd', '写产品需求', 'document a feature',
        '产品规范', '需求文档', 'create prd', 'generate prd',
        'write requirements'
    ]

    # 检查 TaskMaster 模式（优先级最高）
    if any(keyword in input_lower for keyword in taskmaster_keywords):
        return 'taskmaster'

    # 检查学习模式
    learning_score = sum(1 for keyword in learning_keywords if keyword in input_lower)

    # 检查通用模式
    general_score = sum(1 for keyword in general_keywords if keyword in input_lower)

    # 判断逻辑
    if learning_score >= 2 and learning_score > general_score:
        return 'learning'
    elif general_score >= 1:
        return 'general'
    elif learning_score >= 1:
        return 'learning'
    else:
        return 'ambiguous'
```

## 智能引导界面

### 模糊模式处理

当检测结果为 `ambiguous` 时，显示引导界面：

```
🤔 我注意到您的需求可能属于以下几种情况：

请告诉我您更希望：

1️⃣  **学习PRD方法论**
   我想了解如何写PRD、学习最佳实践、掌握文档协作技巧
   → 适合：新手、想要提升写作能力

2️⃣  **生成通用PRD**
   为某个功能/产品创建标准的产品需求文档
   → 适合：产品经理、需要快速生成PRD

3️⃣  **TaskMaster完整工作流**
   生成PRD并自动分解为任务，支持自动执行
   → 适合：工程师、需要端到端解决方案

请输入 1、2 或 3，或者更详细地描述您的需求。
```

## 向后兼容

### 保留原有调用方式

```markdown
### 兼容 prd-generator 调用
用户输入："generate a PRD for..."
→ 自动路由到通用模式

### 兼容 prd-taskmaster 调用
用户输入："taskmaster generate..."
→ 自动路由到TaskMaster模式
```

## 性能优化

### 1. 快速匹配
- 使用简单的关键词匹配而非复杂NLP
- 缓存检测结果（同一会话中）

### 2. 渐进式加载
- 只加载用户所需的模块
- 延迟加载其他模块

### 3. 智能预加载
- 预测用户可能的下一步操作
- 预加载相关资源

## 错误处理

### 检测失败
- 回退到模糊模式
- 提供引导界面
- 允许用户手动选择

### 模式切换
- 用户可以随时切换模式
- 保留已生成的内容
- 平滑过渡

## 测试策略

### 测试用例
1. 学习模式：各种知识性问题
2. 通用模式：标准PRD生成请求
3. TaskMaster模式：包含执行需求的请求
4. 模糊模式：含糊不清的请求

### 准确性指标
- 目标：90%+ 的准确率
- 持续优化检测逻辑
- 收集用户反馈
