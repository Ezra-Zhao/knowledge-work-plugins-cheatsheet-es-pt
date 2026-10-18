# knowledge-work-plugins 速查表 | Cheatsheet | Guía rápida

> 一页看懂 Anthropic 官方的 knowledge-work-plugins：把 Claude 变成某个岗位专家的插件集，为 Claude Cowork 而建、也兼容 Claude Code。每个插件打包了一个岗位的技能（skills）、连接器（connectors）、斜杠命令（slash commands）和子代理，开箱是一套通用起点，真正的价值在于按你公司的工具、术语、流程改造成自己的版本。全部是 Markdown 和 JSON 文件，没有代码、没有构建步骤。

- 上游项目：[anthropics/knowledge-work-plugins](https://github.com/anthropics/knowledge-work-plugins)（2026-10-18 实测 30,112 ★）
- 运行环境：Claude Cowork（主）、Claude Code（兼容）
- 本页结构：中文主文 → English → Español → Português

---

## 核心概念

| 部件 | 一句话说明 |
|---|---|
| Skill（技能） | 岗位知识、最佳实践、分步工作流，Claude 在相关场景自动调用，你不用显式触发 |
| Command（斜杠命令） | 你显式触发的动作，如 `/sales:call-prep`、`/finance:reconciliation` |
| Connector（连接器） | 通过 MCP 服务器把 Claude 接到你岗位用的外部工具：CRM、项目管理、数据仓库、设计工具等 |
| plugin.json | 插件清单文件，放在 `.claude-plugin/` 目录里 |
| .mcp.json | 该插件的工具连接配置，换成自己公司的工具栈就改这个文件 |
| 定制 | 把公司术语、组织结构、流程写进 skill 文件，Claude 就按你公司的方式干活 |

插件目录结构（每个插件都长这样）：

```
plugin-name/
├── .claude-plugin/plugin.json   # 清单
├── .mcp.json                    # 工具连接
├── commands/                    # 你显式触发的斜杠命令
└── skills/                      # Claude 自动调用的岗位知识
```

## 11 个官方插件一览

| 插件 | 干什么 | 典型连接器 |
|---|---|---|
| productivity（效率） | 管任务、日历、日常流程和个人上下文，少重复交代 | Slack、Notion、Asana、Linear、Jira、Monday、ClickUp、Microsoft 365 |
| sales（销售） | 查潜客、备通话、看管道、写外联、做竞品对战卡 | Slack、HubSpot、Close、Clay、ZoomInfo、Fireflies |
| customer-support（客服） | 工单分诊、起草回复、打包升级、查客户背景、把已解决工单沉淀成知识库 | Slack、Intercom、HubSpot、Guru、Jira、Notion |
| product-management（产品） | 写规格、排路线图、综合用户研究、同步干系人、盯竞品 | Linear、Asana、Jira、Figma、Amplitude、Pendo、Intercom |
| marketing（市场） | 起草内容、策划活动、守品牌口径、竞品简报、跨渠道复盘 | Canva、Figma、HubSpot、Amplitude、Ahrefs、Klaviyo |
| legal（法务） | 审合同、分诊 NDA、合规导航、风险评估、会议准备、模板回复 | Box、Egnyte、Jira、Microsoft 365 |
| finance（财务） | 做分录、对账、出财务报表、差异分析、管月结、支持审计 | Snowflake、Databricks、BigQuery、Slack、Microsoft 365 |
| data（数据） | 查数、画图、解读数据集：写 SQL、统计分析、建仪表盘、分享前自校验 | Snowflake、Databricks、BigQuery、Hex、Amplitude |
| enterprise-search（企业搜索） | 一个查询横跨邮件、聊天、文档、Wiki 找任何东西 | Slack、Notion、Guru、Jira、Asana、Microsoft 365 |
| bio-research（生物研究） | 接临床前研究工具与数据库：文献检索、基因组分析、靶点排序 | PubMed、bioRxiv、ClinicalTrials.gov、ChEMBL、Open Targets、Benchling |
| cowork-plugin-management（插件管理） | 用来新建插件或给组织定制现有插件的元插件 | — |

仓库里另有 design、engineering、human-resources、operations、small-business 等目录在持续补充，以 GitHub 仓库实际目录为准。

## 安装

```bash
# Cowork：到 claude.com/plugins 直接安装

# Claude Code：先加市场，再装具体插件
claude plugin marketplace add anthropics/knowledge-work-plugins
claude plugin install sales@knowledge-work-plugins
```

装好即自动生效：skills 在相关场景自动触发，斜杠命令在会话里直接可用。

## 常用斜杠命令示例

| 命令 | 用途 |
|---|---|
| `/sales:call-prep` | 通话前准备：潜客背景、谈话要点 |
| `/data:write-query` | 写 SQL 查询 |
| `/finance:reconciliation` | 账目核对 |
| `/product-management:write-spec` | 写产品规格文档 |

命令全集以各插件目录下 `commands/` 的实际文件为准。

## 典型工作流

1. **销售备会**：装 sales 插件，连上 HubSpot 和日历，开会前跑 `/sales:call-prep`，再让它起草跟进邮件和竞品对战卡。
2. **财务月结**：装 finance 插件，连数据仓库，跑对账命令，生成报表草稿和差异分析，人工复核后再入账。
3. **全公司找资料**：装 enterprise-search，一个问题横跨 Slack、Notion、邮件和 Wiki 一次找全，不用逐个系统翻。

## 把它改成你公司的版本（官方推荐路径）

1. **换连接器**：改 `.mcp.json`，指向你实际用的工具栈。
2. **加公司上下文**：把术语、组织结构、流程写进 skill 文件。
3. **调工作流**：改 skill 里的指令，让它按你团队真实做法走，而不是教科书做法。
4. **建新插件**：用 cowork-plugin-management 插件，或照上面的目录结构，为没覆盖的岗位自建。

## 常见坑

- **开箱只是起点**：通用版谁都能用，不结合你公司的术语和流程，产出就是平均水平；官方明确说真正的威力来自定制。
- **连接器要各自授权**：Slack、HubSpot 这些外部工具需要你自己的账号和权限，插件不会凭空拿到数据。
- **别和 Claude Code 的其他插件市场搞混**：安装命令里市场名是 `knowledge-work-plugins`，插件名格式是 `插件名@knowledge-work-plugins`。
- **全是文件、没有代码**：出问题先查 plugin.json 和 .mcp.json 的写法，不用找构建日志。

---

## English (summary)

knowledge-work-plugins (by Anthropic, ~30.1k stars as of 2026-10-18) turns Claude into a role specialist. Built for Claude Cowork and compatible with Claude Code, each plugin bundles the skills, connectors, slash commands and sub-agents for one job function. Everything is file-based — markdown and JSON, no code, no build step.

The 11 official plugins: productivity, sales, customer-support, product-management, marketing, legal, finance, data, enterprise-search, bio-research, and cowork-plugin-management (for building your own). Every plugin has the same anatomy: `.claude-plugin/plugin.json` (manifest), `.mcp.json` (tool connections), `commands/` (slash commands you invoke, e.g. `/sales:call-prep`, `/data:write-query`, `/finance:reconciliation`, `/product-management:write-spec`), and `skills/` (domain knowledge Claude draws on automatically).

Install from claude.com/plugins in Cowork, or in Claude Code: `claude plugin marketplace add anthropics/knowledge-work-plugins` then `claude plugin install sales@knowledge-work-plugins`. Plugins activate automatically once installed.

The generic plugins are only a starting point — the real leverage is customization: swap connectors in `.mcp.json` to your own tool stack, drop your terminology and processes into the skill files, adjust workflows to how your team actually works, and build new plugins for uncovered roles. Watch out: connectors still need your own accounts and permissions for tools like Slack and HubSpot; don't confuse this marketplace name with others when typing install commands; if something misbehaves, check the JSON files first — there is no build to debug.

---

## Español (guía rápida — traducción de estudio, pendiente de revisión por un hablante nativo)

knowledge-work-plugins (de Anthropic, ~30,1 mil estrellas en octubre de 2026) convierte a Claude en un especialista de tu puesto de trabajo. Está pensado para Claude Cowork y también es compatible con Claude Code. Cada plugin reúne las habilidades (skills), conectores, comandos de barra (slash commands) y subagentes de una función laboral. Todo son archivos markdown y JSON: sin código, sin infraestructura, sin pasos de compilación.

Los 11 plugins oficiales: productivity (productividad), sales (ventas), customer-support (atención al cliente), product-management (producto), marketing, legal (jurídico), finance (finanzas), data (datos), enterprise-search (búsqueda en toda la empresa), bio-research (investigación biomédica) y cowork-plugin-management (para crear plugins nuevos).

Estructura de cada plugin: `.claude-plugin/plugin.json` (manifiesto), `.mcp.json` (conexiones a herramientas), `commands/` (comandos que invocas tú, como `/sales:call-prep`, `/data:write-query`, `/finance:reconciliation`, `/product-management:write-spec`) y `skills/` (conocimiento que Claude aplica automáticamente cuando es relevante).

Instalación: en Cowork, desde claude.com/plugins; en Claude Code, primero `claude plugin marketplace add anthropics/knowledge-work-plugins` y después `claude plugin install sales@knowledge-work-plugins`. Una vez instalados, los plugins se activan solos.

La clave está en personalizarlos: cambia los conectores en `.mcp.json` por las herramientas de tu empresa, añade tu terminología y tus procesos a los archivos de skills, ajusta los flujos a cómo trabaja tu equipo de verdad y crea plugins nuevos para los puestos no cubiertos. Errores frecuentes: esperar resultados excelentes sin personalizar nada (la versión genérica es solo el punto de partida); olvidar que cada conector necesita tu propia cuenta y permisos en Slack, HubSpot, etc.; confundir el nombre del marketplace al escribir el comando de instalación.

---

## Português (guia rápido — tradução de estudo, aguardando revisão de um falante nativo)

O knowledge-work-plugins (da Anthropic, ~30,1 mil estrelas em outubro de 2026) transforma o Claude num especialista da sua função. Foi feito para o Claude Cowork e também é compatível com o Claude Code. Cada plugin reúne as habilidades (skills), conectores, comandos de barra (slash commands) e subagentes de uma função de trabalho. Tudo é arquivo markdown e JSON: sem código, sem infraestrutura, sem etapa de build.

Os 11 plugins oficiais: productivity (produtividade), sales (vendas), customer-support (suporte ao cliente), product-management (produto), marketing, legal (jurídico), finance (finanças), data (dados), enterprise-search (busca em toda a empresa), bio-research (pesquisa biomédica) e cowork-plugin-management (para criar plugins novos).

Estrutura de cada plugin: `.claude-plugin/plugin.json` (manifesto), `.mcp.json` (conexões com ferramentas), `commands/` (comandos que você invoca, como `/sales:call-prep`, `/data:write-query`, `/finance:reconciliation`, `/product-management:write-spec`) e `skills/` (conhecimento que o Claude aplica automaticamente quando é relevante).

Instalação: no Cowork, por claude.com/plugins; no Claude Code, primeiro `claude plugin marketplace add anthropics/knowledge-work-plugins` e depois `claude plugin install sales@knowledge-work-plugins`. Depois de instalados, os plugins se ativam sozinhos.

O segredo está na personalização: troque os conectores no `.mcp.json` pelas ferramentas da sua empresa, adicione a sua terminologia e os seus processos aos arquivos de skills, ajuste os fluxos ao jeito real da sua equipe e crie plugins novos para as funções não cobertas. Armadilhas comuns: esperar resultados excelentes sem personalizar nada (a versão genérica é só o ponto de partida); esquecer que cada conector precisa da sua própria conta e permissões no Slack, HubSpot etc.; confundir o nome do marketplace ao digitar o comando de instalação.

---

## 来源与声明

- 灵感来源 / Source / Fuente / Fonte: [anthropics/knowledge-work-plugins](https://github.com/anthropics/knowledge-work-plugins)
- 本仓是个人学习整理：内容用我自己的话总结自上游公开文档，不搬运原文；插件清单与命令以 2026-10-18 的上游 README 与仓库目录为准。
- 西语、葡语部分为学习译稿，待母语校对。Las secciones en español y portugués son traducciones de estudio pendientes de revisión nativa.
- License: MIT © 2026 Guangyi Zhao
