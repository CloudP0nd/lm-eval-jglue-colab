# JGLUE Evaluation with lm-evaluation-harness (Google Colab)

Google Colab 上で [EleutherAI lm-evaluation-harness](https://github.com/EleutherAI/lm-evaluation-harness) を使い、**JGLUE** (Japanese General Language Understanding Evaluation) の 4 タスクを評価するためのノートブックです。

## 対象タスク

| Task ID | タスク | 評価指標 |
|---|---|---|
| `jcommonsenseqa` | 多肢選択 (常識推論) | Accuracy |
| `jnli` | 多肢選択 (自然言語推論) | Accuracy |
| `jsquad` | 抽出型 QA | Exact Match / F1 |
| `marc_ja` | 多肢選択 (感情分析) | Accuracy |

## 使い方

1. [`JGLUE_eval.ipynb`](./JGLUE_eval.ipynb) をダウンロード
2. [Google Colab](https://colab.research.google.com/) を開き、**File → Upload notebook** でアップロード
3. **Runtime → Change runtime type** で GPU (T4 / A100 / L4) を選択
4. 上から順番にセルを実行
5. 「設定」セルで評価対象モデルを変更可能

### デフォルト設定

- **モデル**: `rinna/japanese-gpt2-small` (Colab Free の T4 で動作確認済み)
- **タスク**: JGLUE 全 4 タスク
- **Few-shot**: 0-shot (JGLUE 公式設定)
- **Batch size**: 8

## 推奨モデル例

| モーダル | モデル | 想定 GPU |
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
| `OOM` | `BATCH_SIZE` を下げる / より小さいモデルを使う |
| `trust_remote_code` エラー | ノートブックの設定で `TRUST_REMOTE_CODE = True` にする |
| `TaskNotFound` | `lm_eval --tasks list` で正確なタスク名を確認 (バージョン依存) |
| `jsquad` の EM が常に 0 | `--gen_kwargs do_sample=False` を追加 |
| MARC-ja タスクが見つからない | `marc_ja` (アンダースコア) を使用 |

## 参考文献

- lm-evaluation-harness: https://github.com/EleutherAI/lm-evaluation-harness
- JGLUE paper: https://aclanthology.org/2022.acl-long.172/
- JGLUE HF dataset: https://huggingface.co/datasets/JGLUE

## License

MIT
