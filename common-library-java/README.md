# Common Library for Java (Spring Boot)

> **"The technical core foundation for Java-based microservices."**  
> 本リポジトリは、Java/Spring Boot を採用する全サービスに対して、共通の技術基盤を提供するライブラリである。`standards/common-library` で定義された設計哲学の実装体（WHAT）に相当する。

---

## 📌 目的
応用情報技術者試験（AP）の「モジュール設計」に基づき、13のリポジトリで重複しがちな「横断的関心事（セキュリティ、ロギング、エラー処理）」を本ライブラリに集約する。これにより、各リポジトリの「機能的強度」を高め、保守コストを最小化することを目的とする。

## 📦 提供機能

### 1. Unified API Response (`com.example.common.api`)
- **`ApiResponse<T>`**: 成功/失敗を問わず、全てのAPI応答をラップする標準コンテナ。
- **`PagingMetadata`**: ページネーション情報の統一フォーマット。

### 2. Standardized Exception Handling (`com.example.common.exception`)
- **`BaseException`**: 実行時例外の基底クラス。
- **`GlobalExceptionHandler`**: `@RestControllerAdvice` を用いた、例外から `ApiResponse` への自動マッピング。
- **`CommonErrorCode`**: 400系、500系の標準エラーコードの定義。

### 3. Observability & Logging (`com.example.common.logging`)
- **`TraceIdInterceptor`**: 全リクエストに対する `X-Trace-Id` の自動採番とログ出力。
- **`LoggingAspect`**: AOPによる、メソッド実行時間および入出力パラメータの自動記録。

### 4. Security Context (`com.example.common.security`)
- **`JwtAuthenticationFilter`**: JWTの共通検証ロジック。
- **`UserContextHolder`**: スレッドローカルを用いた、ログインユーザー情報へのセキュアなアクセス。

## 🛠️ セットアップ

### 1. 依存関係の追加 (pom.xml)
各プロジェクトの `pom.xml` に以下の依存関係を追加します。
```xml
<dependency>
    <groupId>com.example</groupId>
    <artifactId>common-library-java</artifactId>
    <version>1.0.0-SNAPSHOT</version>
</dependency>
```
### 2. 共通機能の有効化
メインクラス、または設定クラスに `@ComponentScan` を追加し、共通ライブラリをスキャン対象に含めてください。
```java
@SpringBootApplication
@ComponentScan(basePackages = {"com.example.common", "com.example.yourproject"})
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```
## 📐 実装規約
AP試験の「保守プロセス」に基づき、本ライブラリの利用者は以下のルールを守ること。

1. **二重実装の禁止**: 本ライブラリで提供されている例外処理やログ出力を、各サービスのリポジトリで独自に再実装してはならない。
2. **依存性の管理**: 本ライブラリを直接書き換える際は、セマンティックバージョニングに従い、他12のリポジトリへの影響度を慎重に評価（影響調査）すること。
3. **テストコードの同梱**: 共通基盤の不具合は全サービスに波及するため、新規機能追加時は命令網羅（C0）を満たす単体テストを必須とする。

---
© 2026 Standard Architecture Blueprint. Managed by System Architect.
