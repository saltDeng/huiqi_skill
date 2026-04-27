# Safeheron CSM 知识库

> 本知识库为 Safeheron 客户成功 / 客服支持团队（CSM）整理，覆盖 API Co-Signer、API、Web Console、APP、交易、权限、策略、邮件/工单模版等 8 大板块的真实工单沉淀。Claude 在回答 CSM 同事的问题时，请严格依据本文档作答，并在结论后注明来源章节。

版本修订记录

| 版本 | 日期 | 更改条款内容 | 编辑者 | 审核者 |
| :---- | :---- | :---- | :---- | :---- |
| V1.0 | 2025.04.28 | 初稿 | Deng Huiqi | Huang Bo |

## 

##  **API Co-Signer**

1. 部署新版本 Cosigner Web Console callback URL 指的是，与老版本区别  
答：新版本部署 Coisgner 流程比之前更加简化了，您这边部署 API Co-Signer 时只需要生成一对 RSA 公私钥即可，之前是需要您生成两对 RSA 公私钥对。  
这里的 callback URL 和公钥就是部署老版本 API Co-Signer 时在配置文件需配置对应的 biz_callback 和 biz_pubkey，公钥也是您自己生成，可以参考 API 文档：[https://docs.safeheron.com/api/zh.html#API%20%E9%89%B4%E6%9D%83](https://docs.safeheron.com/api/zh.html#API%20%E9%89%B4%E6%9D%83)  
      openssl rsa -in api_private.pem -out api_public.pem -pubout

2. 配置 Approval Callback Service 需要用到哪些公私钥，如何导出 Co-Signer 公钥  
答：您在配置 Callback Service 的时候需要使用从 API Co-Signer 导出的身份认证公钥，以及您在申请部署 API Co-Signer 的创建 API Key 时 callback 公钥对应的私钥。  
      sudo ./cosigner export-public-key

3. API Co-Signer 部署文档都是用 Secrets Manager 方式来管理配置，但是文档为啥还是需要创建 .env 配置文件  
答：您好，首先我们推荐使用 Secrets Manager 管理配置，其次 Secrets Manager 主要用于管理 API Co-Signer 相关的敏感数据，但是还是有一些数据是无法使用 Secrets Manager 管理的，所以也会需要用到 .env 文件。

4. 修改 Co-Signer 配置文件 .env ,如何保证修改保存后配置可生效  
答：您可以参考 CLI 命令手册，需要先 stop，再使用 start 命令启动   
   sudo ./cssigner stop  
   sudo ./cosigner start

5. 部署 API Co-Signer callback URL 上传格式要求  
	答：您好，上传 callback URL 目前得使用 pem 格式即通过命令生成源文件，以----------BEGIN PUBLIC KEY—-------- 开头的，若是 “访问的 API ” 则需要将公钥换成一行，Co-Signer 身份认证公钥默认导出一行

6.   callback appoval 服务和 Co-Signer 放在一个内网中，calllback URL 填内网 IP 是否可以  
 	答：确保部署 callback 服务那台机器能通过 IP 正常访问就行。  
7. 部署 API Co-Signer 、Webhook 时不知如何配对 RSA 公私钥对  
答：您好，可以参考以下信息：  
[客户cosigner部署列表](https://docs.google.com/spreadsheets/d/1MtgzdLKzd0zWxLO8xC9OhAmwDX3t_rr-LtsV51iWv0A/edit?pli=1&gid=1133706475#gid=1133706475)  
_[图片说明：详见原文档配图]_

8. 部署 API Co-Signer 机器内存、CPU 、数据库等配置推荐  
答：您好，可以参考以下配置：  
[客户cosigner部署列表](https://docs.google.com/spreadsheets/d/1MtgzdLKzd0zWxLO8xC9OhAmwDX3t_rr-LtsV51iWv0A/edit?pli=1&gid=1729275737#gid=1729275737)  
_[图片说明：详见原文档配图]_

9. 成功部署 Approval Callback Service 后，何时才会有 Callback 调用  
	答：Callback 调用是发生在有交易需审批签名时候，其它时候不会调用。

10. 配置 Callback URL 什么时候是必填的，什么时候是非必填的  
	答：您好，测试环境团队 Callback URL 是选填的，正式团队 Callback URL 是必须要填的。

11. API Co-Signer启动失败了/审核任务长时间处于等待API Co-Signer中，如何定位问题？  
	答：您可以使用 ./cosigner logs -f 命令查看API Co-Signer的日志，日志中会有较为明确的错误信息，或者使用 ./cosigner logs -s 命令将日志导出到本地，然后发送给safeheron的Support寻求帮助。

12. API Co-Signer的日志中出现 'The final private key fragment is not exists, please activate API Co-Signer first.' 这种日志需要如何处理？  
	答：这个是正常的日志，代表API Co-Signer已经正常启动，需要您继续进行激活API Co-Signer的流程了。

13. API Co-Signer审批任务的时间间隔是多少？  
	答：2.0 以下的老版本是 1s，2.0 以上的新版本是 5s。  
14. 部署 API Co-Signer 使用的 KMS 支持哪些云厂商  
	答：我们支持 AWS 或 GCP 的 KMS，不支持使用 ALIYUN 的 KMS（原因：阿里云 的 KMS 需要和服务器使用同一个 VPC，故不支持）

15. API Co-Signer 启动后日志出现 “ Illegal IP ” 是什么原因  
	答：没有将 API Co-Signer 宿主机 IP 加到对应的 API Key 白名单里，加入之后审核，重新启动 API Co-Signer 即可。

16. AWS 上配置了 Mysql ，启动 Co-Signer 用了 enable-mysql 参数  
	答：这个时候启动 Cosigner 会优先使用 secret 上的 mysql 配置

17.执行CLI的setup、start命令是出现报错：Failed to log in to the Docker image repository. The token may have expired. Please download the new CLI from safeheron web console.  
	答：目前已知的可能情况有已下几个：  
1）CLI内置的token过期，可能性较低  
2）未执行setup命令就先执行了start命令，可以让库户执行\`docker images\`命令确认docker是否已经启动  
3）设置了网络防火墙，屏蔽了 registry.gitlab.com  
4）Linux系统镜像的问题，目前已知在 \[Debian 4.19.316\]上 会出现，执行登录时具体报错：Error saving credentials: error storing credentials - err: exit status 1, out:\`Cannot autolaunch D-Bus without X11 SDISPLAY\`，此时可以推荐客户将系统镜像换成Ubuntu 24.04

18. Web 端更新了 Callback URL ，需不需重新启动 Cosigner  
	答：无需重新启动 Cosigner ，等待 5min 即可。

19. Callback 流程多久时间超时？  
错误日志模板：Failed to execute Approval Callback, error: Failed to read Approval Callback response data. Cause by: Timestamp out of range. timestamp: 1756105122892, now: 1756105140981  
	答：要求响应的时间戳与当前API Co-Signer服务器的时间差在5s内。

20. 在 Web/API 取消来交易之后，为什么 Co-Signer 还在回调 Callback Service ?  
	答：在2.x版本之前，还需要在API Co-Signer的数据库执行以下SQL才能取消API Co-Signer已经入库的审核任务: UPDATE \`mpc_client_trans_audit_record\` SET audit_status \= 2 WHERE trade_no \= '${tx_key}';

## **API**

1. 新创建钱包， accountKey 未添加任何币种该如何添加币及注意事项  
	答：可通过添加币种 V2/V1 接口添加，我们推荐使用 V2 接口 ,可用 coinKeyList 数组支持最大一次添加 20 个 coinKey。coinKey 若为 token ，添加该 token 币种会自动添加对应的主网币种，coinKey 若为主网币种，添加该币种就只会添加该币种	  
⚠️注：  
1）成功添加过 coinKey ，再次添加接口响应数据与上次一致；  
2）成功添加过 coinKey , 手动关闭成功添加过 coinKey ，不会再打开币种。

2. 如何通过平台/目标类型来区分是充值、还是提币，以及交易方向判断  
答：您好，交易按目标类型传入什么类型来判断，若目标类型传入VAULT_ACCOUNT 则为内部转账；若目标类型传入 ONE_TIME_ADDRESS 则为流出；若源账户类型为 UNKNOWN 则为流入（ Web Console /APP 也有方向的标记）

3. 客户首次使用 MPC Sign 接口签名报错 9028  
	答：您好，MPC Sign 是底层等签名方法，对安全性要求很高，需要申请以邮件形式联系 Support 增加 MPC Sign 策略。  
	MPC Sign 邮件模版连接：[Help Center](https://support.safeheron.com/help-center/jian-ti-zhong-wen/chan-pin/shen-ru-safeheron/mpc-sign-ce-le)

4. Safeheron 出口的 IP，客户配置白名单  
答：ip列表：  
18.162.105.64  
18.167.22.59  
18.167.21.182

5. 配置某些 Webhook URL 时，出现无法配置的情况  
答：检查下该 Webhook URL 是否在公网透出，如 ngrok 需加 /webhook 可访问。

6.  Safeheron 平台服务端设置是哪个时区  
	答：服务端是 0 时区的。

7. 某些公链交易卡在 pending 中，以及如何区分源交易和加速交易  
答：咨询明确客户业务使用场景，看客户是否 cover 住这种场景，确定通过加速交易来让源交易被废弃掉；加速交易和源交易本身是两笔独立交易，加速交易可通过 replaceTxKey 与 replacedCustomerRefId 来和源交易做区分。

8.  配置 Webhook URL 与 Callback URL 协议要求  
	答：支持 HTTP 或 HTTPS 协议，URL 需 HTTP 或 HTTPS 开头。（有时候配置的 URL 包含某些特定域名时会被 Akamai 拦截，如：ngrok、webhook.site 等和一些公开的内网穿透工具域名）

9.  使用 Web3 相关的 API 接口，提示账户不存在原因  
	答：您好，请求 Safeheron  Web3 所有 API 接口，需用 Web3 钱包 accountKey。

10.  使用自动归集后，当完成自动归集交易后，是否会给我们回调归集交易信息  
	答：您好，无论是加油/归集/普通交易/加速/批量转账交易，在我们系统里本质上都是属于一笔交易，都会通过 Webhook 回调通知   
	详细 Webhook 接入参考：[API  Webhook](https://docs.safeheron.com/api/zh.html#Webhook)

11.  如何查询某个 MPC 钱包地址上的 USDT、ETH 余额资产  
	答：您好，可通过请求 **/v1/account/coin/list** 接口响应中的 balance

12.  如何查询团队下所有 MPC 资产钱包某个 coinKey 余额资产  
	答：您好，可通过请求 **/v1/coin/balance/snapshot** 接口响应中的 coinBalance

13.  新版本 SDK 是否兼容老版本 SDK   
	答：您好，兼容的，SDK 更新主要是新增了一些新的方法，老方法都有保留。

14. 自定义 Webhook 确认数开启后，是只对入账生效吗  
	答：您好，一旦开启某个公链确认数，则该公链交易入账、出账都会生效。

15. 调用 API 某个接口使用同一个 RefId，接口报错还是啥  
	答：您好，接口返回错误码 9001 ，商户业务唯一 ID 已经存在。

16. 接口 Level 和 feeRateDto 优先级是怎样，用 feeRateDto 只传 gasLimit 其它非必填字段该如何处理  
	答：Level 和 feeRateDto 两个若都传，则以 Level 传的为准；若只用 feeRateDto 且只传 gasLimt 话，交易无法创建成功，至少要传 gasLimit 和 feeRate 两个值。

17. 配置 API Key  IP 白名单支持哪些地址格式  
	答：支持 IPV4 和 IPV6 格式。

18. Webhook 通知对交易金额大小有没有过滤，类似小额转账工具和地址污染攻击入账，Webhook 会回调吗  
	答：没有过滤，只要有交易成功入账，就会有 Webhook 通知。

19. API 创建的钱包设置在 APP 展示，统计金额算可见账户还是隐藏账户  
	答：您好，为可见账户，资产统计在可见账户里。

20. 新增/修改/删除  Webhook URL 、RSA 公钥这些配置信息，是否需要审批  
	答：您好，都无需管理员参与审批，新增/修改/删除都是立即生效。

21. 不同公链获取手续费  
	答：不同公链获取手续费一般和 from、to 、vaule 金额相关，如 BTC 手续费是根据 UTXO 的数量来计算的，没有余额即没有 UTXO 的话，fee 为 0 ；当为 TRON预估手续费时，sourceAddress 必传，其目的就是看链上是否有质押，若无质押就无抵押预估。  
BTC 手续费计算方式： feeRate \* bytesSize (以 fee 为准)  
ETH 手续费计算方式： feeRate \* gasLimit  (以 fee 为准)

22. 接入 API 时常见错误码  
	答：验签错误-检查用户私钥配置(错误码：1012,Signature verification failed)     
       解密错误-检查平台公钥配置(错误码：1010,Parameter decryption failed)     

23. 当webhook配置修改后webhook将会如何推送  
答：webhook会推送到新的webhook上，且只有当删除webhook配置后，webhook才会停止推送  
24. 交易加速相关  
答：原交易和加速的交易为两笔独立的交易，加速交易将会顶掉原交易（原交易将会失败）。新交易也会有webhook推送，并且加速交易的信息可以使用原交易数据通过 /v1/transactions/one 单比交易查询接口中的 speedUpHistory 查询到

25. 已被标记为 DEPOSIT 钱包，是否可以取消标记  
答：通过传 NONE 可以取消，取消之后也可再次通过传 DEPOSIT 再次标记。

26. 创建交易时，destinationAccountType 为 VAULT_ACCOUNT ，destinationAccountKey 是否必传的  
答：destinationAccountType 为 VAULT_ACCOUNT 或 WHITELISTING_ACCOUNT 时，destinationAccountKey 分别要传 accountKey 和 whitelistKey，destinationAccountType 为 ONE_TIME_ADDRESS 时，则 destinationAccountKey 不传。

27. Webhook 响应失败，后续如何推送  
答：首次推送失败后，下次推送时间分别为 30s、1m、5m、1h、12h、24h，总共七次，若是最后一次还推送失败就不会再推送了。

## **Web Console**

1. 如何在页面上查看资产钱包、Web3 钱包 accountKey，以及页面如何查看 coinKey  
答：您好，可通过 Web Console-----钱包，通过搜索相应钱包名称 Copy accountKey；进入任意 Web Console-----钱包 ，鼠标悬浮在相应币种即可。

2. Auto Sweep 为什么没有自动执行，在哪里可看到执行成功或者失败的记录  
	答：没有执行自动归集/加油需要关注以下几个点：  
		1）待归集的钱包是否打了 DEPOSIT 标签；	  
		2）配置归集策略生效后，待归集钱包是否有入账且满足策略配置相关条件；  
		3）若配置归集策略之前待归集钱包已有 token，需重新入账或通过全量触发；  
		4）以上检查都没问题话，需看下网络 gas 是否偏低？

3. TRC20 币种能量租赁费如何充值  
答：大体分为以下三个步骤：  
	1）建立好本团队名片，且名片已审核处于生效状态；  
	2）连接 Safeheron TRON Energy 名片；  
	3）配置相关转账策略，确保目标账户包含 Safeheron TRON Energy 名片。  
	详细参考：能量 [Help Center](https://support.safeheron.com/help-center/jian-ti-zhong-wen/chan-pin/shen-ru-safeheron/tron-neng-liang-zu-lin)

4. 旧版策略是否要迁移到新版策略，以及新版本策略何时生效  
	答：需要将旧版本策略迁移到新版本策略，新策略在配置过程中不会生效，当您确认配置完毕新版策略，可联系我们 Support ，切换至新版本策略，旧版本策略则将失效。

5. 客户已使用新版本普通交易策略，现在要用 Web3 钱包该如何配置策略  
答：需要以邮件形式填好模版内容，发送给 Support 处理  
	模版连接：Web3 策略 [Help Center](https://support.safeheron.com/help-center/jian-ti-zhong-wen/chan-pin/shen-ru-safeheron/web3-ce-le)

6. 可以将归集目标地址转到插件某个 Web3 钱包吗  
答：可以在 APP 把需要转到 Web3 钱包地址加入到团队白名单，再将相应的公链 Auto Sweep 策略目标归集钱包设置成该白名单。

7. 交易记录、租赁记录、预付记录、审计日志  
答：默认是当前三个月记录，其中审计日志最大支持查询三个月内，其余交易记录、租赁记录、预付记录最大跨度时间是一年。

8. 手动触发 Webhook 频率是多少  
答：频率是 10次/min。

9. Web 是否支持修改钱包加油开关状态或者名称  
答：不支持，前往 APP 操作。

## **APP**

1. 进入 Safeheron App 使用钱包密码解锁，密码忘记怎么办  
	答：删除 App ，重新下载。

2. 创建交易响应 txKey 是审核通过后返回对，还是先返回，再来审核上链  
	答：txKey 是我们对交易的唯一标识，交易成功创建的时候就会返回。

3. 起初创建团队 Admin 为 2 个人，后期若需增加/减少 Admin 数量  
	答：管理员加入成功后，则无法再更改管理员，需要通过邮件向 Support 提出申请。

4. 团队只有 创建人 一人，是否允许设置自定义审批流  
	答：允许设置自定义审批流。

5. 创建人在设备 A 登录，切换到 B 设备登录，再切换到 A 设备登录，是直接登录还是要输入助记词  
	答：若在 B 设备输入过助记词，这时候返回 A 设备就需要重新输入助记词，判断逻辑就是是否在另一个新等设备生成新私钥，登录无所谓。

6. 助记词生成方式会在手机 APP 端呈现，如果本身手机有内置（类似云服务/VPN）后台软件可以读取我们的剪切板，又或者是本身备份者手机有风险，是不是会接触到助记词？  
	答：首先我们针对手机环境有基础的检测，包含是否有 VPN，越狱，以及本地设备和服务端时间戳一致校验；其次我们在备份助记词是有交互提示，建议是断网情况下执行；最后我们建议使用 iphone 设备，可以设置剪切板权限更安全。

7. APP 钱包排序顺序，以及支持查看钱包最大数量  
	答：价值 USD 高的排在前面，新创建钱包无金额默认在最后（超过 1000 个钱包后是直接取按创建时间前 1000 个钱包，并不是排序好再取 1000 个）支持查看最大 1000 个。

8. APP 转账到合约地址是直接拦截还是强提示，API 转账到合约呢  
	答：若提示弹层，告知风险，确认通过后可继续转账，API 则需要传 failOnContract 为 false 方可转账，failOnContract 默认是 true ，不可直接转。

9. 钱包支持删除吗  
	答：不支持删除，可修改钱包名称，可隐藏钱包或归档钱包。

10. 如何在 APP 删除成员，管理员  
	答：入口：成员管理--找到被删除人邮箱，只能删除普通成员且是没有备份过私钥分片的成员，管理员不支持删除，管理员变更需向 Support 提交邮件申请。

11. 决策模式何时生效，以及决策模式谁能修改  
	答：决策模式需要全部管理员加入激活成功才会生效，在这之前全由 创建人 决策，决策模式只有创建人 创建人 可发起修改。

12. A 邮箱在设备 a1 正常登录使用，退出使用 B 邮箱登录后再登回 A 邮箱，是否还需创建人扫码激活。  
	答：不需要。

13. APP/WEB 钱包列表、单个钱包、币种详情，以及总资产展示多久更新  
	答：总体规则是：地址有余额更新钱包就会自动刷新，对应的总资产也是；手动刷新也可即时刷新；另外汇率有定时的刷新任务 10min ，看团队钱包、币种数量等原因，一般 30 min 内可刷新完成。

14. APP 邀请成员，成员收到邮件是中文还是英文  
	答：若被邀请成员注册过，则以被邀请人的语言为准；若被邀请成员未曾注册过，则以邀请人的语言为准。

15. 私钥分片备份人是否可以从私钥分片入口看到其它备份人  
	答：无论私钥私钥分片备份人是否备份，都可以看到其它备份人。

16. Safeheron 所支持的币种汇率是从哪里获取的  
	答：coingecko 和 CMC。

17. 更换设备或删除 APP 后如何恢复团队  
	答：创建人：  
1、如果只是手机丢了，邮箱，GA 等中心化账户信息还在，可以换手机登录，把助记词导入进入  
2、如果手机丢了，物理备份的助记词也丢了，这时候账号无法恢复，只能通过转账策略把资金转移走，前提创建人在审批流程中不是必须的选项  
其他角色：  
1、如果手机丢了，中心化账户信息还在，换手机登录后创建人扫码重新激活  
2、手机丢了，物理备份的助记词也丢了，流程跟上面一样，因为其他角色恢复账户不依赖助记词

## **交易**

1. SOL Mainnet 转账上链失败原因  
	答：Solana 账户有个免租金额逻辑，用于账户避免注销掉，0.002 左右，不支持全部转走，即发起转账最大金额要减去免租金额

2. 生产环境测试网有哪些，以及如何获取  
	答：支持 Sepolia 测试网、BTC 测试网、TRON 测试网  
	1）GSK Sepolia Contract Address: 0xF191b0720cb49DdAb6ECd72a65955a35b31fc944  
	Safeheron 提供的 token ：[GSK Sepolia Token Faucet](https://sepolia.etherscan.io/address/0x98453d6a16d8A18D6B5F6b1EC521a7620544E359#writeContract)  
	2）USDC Sepolia Contract Address: 0x1c7D4B196Cb0C7B01d743Fbc6116a902379C7238  
	Sepolia 官方提供的 token ：[USDC Sepolia Token Faucet](https://faucet.circle.com/)  
	3\)   TLK Tron  Contract Address: TC2jGCcJUh33oAeQHYHBXu7KVVxrjitkHw  
	Safeheron 提供的 token ：[TLK Tron Token Faucet](https://shasta.tronscan.org/#/contract/TZ9h1LA9b3naHFCKEtwa5eHVzZuhZAD3gq/code%20)  
	4）[BTC TESTNET Faucet](https://coinfaucet.eu/en/btc-testnet4/)

3. 平台哪些公链是支持加速交易  
答：支持加速的 UTXO：BTC、BCH、DASH、LTC、DOGE  
       支持加速的非 UTXO：EVM、FIL、Aptos、CFX  
       不支持加速的链：NEAR、SUI、TRON、SOL、TON

4. 若钱包被 AML 污染了，怎样处理该地址合适  
	答：建议更换地址，并将收到的有问题资产返回给用户，做到流程合规。

5. 哪些公链有超时机制，分别超时时间是多少  
	答：TRON 、TON 是 23 小时；SOL 是 90s，Aptos 是 30 天。

6. 哪种情况会自动添加币种  
	答：主网币种打开，入账 token ，会把相应的 token 币种添加成功，API 添加 token 会自动将该 token 对应的主网币种自动添加成功。

7. 归集触发条件 \> 10 u ，入账了一笔 50 u ，某种原因没有自动归集，想要再触发该如何处理  
	答：可往待归集钱包充值任意金额或通过全量扫描进行归集

8. nonce 基本规则梳理  
	答：nonce 是审批交易时候确认的，相同时间发起多笔交易在审批中，哪个先审核就以就哪个先占用当前推荐的 nonce 值。  
	1）如果maxUseNonce=null 只能取chainNonce，如果不为空如下  
	2）nonce \< chainNonce 提示"Nonce 过低"  
	3）nonce \> maxUseNonce+1 提示"nonce 不能大于 maxUseNonce+1"  
	4）chainNonce \<= Nonce \<= maxUseNonce 进行弹窗提示，用户进行确认

9. gas 在审批时过低提示（支持 FIL、SOL、TRON、EVM）  
答：审批的时候判断逻辑就是当前交易的 maxFee 是否小于 queryFeeLevelForAudit  
接口返回 min 等级(链上原始值，未乘以系数)的 maxFee ，也就是是否小于链上返回的 maxFee， 如果满足 就会返回 feeRateTips=1 ,app 就会触发更新提示。

10. Tron from 地址有质押，大概什么情况下会出现负数  
答：质押 from 地址的这个范围  bandwidth available \< 360 且 energy available \> 360的情况会出现负数。

11. 插件里的 EVM 主链是否可在插件加速，有无加速按钮透出  
答：插件没有加速按钮，不可再插件加速，但是可以通过再次发送交易自定义相同nonce 的方式实现加速的效果。

12. APTOS 公链支持 token 转账吗以及原因  
答：不支持，Aptos token 转账需要 to 地址发起 register 才能转入，对我们来说，系统改造很大暂未支持。

13. 钱包只添加 ETH 和 USDT 币种，充值了 ETH 的 USDC，钱包没有显示币种原因  
答：余额需要到 USDC 币种详情刷新更新，入账记录需要手动补偿。

14. TRON 手续费预估基本逻辑是啥，低中高有什么区别  
答：TRON 手续费是预执行，链上返回多少就是多少，没有低中高，在用户侧看到低中高都是一样的。

15. UTXO 币种加速遇到 pending 中，出现请耐心等待，具体原因  
	答：CPFP 加速时会判断源交易的费率是不是满足当前链上费率，如果已经满足了，就没必要再 CPFP 加速，就会提示这个。

16. 钱包数据文件邮件多久更新一次，发送到团队谁的邮箱里  
	答：您好，钱包数据文件发送必须是要有新增的钱包和新的币种添加，若团队内地址无变化，不会主动一月发送一次，需要手动去 Web Console 下载；发送到备份人邮箱。

17. 已开通能量租赁功能，gas 费如何预估及如何展示  
	答：开通后，交易转账系统会自动根据可用租赁资金的余额，自动估算并展示所需的 gas 费用。可用租赁余额充足话，gas 展示的是租赁能量消耗和该交易带宽所需费用；可用租赁余额不够的话，gas 直接展示的是需要消耗多少 TRX 数量。

18. 交易状态一直是签名中原因  
答：由于MPC计算需要多轮P2P通信，所以客户端网络需要保持稳定，最后一个审批人再次重试审批下就能签名。

19. 为什么配置了归集策略但是没有归集  
答：我们归集逻辑是按照充值的事件来触发归集，而不是按照配置的归集时间轮询所有钱包，所以排查的思路是归集策略生效时间是否晚于充值的时间，或者钱包没有标记Deposit标签，或者配置的归集条件gas不合理等。

20. UTXO 触发归集相关的基本规则  
答：手动归集、API 归集，当前生产环境 Vault 配置的是 30 ，当 UTXO 碎片数量\<= 30 时，不建议归集，当 UTXO 碎片数量 \> 30 时，建议归集；其中 Auto Sweep 中的相关 UTXO 公链满足条件即可归集。

## **权限**

1. 添加 Web3 自定义网络需要成员拥有什么权限  
	答：您好，需要成员拥有 “管理团队钱包” 权限，且团队为正式团队。

2. 手动触发 Web Console  Webhook 需要成员拥有什么权限  
	答：您好，需要成员拥有 “发起交易” 权限。

3. 取消某一笔审批中或签名中的交易，需要成员、API Key 拥有什么权限  
	答：您好，需要成员、API Key 拥有 “发起交易” 权限。

4. 进入插件的某个团队里，需要成员拥有什么权限  
	答：您好，需要成员拥有 “发起交易” 权限。

5. 进入插件的某个团队里，要成员拥有什么权限  
	答：您好，需要成员拥有 “发起交易” 权限。

6. 修改 API Key、Webhook、确认数，需要成员拥有什么权限  
	答：您好，需要成员拥有 “API 管理” 权限。

7. 修改 API Key、Webhook、确认数，需要成员拥有什么权限  
	答：您好，需要成员拥有 “API 管理” 权限。

8. 开启、修改 “自定义审批流” 拥有什么权限  
	答：您好，入口只对 Owner 和 Admin 开放，故只有 Owner 和管理员可开启。

9. 开启、关闭 “自定义审批流” 需要哪些人参与审批  
	答：开启、关闭 “自定义审批流” 是需要全部管理员审核，非决策模式。

10. 审批节点权限是否依赖成员管理权限  
	答：审批节点所有人都可看，拥有“成员管理”权限的人才能进行增加和修改审批节点。

11. 策略引擎” 中策略新增、修改、删除、排序等需要成员拥有什么权限  
	答：您好，需要成员拥有 “管理交易策略” 权限。

12. “Auto Sweep” 中策略新增、修改、删除等需要成员拥有什么权限  
	答：您好，需要成员拥有 “管理交易策略” 权限。

13. “Connect” 中名片新增、修改、删除等需要成员拥有什么权限  
	答：您好，需要成员拥有 “管理白名单/Connect 应用” 权限。

14. 查看 “审计日志” 中各种日志信息，需要成员拥有什么权限  
	答：您好，“审计日志” 只有管理员、Owner 可查看，成员不可查看。

15.  创建钱包、修改钱包名称，需要成员拥有什么权限  
	答：您好，需要成员拥有 “管理团队钱包” 权限。

16.  预付租赁费、配置报警阈值，需要成员拥有什么权限  
	答：您好，需要成员拥有 “发起交易” 权限。

17.  加入到自定义审批流里，需要成员拥有什么权限  
	答：您好，需要成员拥有 “管理白名单” 权限。

18.  进入 Co-Signer 应用需要什么权限 ，下载 CLI 和部署文什么权限  
	答：进入 Co-Signer 无需权限，下载 CLI 和部署文档需成员拥有 “API 管理”权限。

19.  点击 “全量扫描” 按钮 ，需要成员拥有什么权限  
	答：需要成员拥有 “管理交易策略” 权限。

20.  APP/Web AML Checker，需要成员拥有什么权限  
	答：无需成员任何 权限可正常使用。

21.  开启钱包加油开关，需要要成员拥有什么权限  
	答：需要成员拥有 “钱包管理权限”。

## **策略**

1.  老策略里面 24 小时是按照哪个时区  
	答：UTC 0 时区。

2.  生产环境支持白名单添加的数量  
	答：您好，支持添加最大白名单数量为 800，可内部提 sql 增加。

3.  修改决策模式提示“团队内部有任务审核中，因此无法修改决策模式”  
	答：需要您先处理完处于决策模式的审批待办，处理完成方可修改。

4.  策略门限 1/2 ，A 与 B 两人审核，A 审核卡在签名中，如何取消交易  
	答：~~若 A 有“发起交易”权限，A 可直接在待办取消；若 A 无“发起交易”权限，当前只能调用 API 取消接口来取消。~~（现有发起交易权限的人都可取消）

5.  Safeheron 支持新策略配置最大多少条  
	答：创建的默认团队支持最大 10 条，若客户有特殊要求，可根据团队 ID 给到策略，支持最大配置 30 条。

6.  设置策略价值/数量/累计/出现“范围不能重叠，请重新配置”  
	答：策略区间设置的范围不能存在跳开、包含现象。如您上面这个设置第一条是 【0，100000000），下面有一条设置了【10000000，∞），是由于 10000000 这个数值在 【0，100000000） 范围内，就重复重叠了，如果这样设置成功了不就对应了同一个范围内有两个审批流了肯定是不行的，正常的价值范围区间设置范围举例比如：【0，1000）对应给其设置审批流1；【1000，10000）对应 给其设置审批节点2， 依此类推。

7. 满足 Auto Sweep 策略归集条件，某种原因导致归集失败，如何触发  
	答：新入账一笔任意金额的交易，达到 Auto Sweep 策略频率会自动归集/全量扫描。

## 

## **模版**

**更改管理员数量/决策模式**  
邮件模版：  
	可以按照以下步骤操作：由任何管理员的电子邮件向 support@safeheron.com 发送电子邮件，并抄送所有其他管理员的电子邮件地址。一旦有一定数量的管理员回复同意，我们将相应地配置设置（注意：“一定数量”是指决策模式的阈值，例如3/5，那么我们需要3个管理员回复他们的同意;如果您的团队决策模式为 1/x，您可以直接发送电子邮件，而无需抄送其他管理员）。  
以下是电子邮件模板：   
电子邮件主题：团队管理员变更   
内容：  
团队 ID：  
新管理员数量：x   
新决策模式：x/x  
删除管理员（是否需要删除管理员）:

**申请 MPC Sign 策略模版**  
链接（中文）：[https://support.safeheron.com/help-center/jian-ti-zhong-wen/chan-pin/shen-ru-safeheron/mpc-sign-ce-le](https://support.safeheron.com/help-center/jian-ti-zhong-wen/chan-pin/shen-ru-safeheron/mpc-sign-ce-le)

链接（英文）：  
[https://support.safeheron.com/help-center/product-and-solution/dive-into-safeheron/mpc-sign-policy#view-mpc-sign-policies](https://support.safeheron.com/help-center/product-and-solution/dive-into-safeheron/mpc-sign-policy#view-mpc-sign-policies)

**申请 Web3  策略模版**  
链接（中文）：[https://support.safeheron.com/help-center/jian-ti-zhong-wen/chan-pin/shen-ru-safeheron/web3-ce-le](https://support.safeheron.com/help-center/jian-ti-zhong-wen/chan-pin/shen-ru-safeheron/web3-ce-le)

链接（英文）：  
[https://support.safeheron.com/help-center/product-and-solution/dive-into-safeheron/web3-policy](https://support.safeheron.com/help-center/product-and-solution/dive-into-safeheron/web3-policy)

**GA 忘记，需重置**  
链接（中文）：  
[https://support.safeheron.com/help-center/jian-ti-zhong-wen/chan-pin/bang-zhu/wang-ji-gu-ge-yan-zheng](https://support.safeheron.com/help-center/jian-ti-zhong-wen/chan-pin/bang-zhu/wang-ji-gu-ge-yan-zheng)

链接（英文）：  
[https://support.safeheron.com/help-center/product-and-solution/support/what-if-i-forget-google-authentication](https://support.safeheron.com/help-center/product-and-solution/support/what-if-i-forget-google-authentication)  
**创建人更换邮箱，参考模版：**

如果创建人旧邮箱还能使用，请使用旧邮箱向我们发送邮件support@safeheron.com 并抄送所有其他管理员的电子邮件地址。  
以下是电子邮件模板：  
电子邮件主题：更换创建人邮箱  
内容：团队 ID，新的邮箱（不可以是注册过 Safeheron 的邮箱）
