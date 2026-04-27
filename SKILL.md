---
name: safeheron-csm-knowledge
description: Safeheron CSM（客户成功 / 客服支持）团队的内部知识库，包含 124 条来自真实工单的 Q&A，覆盖 API Co-Signer、API、Web Console、APP、交易、权限、策略、邮件/工单模版 8 大板块。**只要用户的问题涉及 Safeheron 平台 —— 包括 Cosigner 部署、Callback Service、API 报错、币种添加、权限/角色、策略变更、Web/APP 登录、GA 重置、决策模式调整、申请 MPC Sign / Web3 策略、更换创建人邮箱、安装/激活 APP 出现的奇怪报错等等 —— 都立即调用本 skill**。即使用户只是描述一个具体现象（"启动 Cosigner 报 Illegal IP"、"手机扫码后转圈"、"客户问能不能加 ALIYUN KMS"），只要明显是 CSM 工单场景，就走这个 skill；不要凭印象作答，先查知识库再回答。
---

# Safeheron CSM 知识库

## 这个 skill 给谁用

Safeheron 客户成功（CSM）/ 客服支持 / 集成支持同事。CSM 同事每天会被客户问一堆问题，Safeheron 平台的细节非常多（部署、密钥、回调、策略、KMS、决策模式、邮件模版……），凭记忆答容易出错，本 skill 把过往工单沉淀的标准答复做成可检索的内部资料。

## Claude 应该怎么使用

请按下面的工作流程回答 CSM 同事的问题：

1. **先读知识库**：用 Read 工具加载 `references/csm_faq.md`。文件不大（约 17 KB / 100+ 条 Q&A），可一次性读完。

2. **定位章节**：根据用户问题里的关键词判断属于哪个板块，再到对应章节里检索：
   - **API Co-Signer**：部署、callback URL、RSA 密钥、KMS、CLI 命令、登录失败、Illegal IP、enable-mysql、cosigner export-public-key
   - **API**：添加币种、交易方向、9028、MPC Sign、accountKey/coinKey、签名、AML、自定义确认数、UTXO 归集、余额快照、地址替换
   - **Web Console**：登录、GA、二次验证、首页报错、币种页加载
   - **APP**：扫码、激活、私钥碎片、绑定新设备、生物识别、推送
   - **交易**：状态机、确认数、回滚、取消、Auto Sweep、Gas Service、链上 diff、入账
   - **权限**：角色、白名单、API Key、操作日志、新增管理员、IP 白名单
   - **策略**：MPC Sign / Web3 / 普通策略的申请流程、累计额度、模板
   - **模版**：邮件 / 工单标准模版（更改决策模式、申请 MPC Sign 策略、申请 Web3 策略、GA 忘记重置、更换创建人邮箱）

3. **基于证据作答**：直接用知识库里的标准答复回答，把里面给的官方文档链接（docs.safeheron.com、support.safeheron.com、Google Sheets 链接等）一并附给用户，方便他们转给客户做权威佐证。

4. **标注来源**：每条结论后用 `（参考：XX 板块 第 N 条）` 的形式注明出处，方便 CSM 同事回查原文。

5. **没命中就老实说**：如果 KB 里没有现成答案，**不要凭印象编造**。直接告诉 CSM：「这个问题在 CSM 知识库里没有现成答复，建议：(1) 查阅 https://docs.safeheron.com 官方 API 文档；(2) 查阅 https://support.safeheron.com 帮助中心；(3) 升级到 Safeheron Support 团队。」如果你能基于通用知识给一些排查方向，可以同步给出，但要明确标注是「推测，需 Support 确认」。

## 触发场景示例

下面这些问题都应该立刻命中本 skill：

- 「客户启动 Cosigner 失败，日志里有 Illegal IP，怎么办？」
- 「Cosigner CLI setup 报 Failed to log in to the Docker image repository」
- 「Approval Callback Service 怎么导出公钥？」
- 「为什么客户取消了交易 Cosigner 还在回调？」
- 「客户 Cosigner 的 KMS 想用 ALIYUN，行不行？」
- 「新建钱包后没加币，怎么批量添加 coinKey？」
- 「9028 是什么错误？」
- 「客户 GA 忘了想重置，标准流程是什么？」
- 「客户要换创建人邮箱，要发什么邮件？」
- 「申请 MPC Sign 策略邮件模版有吗？」
- 「自定义确认数会让交易更快完成吗？」
- 「APP 扫码激活一直卡住怎么办？」（如果 KB 里没有，参考第 5 步走兜底流程）
- 「客户问累计额度策略改了之后历史值会保留吗？」

## 回答风格

- **先结论后解释**：CSM 同事时间宝贵，第一句给答案，再展开操作步骤或原理。
- **保留专业术语**：accountKey、coinKey、Co-Signer、Callback Service、Approval Callback、Webhook、KYT、Auto Sweep、Gas Service、决策模式（Decision Mode）、MPC Sign 等是 Safeheron 平台固定术语，不要意译。
- **保留命令/SQL/链接原样**：知识库里出现的 `sudo ./cosigner export-public-key`、`UPDATE mpc_client_trans_audit_record SET audit_status = 2 WHERE trade_no = '${tx_key}'`、文档链接等要原样转出。
- **匹配用户语言**：用户用中文问就用中文答，用英文问就用英文答。知识库本身是中文的，回答英文问题时把要点翻译过去即可。
- **对面是 CSM 不是终端客户**：可以用内部口吻，不必每条都加客套话；但如果 CSM 显然是要把答案直接转给客户，就用客户视角的口吻（"您好，..."）。问不清楚时反问一句"这个回答是给客户还是给您内部参考"，再据此调整措辞。

## 边界与免责

- 本 KB 是 2025-04-28 V1.0 版本的工单沉淀，**对产品的最新版本（如 Cosigner 2.x 之后的特性、新加的链、新增的 KMS 厂商）可能滞后**。当用户描述的现象与 KB 内容明显矛盾时，提醒他可能是 KB 已过期，建议联系 Support 确认。
- 涉及资金、合规判断（如 AML 黑名单处理、特定地址替换、跨链恢复）时，KB 给的是参考路径，**最终决策权在客户和 Safeheron 官方支持**，不要替客户拍板。
- 如果 CSM 让你"帮客户写一封回信"，可以基于 KB 内容起草，但要在末尾提醒 CSM "请发送前由 Support 复核"。

## 文件清单

- `SKILL.md`：本说明
- `references/csm_faq.md`：完整知识库，按 8 大板块组织，含官方文档链接和邮件模版
