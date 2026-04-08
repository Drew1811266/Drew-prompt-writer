# Tasks

## 多轮对话编辑功能

- [x] Task 1: 在 SKILL.md 中添加整改暗号识别逻辑
  - [x] SubTask 1.1: 在暗号激活码部分添加 `c` 暗号说明
  - [x] SubTask 1.2: 定义用户整改需求的常见表达方式（重新设计、改为、换成等）
  - [x] SubTask 1.3: 实现整改需求的解析与提示词修改逻辑

- [x] Task 2: 在 SKILL.md 中添加结束暗号识别逻辑
  - [x] SubTask 2.1: 添加 `end` 和 `结束` 暗号说明
  - [x] SubTask 2.2: 实现上下文清空机制

- [x] Task 3: 在 SKILL.md 中添加多轮对话上下文管理
  - [x] SubTask 3.1: 定义上下文记忆的数据结构（存储原始需求、当前提示词版本）
  - [x] SubTask 3.2: 实现整改模式下的累积修改逻辑
  - [x] SubTask 3.3: 确保新需求输入时清空旧上下文

- [x] Task 4: 更新输出格式说明
  - [x] SubTask 4.1: 整改模式下输出完整提示词（非差异）
  - [x] SubTask 4.2: 确保整改后输出格式与首次生成一致

- [x] Task 5: 添加整改模式示例
  - [x] SubTask 5.1: 添加"重新设计三个镜头"示例
  - [x] SubTask 5.2: 添加"景别改为中景"示例
  - [x] SubTask 5.3: 添加"角色的裤子换成长裤"示例
  - [x] SubTask 5.4: 添加"场景改为校园"示例
  - [x] SubTask 5.5: 添加"角色的行为改为滑滑板"示例

## Task Dependencies
- Task 2 可以在 Task 1 之后独立进行
- Task 3 依赖 Task 1 和 Task 2 的完成
- Task 4 依赖 Task 3 的完成
- Task 5 可以在 Task 1-4 完成后进行

## Completed Implementation
所有任务已完成，SKILL.md 已更新：
- 暗号触发规则：新增 `c`（整改编辑）和 `end/结束`（清空上下文）
- 多轮对话编辑功能章节：包含上下文记忆机制、结束暗号、整改暗号识别、整改处理流程、整改输出格式、6个整改示例、多轮对话管理原则
