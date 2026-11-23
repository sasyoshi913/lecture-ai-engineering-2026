# 第6回 演習・宿題

## 演習

- **STEP 1 のツールの名前**:
  - `glossary_search_glossary`[cite: 198, 203]

- **STEP 2 の `[tool call]`**:
  - `[tool call]   glossary_search_glossary({'query': '3レベル'})`[cite: 199, 203]

- **STEP 3 の `[tool call]`**:
  - `[tool call]   fetch_fetch({'url': 'https://modelcontextprotocol.io/'})`[cite: 200, 203]
  - `[tool call]   glossary_search_glossary({'query': 'Model Context Protocol'})`（要約後に講座との関連回を示すために追加で呼び出された）

- **STEP 4 の `[tool call]`**:
  - `[tool call]   aws_aws___search_documentation({'search_phrase': 'Amazon S3 バージョニング 概要', 'topics': ['general']})`[cite: 201, 203]
  - `[tool call]   aws_aws___search_documentation({'search_phrase': 'Versioning what is Amazon S3 versioning', 'topics': ['general']})`[cite: 201, 203]

---

## 宿題

- **取り組んだレベル**: 基礎[cite: 182, 205]

- **やったこと**:
  - サーバー側の `app/mcp_server.py` の `# TODO(宿題・基礎)` の下に `server.tool()(tools.get_lecture_summary)` の1行のみを書き足して保存した（エージェント側の `app/mcp_agent.py` および `app/mcp_servers.json` は一切変更していない）[cite: 183, 206]。
  - `git status` で変更ファイルを確認したうえで、`--list-tools` によるツール一覧の増加確認、`--trace` 付きでの質問実行（`"第2回では何を学びますか?"`）、および `uv run pytest -q` によるテスト実行を行った[cite: 183, 206]。

- **結果**:
  - **`--list-tools` の変化**: ツール一覧が7個から **8個** に増え、新しく `[glossary_get_lecture_summary]`（引数: `{"lecture": {"title": "Lecture", "type": "integer"}}`）が自動で認識された[cite: 183, 206]。
  - **`--trace` の出力（`"第2回では何を学びますか?"`）**:
    - 呼び出し: `[tool call]   glossary_get_lecture_summary({'lecture': 2})`
    - 返却値: `[tool return] glossary_get_lecture_summary -> # 第2回 ファインチューニングとLoRA ...`
    - Usage: `入力 5,942 tokens / 出力 225 tokens / モデル応答 2 回`
    - 回答: 公開されたツールから第2回の要点を取得し、「ファインチューニングとLoRA」の主な内容と関係する回（第2回）を正確に回答した。
  - **テスト結果**: `uv run pytest -q` を実行し、`21 passed in 4.36s` ですべてのテストが正常に通過した[cite: 183, 206]。

- **気づき・疑問**:
  - エージェント本体（`app/mcp_agent.py`）や接続設定（`app/mcp_servers.json`）を一切書き換えなくても、MCPサーバー側（`app/mcp_server.py`）でツールを追加公開するだけで、エージェント起動時のツール一覧問い合わせを通じて即座に新しいツールがモデルへ渡り利用可能になる（疎結合な拡張性）ことを実感した[cite: 183, 193, 194, 206]。
  - また、STEP 3 の要約質問では `fetch_fetch` で外部ページを取得したあとに、エージェントが自律的に `glossary_search_glossary` も呼び出して「関連する回：第3回」を補完しており、ローカルの自作MCPサーバーと公開・外部のMCPサーバーを同じ仕組みでシームレスに組み合わせて使える点が非常に強力だと感じた[cite: 195, 200]。
