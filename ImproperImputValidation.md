# 入力検証不備 (Improper Input Validation)

### 1. バッファオーバーフロー (Buffer Overflow)

- 入力データ
  - 割り当てられたバッファサイズを超える
- メモリ破壊
- 任意のコード実行

### 2. フォーマット文字列攻撃 (Format String Attack)

- 入力データ
  - フォーマット文字列
- 意図しない情報漏洩
- コード実行

### 3. SQLインジェクション (SQL Injection)

- 入力データ
  - SQLクエリ
- データベースへの不正アクセス
- データ改ざん

### 4. クロスサイトスクリプティング (Cross-Site Scripting; XSS)

- 入力データ
  - スクリプト
- 他のユーザーのブラウザ上で実行
- Cookieの窃取
- セッションハイジャック

### 5. コマンドインジェクション (Command Injection)

- 入力データ
  - OSコマンドとして解釈
- サーバー上で任意のコマンドが実行

### 6. パス・トラバーサル (Path Traversal)

- 入力データ
  - ディレクトリパスを含む
- 本来アクセスできないファイルにアクセス

### 7. HTTPヘッダインジェクション (HTTP Header Injection)

- 入力データ
  - HTTPヘッダに挿入
- レスポンスの改ざん
- セッション固定化攻撃

### 8. XML外部エンティティ (XML External Entity; XXE)

- XML入力データ
  - 外部エンティティ
- ファイルアクセス
- サービス拒否攻撃

### 9. デシリアライゼーションの脆弱性 (Deserialization Vulnerability)

- 信頼できないデータ
    - デシリアライズ
- 任意のコード実行

### 10. 正規表現DoS (Regular Expression Denial of Service; ReDoS)

- 複雑な正規表現
- 過剰なCPUリソース消費
- サービス拒否攻撃

### 11. 不適切な文字エンコーディング処理 (Improper Character Encoding Handling)

- 入力データ・内部データ
  - 不適切な文字エンコーディング
- 脆弱性
  - 文字化け
  - 情報漏洩
  - サービス拒否
  - クロスサイトスクリプティング
  - UTF-8 BOM

#### 1. 文字コードの不一致 (Encoding Mismatch)

- データを受け取る側と送信側で想定している文字コードが異なる
- 文字化け
- データ破損

- 例
  - 送信側
    - UTF-8として送信
  - 受信側
    - Shift-JISとして解釈

#### 2. エンコーディングの欠落 (Missing Encoding Information)

*   データに文字コードの情報が付与されておらず、受信側がどのように解釈すべきか判断できない。

- 例
  - HTTPレスポンスのContent-Typeヘッダ
    - charsetが指定されていない

#### 3. 不正な文字コードの使用 (Use of Invalid Encoding)

- 概要
  - サポートされていない文字コード
  - 不正な形式の文字コード

#### 4. バイト順マーク (BOM) の不適切な処理 (Improper BOM Handling)

- BOM (Byte Order Mark) は
  - 文字コードを示すための数バイトのデータ
  - 特にUTF-8では必須ではない。
  - BOMの有無によって挙動が変わる可能性
    - BOMの有無を正しく区別しない。
    - BOMをデータの一部として誤って処理する。
    - UTF-8以外でBOMを期待する。
- 例
  - **BOMの有無で処理が変わる:**
    - BOM付きのJSONとBOMなしのJSONで異なる挙動を示すJSONパーサ。
  - **BOMをデータとして扱う:**
    - BOMをJSONデータの一部として誤って解釈し、構文エラーを引き起こす。
  - **BOMを削除しない:**
    - 受信したデータからBOMを削除せずに後続の処理に渡す
      - 意図しない挙動

#### 5. マルチバイト文字の処理不備 (Improper Handling of Multibyte Characters)

- UTF-8などのマルチバイト文字コード
  - 文字の途中でデータが途切れる
  - 不正なバイトシーケンス処理
- サロゲートペアの処理不備

#### 6. エンコーディング変換時の問題 (Problems during Encoding Conversion)

- エンコーディング変換時の問題
  - 情報の欠落
  - 不正な文字精製
- 例
  - UTF-8からASCIIに変換
    - ASCIIで表現できない文字が欠落
  - Base64にデコード
    - base64アルファベット（A-Z, a-z, 0-9, +, /, =）以外の文字

#### 7. 文字エンコーディングを利用した攻撃 (Encoding-Based Attacks)

- 特定の文字エンコーディングの性質を利用した攻撃。
- 例
  - オーバーフロー
  - アンダーフロー

#### 8. 正規化の欠如 (Lack of Normalization)

- 同じ文字を複数の表現方法で表せるUnicodeにおいて、正規化が行われていないために、予期せぬ挙動が発生する。
- 例
  - 全角と半角の区別
  - 結合文字と結合されていない文字の区別

### 12. 不適切な数値処理 (Improper Numeric Handling)

- 入力データ
  - 不適切な数値の範囲チェックや型変換
- オーバーフロー
- アンダーフロー