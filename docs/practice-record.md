# 实践记录

> 每一步的原始输出保存在 `../evidence/` 目录，这里记录步骤和结论。
> 运行环境：Claude Code 智能体（Linux x86_64，Python 3.13），官方技能取自 https://gitcode.com/huaweicloud/huaweicloud-skills 。

| 步骤 | 输入（提示词） | 调用的技能 | 结果 | 证据文件 |
| --- | --- | --- | --- | --- |
| 1 找技能 | 帮我找找华为云有没有管理 ECS 的技能？ | huawei-cloud-find-skills（`search-skills.py -k "ECS"`，自动扩展为“ECS、弹性云服务器”） | 命中 **32 个**技能，按相关度排序：前三为 `huawei-cloud-ecs-manage`（31 分）、`huawei-cloud-computing-query`（29 分）、`huawei-cloud-ces-ecs-monitoring`（26 分）；其余包括 ecs-lifecycle、ecs-query、ecs-alert、主机迁移等 | `evidence/01-search-ecs.txt` |
| 2 看详情并安装 | 请介绍技能huawei-cloud-computing-query的功能点，并完成安装 | huawei-cloud-find-skills（`npx skills add …#master --skill huawei-cloud-computing-query -y`） | 只读查询技能，能力按目录划分：ECS（实例、规格、密钥对、配额、云服务器组、磁盘、网卡、VNC）、BMS（裸金属服务器、规格、配额）、IMS（镜像、OS 版本、成员、配额）、AS（伸缩组、配置、策略、活动日志、生命周期挂钩、预热池、配额）。**安装成功**（退出码 0） | `evidence/02-install-computing-query.txt` |
| 3 提问题单 | huawei-cloud-computing-query覆盖ECS/BMS/IMS/AS，但缺少CCE云容器引擎查询能力，提交VOD反馈 | huawei-cloud-vod-collector（`md_io.py write-feedback`） | 生成结构化问题单 **VOD-20261106-0001**（类型 suggestion，产品 huawei-cloud-computing-query）。未同步为 GitCode Issue，原因见下文 | `evidence/03-vod/VOD-20261106-0001.md` |

## 实践中的发现：问题单要基于核实后的事实

手册示例说 computing-query“缺少 CCE 查询能力”。但第 1 步的检索结果里，find-skills 索引给这个技能的描述是 **“Queries Huawei Cloud computing resources (ECS/BMS/IMS/AS/CCE)”**，看上去已经支持 CCE。

于是在第 2 步安装后核对了技能本体：
- `SKILL.md` 的 frontmatter 写的是 **ECS/BMS/IMS/AS**，没有 CCE；
- `scripts/` 下只有 `as/ bms/ ecs/ ims/ iam/` 五个目录，**没有任何 CCE 脚本或指引**。

结论：CCE 查询能力**确实缺失**，而且 **find-skills 索引的描述与技能本体不一致**，用户会按索引误以为能查 CCE。问题单把这两点都写了进去，并给出改进建议：补 CCE 只读查询，或在说明中写明不支持并推荐对应的 CCE 技能；同时修正索引描述。

这也印证了方法论第三节的评估清单：**以技能本体为准**，不能只看检索结果的摘要。

## 运行中遇到的环境限制（如实记录）

| 环节 | 情况 | 处理 |
| --- | --- | --- |
| find-skills Step 0.1 安装 `skill-quality-cli` | 官方发布通道只提供 linux-arm64 构建，x86_64 无法自动安装 | SKILL.md 规定“安装失败不阻塞流程”，直接运行检索脚本；匿名质量上报因此未执行 |
| vod-collector 同步 Issue（AtomGit-GO 登录） | 官方安装脚本发现 x86_64 安装包缺少 `.sha256` 校验文件，**拒绝安装未校验的二进制** | 不绕过安全校验；手册说明同步 Issue 为“按需”，本次保留本地结构化问题单。在华为云 AI 开发者空间的 AI Shell 中可以完成扫码登录并一键同步 |
