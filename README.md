# Safeheron CSM Knowledge

一个面向 **Safeheron CSM / 客服支持 / 集成支持** 团队的内部知识库 skill。装上之后，CSM 同事可以直接用自然语言向 Claude 提问，由 Claude 基于本知识库给出标准答复，并标注出处方便回溯。

## 覆盖内容

知识库整理自 Safeheron 真实工单沉淀，共 **100+ 条 Q&A + 5 套邮件模版**，按 8 大板块组织：

| 板块 | 条数 | 主要话题 |
|---|---|---|
| API Co-Signer | 20 | 部署、callback URL、RSA 密钥、KMS、CLI 命令、Illegal IP、enable-mysql |
| API | 30 | 添加币种、交易方向、错误码 9028、MPC Sign、AML、UTXO 归集、余额快照 |
| Web Console | 9 | 登录、GA、二次验证、首页报错 |
| APP | 17 | 扫码、激活、私钥碎片、生物识别、推送 |
| 交易 | 20 | 状态机、确认数、回滚、Auto Sweep、Gas Service、链上 diff |
| 权限 | 21 | 角色、白名单、API Key、操作日志、IP 白名单 |
| 策略 | 7 | MPC Sign / Web3 / 普通策略的申请流程、累计额度 |
| 模版 | 5 | 更改决策模式、申请 MPC Sign 策略、申请 Web3 策略、GA 重置、更换创建人邮箱 |

> 当前版本：**v1.0 / 2025-04-28**。后续 Safeheron 产品更新（如 Cosigner 新版本、新链、新 KMS 厂商）可能滞后于 KB，使用时若发现内容过时请提 PR 更新。

## 安装方式

### Cowork / Claude Desktop（最推荐，CSM 同事一键搞定）

1. 进入本仓库 [Releases](../../releases/latest) 页面。
2. 下载最新的 `safeheron-csm-knowledge.skill` 文件。
3. 打开 Cowork，把 `.skill` 文件拖进对话窗口，点 **Save skill** 即可。

### Claude Code（开发者）

把整个仓库 clone 到 Claude Code 的 skills 目录：

```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/<your-username>/safeheron-csm-knowledge.git
```

之后启动 Claude Code 就能在 `<available_skills>` 列表里看到 `safeheron-csm-knowledge`。

### 自己重新打包 .skill

如果你修改了 `references/csm_faq.md` 想生成新的 .skill：

```bash
# 在装了 skill-creator 的环境里
python -m scripts.package_skill /path/to/safeheron-csm-knowledge ./dist
# → 会在 ./dist/ 下生成 safeheron-csm-knowledge.skill
```

## 怎么用

装上之后，CSM 同事正常向 Claude 提问，例如：

- 「客户启动 Cosigner 报 Illegal IP，怎么处理？」
- 「9028 是啥错误？」
- 「客户要换创建人邮箱，给我一份邮件模版」
- 「申请 MPC Sign 策略需要发什么内容给 support？」
- 「自定义确认数会让交易更快完成吗？」

Claude 会自动加载知识库 → 定位章节 → 给出标准答复，并在结尾标注 `（参考：API Co-Signer 第 15 条）` 之类的来源。KB 里没有的问题会明确告知，并指引到 docs.safeheron.com / support.safeheron.com / Support 团队，不会瞎编。

## 仓库结构

```
safeheron-csm-knowledge/
├── README.md                    # 本说明
├── SKILL.md                     # skill 定义（frontmatter + 给 Claude 的工作指引）
└── references/
    └── csm_faq.md               # 完整知识库（按 8 大板块组织）
```

## 维护与贡献

知识库源数据全部在 `references/csm_faq.md` 这一个 markdown 文件里。新增、修改、勘误：

1. Fork 本仓库或新建分支。
2. 在 `references/csm_faq.md` 里直接改 markdown。
3. 提交 PR，标题写明改动了哪个板块的第几条。
4. 合并后由维护者发新 release（`v1.1`、`v1.2`……），CSM 同事重新下载 `.skill` 即可。

如果改动比较大（新增板块、调整结构），请同步更新：
- `SKILL.md` 里的板块清单
- 本 `README.md` 里的覆盖内容表格

## 边界与免责

- 本 KB 是工单沉淀，**不是官方文档**。最终对客户回复仍以 [Safeheron 官方文档](https://docs.safeheron.com) 和 [Help Center](https://support.safeheron.com) 为准。
- 涉及资金、合规判断（AML 黑名单处理、地址替换、跨链恢复）时，请以 Safeheron Support 团队的最终判断为准。
- 帮客户起草回信时，建议发送前由 Support 复核。

## License

内部使用。请勿外传。
