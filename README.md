# Common Library Policy & Standards

> **"A unified foundation for cross-cutting concerns across all services."**  
> 本リポジトリは、特定の言語に依存せず、13の全サービスが共通して備えるべき「横断的関心事」の実装規約と、各言語版ライブラリへのポインタを管理する。

---

## 📌 目的
応用情報技術者試験（AP）における「ソフトウェア再利用」および「ITガバナンス」の原則に基づき、プラットフォーム間の差異を吸収し、システム全体で一貫した振る舞い（エラー応答、ログ、セキュリティ）を保証する。

## 🏛️ 共通化の原則（Architectural Principles）
どの言語で共通ライブラリを実装する場合も、以下の3点を遵守すること。

### 1. 共通例外・応答プロトコル (Unified Response)
- **統一されたエラー構造**: 言語を問わず、APIは必ず `code`, `message`, `details` を持つ共通のJSON構造を返す。
- **例外の分類**: 業務例外（Business Exception）とシステム例外（System Exception）の区別を全言語で共通化する。

### 2. 観測可能性の標準化 (Observability Standard)
- **分散トレーシング**: HTTPヘッダー経由で `X-Trace-Id` を伝搬させ、全言語のログでこれを記録する。
- **構造化ロギング**: 分析を容易にするため、ログは原則としてJSON形式で出力し、タイムスタンプ精度や重要度（Severity）の定義を統一する。

### 3. 認証・認可の共通処理 (Security Baseline)
- **トークン検証**: JWT（JSON Web Token）のパース、署名検証、有効期限チェックのロジック。
- **コンテキスト管理**: 認証済みのユーザー情報をスレッドや非同期コンテキスト内で保持・参照する仕組み。

## 📂 各言語の実装リポジトリ
設計思想は本リポジトリで一元管理し、実際の実装は以下の言語別リポジトリで行う。

- **[common-library-java](../../template-java-spring)**: Spring Boot用。Jar形式で提供。
- **[common-library-ts](../../template-angular-spa)**: Angular/TypeScript用。npm形式で提供。
- **[common-library-py](../../template-python-ml)**: Python/FastAPI用。Wheel形式で提供。

## 📐 実装規約 (Engineering Standards)
AP試験の「保守性」を担保するため、各言語ライブラリは以下の「結合度」と「強度」の基準を満たすこと。

1. **ドメイン知識の排除**: 
   - ライブラリ内に特定の業務ロジック（例：在庫計算、注文処理）を記述してはならない。
2. **依存性の最小化**: 
   - ライブラリ自体が外部の重量級フレームワークに過度に依存し、利用側の自由度を奪わないこと。
3. **バージョニング戦略**: 
   - セマンティックバージョニング（SemVer）を適用し、APIの互換性を厳格に管理する。

## ⚖️ 準拠性 (Compliance)
すべてのリポジトリは、自身の言語に対応する `common-library` を必ずインポートし、基盤機能を自前で再実装（車輪の再発明）することを禁止する。

---
© 2026 Standard Architecture Blueprint. Managed by System Architect.
