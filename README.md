# Common Library (Standard Implementation Base)

> **"Infrastructure for ensuring system-wide consistency and reliability."**  
> 本リポジトリは、複数のマイクロサービス群すべてにおいて共有される、技術的基盤（横断的関心事）を管理する共通ライブラリである。

---

## 📌 目的
応用情報技術者試験（AP）における「ソフトウェア再利用」の原則に基づき、共通ロジックを本ライブラリに集約することで、開発効率の向上とシステム全体の品質均一化（標準化）を実現する。

## 🏛️ 共通化の対象（Scope）
AP試験の「モジュール設計」における「機能的強度」を高めるため、業務ロジック（ドメイン知識）を含まない、以下の技術基盤のみを対象とする。

### 1. 共通例外ハンドリング (Exception Management)
- **BaseException**: 全サービスで継承すべき基底例外クラス。
- **Global Error Handler**: エラーレスポンスのJSON構造をシステム全体で統一。
- **Standard Error Codes**: 認証エラー、バリデーションエラー等の共通コード定義。

### 2. 統一ロギング (Structured Logging)
- **Trace-ID Propagation**: 分散環境下でリクエストを追跡するための識別子制御。
- **Privacy Masking**: 個人情報や機密情報のログ露出を自動防止するフィルター。
- **Audit Logs**: セキュリティ監査に必要なログフォーマットの標準化。

### 3. 通信規約・DTOボイラープレート (API Standardization)
- **Generic Response**: `ApiResponse<T>` 等によるレスポンス形式の固定。
- **Pagination Model**: ページネーション情報の共通フォーマット。
- **Common Validators**: Java Bean Validation を拡張した共通バリデーションロジック。

## 📂 ディレクトリ構造
```text
src/main/java/com/example/common/
├── api/              # 通信規約（Response, Paging）
├── auth/             # セキュリティ基盤（JWT Parsing, Context）
├── exception/        # 例外基底クラス、標準例外型
├── logging/          # 構造化ログ、AOPインターセプタ
└── util/             # 言語機能を補完する汎用ツール（Date, JSON）
```
## 🛠️ 技術スタック
- **Java 17 / Spring Boot 3.x**: フレームワークの基盤統一。
- **Lombok**: 共通DTOのボイラープレートコード削減。
- **Jackson**: データシリアライズ・デシリアライズの共通設定。

## 📐 実装・運用規約
AP試験の「変更管理」および「保守性」の観点から、以下のルールを厳守する。

1. **下位互換性の維持 (Backward Compatibility)**: 
   - ライブラリの変更は13リポジトリすべてに影響するため、破壊的変更は原則禁止。
2. **疎結合の維持**: 
   - 特定のサービス（業務ドメイン）に依存するクラスを含めない。
3. **高強度・低結合**: 
   - 共通部品としての「機能的強度」を追求し、利用側（各サービス）との結合度を最小限に抑える。

## 🧪 品質管理
- **命令網羅（C0）100%**: 基盤部品の不具合は致命的な波及効果（ドミノ倒し）を招くため、全パスのテストを必須とする。

---
© 2026 Standard Architecture Blueprint.
