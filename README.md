# JGLUE Evaluation with lm-evaluation-harness (Google Colab)

Google Colab 上で [EleutherAI lm-evaluation-harness](https://github.com/EleutherAI/lm-evaluation-harness) を使い、**JGLUE** (Japanese General Language Understanding Evaluation) の 4 タスクを評価するためのノートブックです。

## 必要な lm-eval バージョン

- **lm-eval >= 0.4.6** 必須 (JGLUE タスクは v0.4.6 で `japanese_leaderboard` 配下として追加されました — [PR #2439](https://github.com/EleutherAI/lm-evaluation-harness/pull/2439))
- v0.4.5 以前には JGLUE タスクが含まれていません

## 対象タスク

| Task ID | タスク | 出力形式 | 評価指標 | few-shot デフォルト |
|---|---|---|---|---|
| `ja_leaderboard_jcommonsenseqa` | 多肢選択 (常識推論) | multiple_choice | Accuracy | 3 |
| `ja_leaderboard_jnli` | 多肢選択 (自然言語推論) | multiple_choice | Accuracy | 3 |
| `ja_leaderboard_jsquad` | 抽出型 QA | generate_until | Exact Match | 2 |
| `ja_leaderboard_marc_ja` | 多肢選択 (感情分析・2値) | multiple_choice | Accuracy | 3 |

> **注意**: ベア ID (`jcommonsenseqa`, `jnli`, `jsquad`, `marc_ja`) は現行 lm-eval では使えません。必ず `ja_leaderboard_*` プレフィックスを付けてください。

## 使い方

1. [`JGLUE_eval.ipynb`](./JGLUE_eval.ipynb) をダウンロード
2. [Google Colab](https://colab.research.google.com/) を開き、**File → Upload notebook** でアップロード
3. **Runtime → Change runtime type** で GPU (T4 / A100 / L4) を選択
4. 上から順番にセルを実行
5. 「設定」セルで評価対象モデルを変更可能

### デフォルト設定

- **モデル**: `rinna/japanese-gpt2-small` (Colab Free の T4 で動作確認済み)
- **タスク**: JGLUE 全 4 タスク (`ja_leaderboard_*`)
- **Few-shot**: 0-shot (JGLUE 論文準拠。`NUM_FEW_SHOT=None` にすると各タスク yaml デフォルトの 2〜3 shot になります)
- **Batch size**: 8

## 推奨モデル例

| サイズ | モデル | 想定 GPU |
|---|---|---|
| Small | `rinna/japanese-gpt2-small` | T4 (Free) |
| Medium | `line-corporation/japanese-large-lm-1.7b` | T4 / A10G |
| Large | `elyza/ELYZA-japanese-Llama-2-7b-instruct` | A100 (40GB) |
| Large | `tokyotech-llm/Swallow-7b-instruct-v0.1` | A100 (40GB) |
| Large | `mistralai/Mistral-7B-v0.3` | A100 (40GB) |

7B 以上のモデルを評価する場合は、ノートブック末尾の **vLLM バックエンド** セクションを使うと 5–10 倍高速化します。

## トラブルシューティング

| 症状 | 対処 |
|---|---|
| `TaskNotFound` | `lm_eval --tasks list` で正確なタスク名を確認。`ja_leaderboard_*` プレフィックス必須 |
| `lm-eval` のバージョンが古い | `!pip install -U "lm-eval>=0.4.6"` で更新 |
| `OOM` | `BATCH_SIZE` を下げる / より小さいモデルを使う |
| `trust_remote_code` エラー | ノートブックの設定で `TRUST_REMOTE_CODE = True` にする |
| `jsquad` の EM が常に 0 | `generate_until` 型タスクです。非インストラクトモデル (gpt2 等) では指示フォーマットに追従できません。インストラクトモデルを使うか `--gen_kwargs do_sample=False` を追加 |
| MARC-ja タスクが見つからない | `ja_leaderboard_marc_ja` (プレフィックス + アンダースコア) を使用 |

## 参考文献

- lm-evaluation-harness: https://github.com/EleutherAI/lm-evaluation-harness
- JGLUE paper: https://aclanthology.org/2022.acl-long.172/
- JGLUE HF dataset: https://huggingface.co/datasets/Rakuten/JGLUE
- Japanese Leaderboard タスク yaml: https://github.com/EleutherAI/lm-evaluation-harness/tree/main/lm_eval/tasks/japanese_leaderboard
- Japanese Leaderboard 追加 PR (#2439): https://github.com/EleutherAI/lm-evaluation-harness/pull/2439

## License

MIT
