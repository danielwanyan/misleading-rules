# Misleading Rules 规则仓库

用于存储和管理 TikTok Shop Misleading Functionality & Effect (MFE) 审核规则。

## 目录结构

```
misleading-rules/
├── rules/
│   └── all.json          # 主规则文件（Aicolate 直接拉取此文件）
├── exceptions/
│   └── examples.yaml     # 例外案例库（积累历史误判案例）
└── README.md             # 本文档
```

## 如何更新规则

### 方式一：直接编辑 JSON 文件（技术同学）

1. 编辑 `rules/all.json`
2. 更新 `version` 字段（例如：`2025-05-18-v1` → `2025-05-18-v2`）
3. 更新 `last_updated` 字段
4. Commit & Push

### 方式二：添加例外案例（业务同学）

1. 编辑 `exceptions/examples.yaml`
2. 按照现有格式添加新案例
3. Commit & Push

## 规则版本说明

版本号格式：`YYYY-MM-DD-vN`

- `YYYY-MM-DD`: 更新日期
- `vN`: 当天的第 N 次更新

示例：
- `2025-05-18-v1` - 2025年5月18日第一次更新
- `2025-05-18-v2` - 2025年5月18日第二次更新

## Aicolate 接入方式

在 Aicolate Workflow 中添加 HTTP Request 节点：

- **URL**: `https://raw.githubusercontent.com/你的用户名/misleading-rules/main/rules/all.json`
- **方法**: GET
- **输出变量**: `RULES_JSON`

## 规则文件结构说明

### `rules/all.json`

```json
{
  "version": "规则版本号",
  "last_updated": "最后更新日期",
  "global": { ... },           // 全局规则（兜底豁免等）
  "step1": { ... },            // Step 1 规则（前后对比、数字特效）
  "step2": { ... },            // Step 2 规则（夸大承诺、健康宣称、违背科学）
  "step3": { ... },            // Step 3 规则（Listing核验、包装核验）
  "conflict_rules": [ ... ]    // 冲突处理规则
}
```

### `exceptions/examples.yaml`

```yaml
- case_id: EX001
  scenario: 违规场景类型
  title: 案例简短描述
  content: 案例详细内容
  expected: 有误导 / 无误导
  reason: 为什么这样判断
  added_date: 添加日期
```

## 注意事项

1. **不要修改固定的执行逻辑** - 规则数据只包含"判断什么"，不包含"如何判断"
2. **每次更新都要更新版本号** - 便于追踪和排查问题
3. **添加案例要详细** - 包含足够的上下文信息，便于模型理解
