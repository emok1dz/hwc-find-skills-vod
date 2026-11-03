# 华为云技能查找与VOD反馈方法论

ICT 大赛实践赛题 **1.9「Skill-让AI 帮你找到所需的华为云技能」** 作品。

本作品整理了“**先找技能 → 安装使用 → 不满足就提 VOD 问题单**”的完整流程，并把它提炼成一套可复用的标准化方法论。实践中用到两个华为云官方技能：

| 技能 | 作用 | 是否产生费用 |
| --- | --- | --- |
| [`huawei-cloud-find-skills`](https://skills.huaweicloud.com/detail/huawei-cloud-find-skills) | 在数百个华为云技能中，按意图（关键词 / 类目）检索、查看详情、一键安装 | 否（只读查询 GitCode 技能索引） |
| [`huawei-cloud-vod-collector`](https://skills.huaweicloud.com/detail/huawei-cloud-vod-collector) | 把体验问题、需求缺口沉淀为结构化问题单（VOD-YYYYMMDD-NNNN），并同步为 GitCode Issue | 否（本地生成文件，授权后提交 Issue） |

## 目录

| 路径 | 内容 |
| --- | --- |
| [`docs/methodology.md`](docs/methodology.md) | **标准化方法论**：找技能 → 评估 → 安装 → 使用 → 缺口反馈的 5 步流程、检查清单和提示词模板 |
| [`docs/practice-record.md`](docs/practice-record.md) | 实践记录：每一步的输入、技能调用和结果 |
| [`evidence/`](evidence/) | 实践证据：搜索结果、技能安装结果、VOD 问题单原文、Issue 链接 |

## 实践流程概览

```
用户意图 ──► find-skills 检索（关键词/类目，评分排序）
              │
              ├─ 命中 ──► 查看详情 ──► 安装（npx skills add）──► 按技能说明执行任务
              │                                                   │
              │                                                   └─ 使用中遇到问题/缺口 ─┐
              └─ 未命中（需求缺口）───────────────────────────────────────────────────┤
                                                                                       ▼
                                          vod-collector：采集 → 去重 → 提炼 → 生成 VOD 问题单
                                                                                       │
                                                                     GitCode 授权 ──► 同步为 Issue（可追踪）
```

## 本次实践内容

1. **找技能**：提问“帮我找找华为云有没有管理 ECS 的技能？”，由 find-skills 检索出 ECS 相关技能并按相关度排序。
2. **安装使用**：查看 `huawei-cloud-computing-query` 的功能点并完成安装。
3. **提问题单**：发现 `huawei-cloud-computing-query` 覆盖 ECS/BMS/IMS/AS，但缺少 CCE 云容器引擎的查询能力，于是用 vod-collector 生成 VOD 问题单，并在 GitCode 授权后同步为 Issue。

各步骤的原始输出见 [`evidence/`](evidence/)，详细记录见 [`docs/practice-record.md`](docs/practice-record.md)。
