---
name: "prd-master"
description: "将模糊想法转换为生产级PRD的智能工具。支持快速PRD生成和TaskMaster完整工作流。"
allowed-tools: [Read, Write, Edit, Grep, Glob, Bash, AskUserQuestion, TodoWrite]
---

# PRD Master - 生产级PRD生成器 v4.0

## 🎯 智能模式检测

当您使用此 Skill 时，我会**自动检测**您的需求：

### 🔍 检测逻辑

```python
def detect_user_intent(input):
    input_lower = input.lower()

    # TaskMaster模式（明确提及）
    if any(kw in input_lower for kw in ['taskmaster', '执行', '自动执行', '任务管理']):
        return 'taskmaster'

    # 学习模式（方法论问题）
    if any(kw in input_lower for kw in ['如何写', '怎么写', '方法论', '最佳实践', '什么是prd']):
        return 'learning'

    # 默认：通用生成模式
    return 'general'
```

### 💡 使用示例

| 您的输入 | 我理解您需要 | 自动进入 |
|----------|-------------|----------|
| "我想学习PRD" | 方法论指导 | 🎓 学习模式 |
| "为认证功能创建PRD" | 快速生成PRD | 🔧 通用生成模式 |
| "用TaskMaster执行" | 生成+任务执行 | 🚀 TaskMaster模式 |

## 🎓 学习模式

当检测到您需要方法论指导时，我会提供：
- PRD核心思路（背景→问题→价值→目的）
- "为什么要做" vs "要做什么" 结构
- "条件-动作-预期"三段式表达
- 完整的用户注册功能案例

## 🔧 通用PRD生成模式（5步工作流）

**适合**：快速将想法转换为标准PRD

1. **收集上下文**（提问）：问题、用户、目标、指标、约束
2. **生成PRD结构**：13个标准部分的精简版本
3. **创建用户故事**：As a... I want... So that... 格式
4. **定义成功指标**：AARRR/HEART/北极星指标框架
5. **生成最终文档**：Markdown格式，可直接使用

**输出位置**：项目根目录的 `PRD.md` 或 `.taskmaster/docs/prd.md`

## 🚀 TaskMaster完整工作流（6步简化）

**适合**：生成PRD并自动分解为执行任务

1. **检测现有PRD**：检查是否已有PRD文件
2. **检测TaskMaster**：MCP > CLI > 缺失提示
3. **收集需求**（8个核心问题，非13个）
4. **生成PRD**（11部分，面向工程师）
5. **解析并展开任务**：自动生成分层任务列表
6. **选择执行模式**：移交控制或自主执行

**核心优势**：
- 任务自动分解（无需手动拆解）
- 依赖自动识别
- 用户测试检查点自动插入

---

## 📚 相关文档

- [通用生成模式详解](SKILL-general.md) - 5步工作流完整说明
- [TaskMaster模式详解](SKILL-taskmaster.md) - 6步工作流和自动化执行
- [参考文档](references/) - 方法论、案例、示例
- [模板库](templates/) - 可复用的PRD模板

---

**版本**: v4.0.0
**最后更新**: 2025-12-07
**核心改进**: 删除冗余内容，回归"模糊想法→生产级PRD"的核心定位
