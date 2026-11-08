# zhengjue-corpus — 《正觉藏》数字藏经语料、四部判教目录与五位一体全量变更溯源主仓 (v3.0)

> **定位**：`zhengjue-canon` 第二层千年数字法宝核心资产仓。采用 **「五位一体（5-Part Canonical Bundle）」** 结构，完整保存从 CBETA 原始底本（`*.cbeta.json`）到最终公开版（`*.md` + `*.standoff.json`）的 **100% 全量改动账本（`*.changes.jsonl`）** 与 **修改意见内部 Markdown 专文（`articles/ART-*.md`）**。

---

## 一、 单卷「五位一体」标准文件结构

以《成唯识论》卷第一（`canons/orthodox/T31n1585/001`）为例：

```text
canons/orthodox/T31n1585/
├── 001.cbeta.json          # 【1. CBETA 原始基准快照 (Immutable Baseline)】
│                           #    由 cbeta-markdown 从 TEI-P5 XML 提取并通过 L0-L3 四级零丢失验证，永不手改！
├── 001.changes.jsonl       # 【2. 全过程改动账本 (Full-Lifecycle Change Ledger)】★ 核心！
│                           #    记录从 001.cbeta.json 到最终版的每一处标点断句、底本错字订正、字型标注与批注，含更改人、审阅人链条及关联专文 ID
├── articles/               # 【3. 修改意见内部 Markdown 专文库 (Review Articles)】★ 核心！
│   └── ART-T31n1585-001-0001.md  # 评审当下撰写、或事后在『我的评审与文章中心』回溯补充的完整 Markdown 辨正/考证文章
├── 001.md                  # 【4. 最终公开定稿主文 (Published Canonical MD)】
│                           #    由 Project(001.cbeta.json, 001.changes.jsonl) 100% 确定性生成；正文直接显示订正后的正确文字与新句读
└── 001.standoff.json       # 【5. 最终公开伴生图层 (Published Standoff Sidecar)】
                            #    挂载在 001.md 字格坐标上的行号里程碑、古本校勘记、8种语义字型行内区间、划词辨正批注及全量改动溯源索引
```

---

## 二、 确定性状态重放守恒定律（Deterministic Replay Invariant）

$$\text{Project}\Big(\texttt{001.cbeta.json},\ \big[\Delta_1, \Delta_2, \dots, \Delta_m\big]_{\texttt{001.changes.jsonl}}\Big) \equiv \big(\texttt{001.md},\ \texttt{001.standoff.json}\big)$$

- **底本错字直接订正（`TEXT_EMENDATION`）**：最终公开版正文 `001.md` 直接呈现订正后的正确文字，并在 `001.standoff.json` 中挂载原字溯源锚点；
- **零暗改保障**：任何未在 `001.changes.jsonl` 账本中显式登记的文字或标点变动，均会被 CI 重放校验器立即拦截阻断。
