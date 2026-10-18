# 把官方插件改成自己公司的版本 | Make It Yours

个人学习整理，配合主 README 使用。以下为方法示意，文件写法以官方仓库为准。

## 1. 换连接器

打开插件根目录的 `.mcp.json`，把示例里的工具换成你公司实际用的栈。例如销售插件默认列了多个 CRM，只保留你用的那一个，并按该 MCP 服务器的要求填好配置。

English: Edit `.mcp.json` to point at the tools your company actually uses; remove the rest.

## 2. 给 skill 加公司上下文

在插件的 `skills/` 里找到对应工作流文件，补上三类信息：

- 公司术语：产品名、客户分层叫法、内部缩写
- 组织结构：谁负责什么、审批找谁
- 流程口径：报价怎么审、工单几级升级、月结哪天截止

English: Add terminology, org structure, and process rules to the skill files so Claude works the way your team works.

## 3. 自建一个没覆盖的岗位插件

```bash
# 目录骨架（与官方插件同构）
my-plugin/
├── .claude-plugin/plugin.json
├── .mcp.json
├── commands/
│   └── weekly-report.md
└── skills/
    └── domain-basics.md
```

装插件管理插件 `cowork-plugin-management@knowledge-work-plugins` 后，可以让 Claude 引导你走完新建流程。

English: Scaffold the same four-part structure, or install the cowork-plugin-management plugin and let Claude walk you through creating one.
