# zhengjue-corpus — 《正觉藏》数字藏经语料、全量 CBETA 纳管目录、五位一体变更溯源与破邪显正靶标主仓 (v3.3)

> **定位**：`zhengjue-canon` 第二层千年数字法宝核心资产仓。
> 1. **以 100% 全量 CBETA 为目录与坐标底座（`catalog/*.yaml`）**：对全部 CBETA 典籍实施四级收录策略（`COLLECTED_REVISED` / `COLLECTED_ANNOTATED_ONLY` / `UNCOLLECTED_REFUTATION_TARGET` / `UNCOLLECTED_TAGGED_ONLY`）与多维标签管理；
> 2. **收录经卷采用「五位一体（5-Part Canonical Bundle）」结构（`canons/`）**：聚焦汉传佛教核心三转法轮真经、菩萨正论与古德注疏，完整保存从 CBETA 原始底本（`*.cbeta.json`）到最终公开版（`*.md` + `*.standoff.json`）的 **100% 全量改动账本（`*.changes.jsonl`）** 与 **修改意见内部 Markdown 专文（`articles/ART-*.md`）**；
> 3. **不收录外典与近代讲记采用「破邪显正反向靶标账本」（`refutations/`）**：针对印顺全集（`Y`）、宗喀巴广论（`B`）及部分近代六识论讲记，不收录整卷全文（严守近现代著作权边界并保持仓库轻量），仅锚定其原始 CBETA 坐标、法定合理引用短句（`≤300字`）、邪见标签、破斥专文与正法藏圣教量双向对读锚点。

---

## 一、 仓库目录与「五位一体 + 破邪显正靶标」标准文件结构

```text
zhengjue-corpus/
├── catalog/                                # 【全量 CBETA 四级收录策略与多维标签主目录】
│   ├── orthodox-canon.yaml                 #   1. 正法藏部 (COLLECTED_REVISED)
│   ├── zhengjue-treasury.yaml              #   2. 正觉法宝部 (COLLECTED_REVISED)
│   ├── sectarian-reviewed.yaml             #   3. 部派存查部 (COLLECTED_ANNOTATED_ONLY)
│   ├── pseudo-buddhist.yaml                #   4. 伪佛法部 (COLLECTED_ANNOTATED_ONLY)
│   └── expunged-blacklist.yaml             #   5. 不收录外典与近代讲记打标目录 (UNCOLLECTED_REFUTATION_TARGET / UNCOLLECTED_TAGGED_ONLY)
├── canons/orthodox/T31n1585/               # 【收录经卷：单卷「五位一体」标准文件结构】
│   ├── 001.cbeta.json                      #   [1] CBETA 原始基准快照 (L0-L3 零丢失验证，净室剥离现代标点，永不手改)
│   ├── 001.changes.jsonl                   #   [2] 全过程改动账本 (断句/订正/分段/8种字型/批注 + 完整审阅链条)
│   ├── articles/                           #   [3] 修改意见与法义辨正 Markdown 专文库
│   │   └── ART-2026-0001.md
│   ├── 001.md                              #   [4] 最终公开定稿主文 (由 Project(001.cbeta.json, 001.changes.jsonl) 生成)
│   └── 001.standoff.json                   #   [5] 最终公开伴生图层 (行号里程碑、校勘记、8种字型区间、溯源索引)
└── refutations/                            # 【不收录文献：破邪显正反向靶标引文与批驳账本】★ 核心！
    └── Y/
        └── Y08n0008.refutations.jsonl      #   锚定印顺《成佛之道》等不收录外典之 CBETA 坐标 + ≤300字合理引用短句 + 破斥专文与正法对读锚点
```

---

## 二、 确定性状态重放守恒定律（Deterministic Replay Invariant）

$$\text{Project}\Big(\texttt{001.cbeta.json},\ \big[\Delta_1, \Delta_2, \dots, \Delta_m\big]_{\texttt{001.changes.jsonl}}\Big) \equiv \big(\texttt{001.md},\ \texttt{001.standoff.json}\big)$$

- **底本错字直接订正（`TEXT_EMENDATION`）**：最终公开版正文 `001.md` 直接呈现订正后的正确文字，并在 `001.standoff.json` 中挂载原字溯源锚点；
- **零暗改保障**：任何未在 `001.changes.jsonl` 账本中显式登记的文字或标点变动，均会被 CI 重放校验器立即拦截阻断。
