# zhengjue-corpus — 《正覺藏》（正覺大藏經）雙軌制 Markdown 數位語料庫

<p align="center">
  <img src="https://raw.githubusercontent.com/zhengjue-canon/.github/main/assets/banner.svg" width="100%" alt="Zhengjue Canon Banner" />
</p>

本倉庫為 **《正覺藏》（Zhengjue Canon）** 的純文本數位語料資產倉，由 [`zhengjue-platform`](https://github.com/zhengjue-canon/zhengjue-platform) 工具鏈生成、校驗並同步審定簽核記錄。

---

## 一、 藏經四維目錄結構（Canonical Tiers）

```text
zhengjue-corpus/
├── catalog/
│   ├── orthodox-canon.yaml      # 1. 【正法藏部】七葉窟內外結集三乘真經、諸大菩薩正論、審定平反之真經
│   ├── zhengjue-treasury.yaml   # 2. 【正覺法寶部】平實導師全部講經講義、《成唯識論釋》、法義辨正專書
│   ├── sectarian-reviewed.yaml  # 3. 【部派論典辨正部】《大毘婆沙論》等部派凡夫論典（鎖定原文 + 電腦注解式辨正）
│   ├── pseudo-buddhist.yaml     # 4. 【偽佛法部】《大日經》《金剛頂經》等密續偽經（附一對一辨正專文與剪貼分析）
│   └── expunged-blacklist.yaml  # 5. 【徹底剔除隔離庫】CBETA《大藏經補編》混入之宗喀巴《廣論》與近代六識論著作
└── canons/
    ├── orthodox/                # 正法藏部經卷 (.md + .standoff.json)
    ├── treasury/                # 正覺法寶部典籍 (.md + .standoff.json)
    ├── sectarian/               # 部派論典辨正部 (.md + .standoff.json)
    └── pseudo/                  # 偽佛法部辨正對照 (.md + .standoff.json)
```

---

## 二、 雙軌制語料規範（`ZJ-Markdown` + `Standoff Sidecar`）

每一卷經論由兩個同名文件組成，並通過 **逐字零丟失哈希校驗（`base_sha256`）**：

1. **`{work_id}_{fascicle_no}.md`（經卷主文件）**：
   - 包含 YAML 元數據頭、純淨經論正文、正覺審定新式句讀（逗標）、內嵌科判指令（`::keping[...]`）以及正覺 8 種多字型語義指令（`:::gatha` 頌文、`:question[...]` 問句、`:kepan_hint[...]` 判教提示等）。
2. **`{work_id}_{fascicle_no}.standoff.json`（伴生圖層文件）**：
   - 按 **Unicode Code Point 純漢字偏移量** 精確掛載：
     - 原書頁欄行號里程碑映射（`<lb>` / `<pb>`）；
     - 宋、元、明、高麗藏等古本異文校勘記（`<app><lem><rdg>`）；
     - 《大正藏》/CBETA 原有舊斷句對照層；
     - 平實導師與編審老師之「類電腦檔案注解」劃詞辨正批注；
     - 經審定公開之大眾質疑與正覺辨正問答記錄。
