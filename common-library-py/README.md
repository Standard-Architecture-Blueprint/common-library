# Common Library for Python (FastAPI / ML)

> **"Ensuring data integrity and observability for Python services."**  
> 本リポジトリは、Python (FastAPI等) を採用する全サービス、および機械学習コンポーネントに対して、共通の基盤機能を提供するライブラリである。

---

## 📌 目的
応用情報技術者試験（AP）の「ソフトウェア品質」および「アルゴリズム」の原則に基づき、APIの入出力、エラーハンドリング、ログフォーマットを多言語（Java/TS）と統一する。これにより、マイクロサービス全体におけるデータの「正確性」と「追跡可能性」を担保する。

## 📦 提供機能

### 1. API Response Schema (`api/`)
- **`CommonResponse[T]`**: PydanticのGenericsを用いた、Java/TSと共通のレスポンスモデル。
- **`ErrorSchema`**: 異常系におけるエラー詳細の構造定義。

### 2. Standardized Logging (`logging/`)
- **`JsonFormatter`**: ログをJSON形式で出力し、クラウド環境（CloudWatch/ELK）での分析を容易にする。
- **`CorrelationIdMiddleware`**: HTTPリクエストから `X-Trace-Id` を抽出し、全ログに付与する。

### 3. ML Model Interface (`utils/`)
- **`BaseModelWrapper`**: 推論結果の出力フォーマットをAPI規格に適合させるためのユーティリティ。

## 🛠️ セットアップ

### 1. パッケージのインストール
```bash
pip install common-library-py
```

### 2. FastAPI への適用
```python
from common_lib.exception import add_common_exception_handlers
from fastapi import FastAPI

app = FastAPI()
add_common_exception_handlers(app)
```

## 📐 実装規約
AP試験の「保守プロセス」に基づき、本ライブラリの利用者は以下のルールを守ること。

1. Pydanticによる型定義: 
   レスポンスは辞書型(dict)ではなく、必ず本ライブラリの Pydantic モデルを使用して型安全性を確保すること。
2. 例外の正規化: 
   HTTPException を直接 raise せず、本ライブラリで定義された共通例外クラスを使用し、レスポンス形式の崩れを防ぐこと。
3. パフォーマンスへの配慮: 
   ライブラリ内の処理（特にログやバリデーション）は、推論処理等のボトルネックにならないよう、軽量かつ効率的な実装を維持すること。

---
© 2026 Standard Architecture Blueprint. Managed by System Architect.
