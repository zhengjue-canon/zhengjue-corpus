# AGENTS.md — `zhengjue-corpus` 语料与账本守恒规范

1. **严禁手改 `*.cbeta.json`**：`*.cbeta.json` 只能由 `cbeta-markdown compile-baseline` 生成。
2. **严禁脱离账本直接手改 `*.md`**：对经文句读、底本错字订正、分段或 8 种字型的任何修改，必须写入 `*.changes.jsonl` 并通过 `cbeta-md project` 重放生成 `*.md` 与 `*.standoff.json`。
3. **修改意见长文规范**：所有关联至 `ChangeRecord` 的完整 Markdown 专文统一存放于同目录下的 `articles/ART-<sutraId>-<fascicle>-<seq>.md`，支持独立递增 `revision` 版本。
