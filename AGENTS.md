# AGENTS.md — `zhengjue-canon` 全局 AI 驱动开发顶层宪法 (v3.5)

欢迎参与《正觉藏》数字化编审、句读审定、全量变更溯源与多端协同平台（`zhengjue-canon`）的开发。所有 AI Coding Agent 在任何子仓库执行设计、编码、重构或测试前，**必须首先阅读并严格遵守本规范**。

---

## 一、 十一大不可违背的业务、数学与法律红线（Hard Invariants）

1. **法律合规与版权净室红线（Legal & Clean-Room Invariant，详见 `REQ-08`）** ★ 最高红线：
   - **底本净室剥离**：`cbeta-markdown` 在生成基准快照时，必须默认剥离 CBETA 专有的现代新式标点与 `resp="CBETA"` 按语，仅提取 **100% 属于公共领域（Public Domain）的《大正藏》古籍纯汉字原文、1924 年原版册页栏行号、原版古句读与原版宋元明丽古本校勘记**；《正觉藏》公开的整卷现代句读 100% 由正觉独立初标与导师审定生成。
   - **近现代受保护作品严禁全文入库**：作者或译者逝世未满 70 年之近现代著作/译本（如法尊译本、印顺著作等）强制标记 `PROTECTED_QUOTE_ONLY`，**严禁全文入库或公开展示**，仅限在辨正专文（`ART-*.md`）中依法定“合理使用（适当引用）”短句引述（单篇引用 ≤ 300 字且注明完整出处）。
   - **依法不依人**：所有划词批注与辨正专文严格聚焦经论文本与唯识法相辨析，严禁包含针对在世自然人或现存法人的人身侮辱词汇。
   - **字体与依赖零侵权**：WebFont 与 App 内嵌字体默认仅限 `SIL OFL 1.1` 开源免费商用字库（或已登记授权证书编号之字体）；严禁引入 `GPL / AGPL / CC-BY-NC` 传染性依赖。
   - **义工协议强制外键**：任何 `ChangeRecord` 或 `ReviewArticle` 入库前，作者账号必须已签署电子版《著作权归属暨永久不可撤销公益流通授权协议》（绑定非空 `agreementId`）。

2. **源头零丢失不变量（Baseline Lossless Invariant，详见 `REQ-02`）**：
   - 基准快照必须 100% 通过 **L0~L3** 四级形式化验证，严禁人工手改。

3. **从基准底本到最终公开版的 100% 全量改动溯源不变量（Deterministic Replay Invariant，详见 `REQ-03`）**：
   - 从基准底本到最终公开版 `001.md` 与 `001.standoff.json` 的任何变化（标点、错字订正、分段、8种字型、划词批注）必须 100% 记录在 `001.changes.jsonl` 中，满足：
     $$\text{Project}(\texttt{001.cbeta.json}, \texttt{001.changes.jsonl}) \equiv (\texttt{001.md}, \texttt{001.standoff.json})$$

4. **底本错字直接订正与原貌溯源原则（Direct Emendation with Provenance）**：
   - 遇到底本错字，最终公开版正文直接显示订正后的正确文字，并在该字格坐标挂载 `TEXT_EMENDATION` 溯源锚点。

5. **内外部一致的全透明修订历史（Unified Internal & Public Provenance，详见 `REQ-06`）**：
   - 外部公开阅读端与内部编审管理端展示完全一致的改动历史链条（`reviewChain[]`）、更改人、历次审阅人、修改意见及关联的完整 Markdown 专文（`articles/ART-*.md`）。

6. **“评审当下撰文 + 查看我的评审 + 事后补充专文”双模式工作流（详见 `REQ-04`）**：
   - 审阅人既可当场分屏写完整 Markdown 文章，也可勾选 `needsArticleSupplement: true`（「🔖 待补充长文」），随后在 **「我的评审与文章中心」** 查看自己的全部评审并回溯经文上下文补充文章。

7. **三级结集发版门禁与开放法宝流通（Release Versioning & Open Distribution，详见 `REQ-07`）**：
   - 严格区分大版本（`v1.0.0` 年度结集）、小版本（`v1.1.0` 新增定稿经卷/专文）与补丁版（`v1.1.1` 勘误微调），未达 `FINAL_APPROVED` 的半成品经卷严禁进入发版清单；公开版不设防爬与数字水印。

8. **双坐标系（`LineTokenId` + Unicode CodePoint）寻址铁律**：
   - 持久化锚点统一使用 `LineTokenId`（如 `0001b14#c05`、`0001b14#g05`）防止跨行漂移，底层字符计算统一使用 `@zhengjue/standoff` 的 `CodePointString`，严禁对经文字符串直接调用 UTF-16 `.length` 或 `.slice()`。

9. **全量 CBETA 纳管打标与「不收录文献破邪显正」反向靶标原则（详见 `REQ-01`、`REQ-06`、`REQ-08`）**：
   - 目录与底层坐标索引必须基于 **100% 全量 CBETA 语料库**；绝大多数晚期、藏传及近现代文献（如近代法师讲记、印顺全集 `Y`、宗喀巴广论 `B`）打上分类标签并标记为 `UNCOLLECTED_TAGGED_ONLY` 或 `UNCOLLECTED_REFUTATION_TARGET`（不收录整卷正文、不参与句读修订）；
   - 对于具「破邪显正」反向价值的靶标文献（`UNCOLLECTED_REFUTATION_TARGET`），保留其 CBETA 原始坐标寻址，在 `refutations/**/*.refutations.jsonl` 中仅锚定 `≤ 300 字` 的法定评论性合理引用短句、邪见标签、破斥专文（`ART-*.md`）及正法藏圣教量双向对读锚点，兼顾破邪显正价值、近现代著作权合规与 Git 仓库轻量性。

10. **学界与大众精准引文定位暨跨版本永不漂移原则（`ZJC-Cite` Canonical Citation Invariant，详见 `REQ-11`）**：
   - 前端（`zhengjue-web`、`zhengjue-admin`、`zhengjue-mobile`）在划词选择复制经典引文时，必须通过 `@zhengjue/citation` 自动反查选区对应的底本 `LineTokenId` 字格区间（如 `ZJC:T31n1585:001.0001a08#c01-09#c02`），默认附带标准学术引文尾注与永久直达短链（`https://canon.zhengjue.org/c/T31n1585/1/0001a08.01-09.02`），并支持纯文本、Markdown、BibTeX 与二维码金句卡 4 种复制模式；
   - 外部读者或学者无论点击短链 URL，还是将引文码/带尾注引文直接粘贴至《正觉藏》搜索框，前端必须 **0ms 识别并直达目标经卷、自然段与精确字格区间并执行脉冲高亮**，跨越任何后续标点或分段修订版本永不漂移。

11. **Admin 三位一体接入层（Web + Tauri v2 桌面端 + MCP Server）与 AI Agent 辅助编审治理红线（详见 `REQ-11`）**：
   - `zhengjue-admin` 采用 **Web 端（React 19 + Vite 8） + 跨平台原生桌面端（Tauri v2，支持离线 SQLite/Git、本地数百 GB 古籍原拓直读、硬件加密密钥签章） + AI Agent 接入层（`@zhengjue/mcp-server`，支持 `stdio` 与 `Streamable HTTP`）** 三位一体架构；
   - **AI Agent 严禁越权终审铁律**：任何通过 MCP Server 接入的 AI Agent 仅限作为「初标与起草提案助手（Proposer）」，其提交的 `ChangeRecord` 或 `ReviewArticle` 强制记录 `agentMetadata`（含 `viaMcp: true`、`modelId`、`humanSupervisorId`），且在状态机中**最高只能推进至 `DRAFT` 或 `INITIAL_SUBMITTED` 状态**；一审（`FIRST_REVIEWED`）、二审（`SECOND_REVIEWED`）与平实导师终审定稿（`FINAL_APPROVED`）在协议层物理禁止 MCP 调用。
