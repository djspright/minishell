# Minishell
*As beautiful as a shell*

[![C](https://img.shields.io/badge/C-00599C?style=flat&logo=c&logoColor=white)](https://en.wikipedia.org/wiki/C_(programming_language))
[![42 School](https://img.shields.io/badge/42-000000?style=flat&logo=42&logoColor=white)](https://42.fr/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## プロジェクト概要

**Minishell**は、42 Schoolのカリキュラムで開発したBashライクなシェル実装です。システムプログラミングの深い理解とUnix/Linuxの基本概念を実証するプロジェクトです。

### 技術的ハイライト

- **低レベルシステムプログラミング**: C言語でのシステムコール直接操作
- **プロセス管理**: fork, exec, waitによる子プロセス制御
- **ファイルディスクリプタ操作**: pipe, dup2によるI/Oリダイレクション
- **シグナルハンドリング**: 適切な割り込み処理とプロセス制御
- **字句・構文解析**: カスタムパーサーによるコマンドライン解析
- **メモリ管理**: 動的メモリ割り当てとリーク防止

## 主要機能

### コア機能
- **インタラクティブプロンプト**: コマンド入力とインタラクティブな操作
- **コマンド履歴**: readline統合による履歴管理
- **環境変数展開**: `$VAR` および `$?` 対応
- **クォート処理**: シングル・ダブルクォートの適切な解釈

### I/O制御
- **パイプライン**: 複数コマンドの連鎖実行 (`cmd1 | cmd2 | cmd3`)
- **入出力リダイレクション**: 
  - `<` 入力リダイレクト
  - `>` 出力リダイレクト
  - `>>` 追記リダイレクト
  - `<<` ヒアドキュメント

### シグナル制御
- **Ctrl+C**: プロセス中断とプロンプト復帰
- **Ctrl+D**: 適切なシェル終了
- **Ctrl+\\**: シグナル無視

### ビルトインコマンド
```bash
echo [-n]      # テキスト出力
cd [path]      # ディレクトリ変更
pwd            # 現在位置表示
export [VAR=val] # 環境変数設定
unset [VAR]    # 環境変数削除
env            # 環境変数一覧
exit [code]    # シェル終了
```

### ボーナス機能
- **論理演算子**: `&&` (AND), `||` (OR), `()` (優先度制御)
- **ワイルドカード**: `*` グロブパターン

## 技術仕様

### アーキテクチャ
```
字句解析 → 構文解析 → 展開処理 → 実行エンジン
    ↓         ↓         ↓         ↓
  Token    AST      環境変数    プロセス制御
                    展開       I/O制御
```

### 使用技術
- **言語**: C (C99標準)
- **外部ライブラリ**: GNU Readline
- **システムコール**: 
  - プロセス: `fork`, `execve`, `wait`, `waitpid`
  - ファイル: `open`, `read`, `write`, `close`, `dup2`, `pipe`
  - シグナル: `signal`, `sigaction`, `kill`
  - ファイルシステム: `stat`, `access`, `getcwd`, `chdir`

### 品質保証
- **メモリ管理**: 完全なメモリリーク対策
- **エラーハンドリング**: 堅牢なエラー処理
- **コーディング規約**: 42 Norm準拠
- **テスト**: 包括的なテストケース

## ビルド・実行

### 要件
- **OS**: Linux / macOS
- **コンパイラ**: GCC / Clang
- **依存関係**: readline library

### インストール
```bash
# リポジトリクローン
git clone https://github.com/username/minishell.git
cd minishell

# ビルド
make

# 実行
./minishell
```

### 基本操作
```bash
# シンプルコマンド
minishell$ ls -la
minishell$ echo "Hello, World!"

# パイプライン
minishell$ cat file.txt | grep pattern | wc -l

# リダイレクション
minishell$ echo "content" > output.txt
minishell$ cat < input.txt

# 環境変数
minishell$ export VAR=value
minishell$ echo $VAR

# 終了
minishell$ exit
```

## テスト例

### 基本機能テスト
```bash
# コマンド実行
./minishell
minishell$ /bin/ls
minishell$ echo hello world

# パイプとリダイレクション
minishell$ cat /etc/passwd | head -5 | tail -2
minishell$ echo "test" > file.txt && cat file.txt

# 環境変数とクォート
minishell$ export USER=test
minishell$ echo "Hello $USER"    # Hello test
minishell$ echo 'Hello $USER'    # Hello $USER

# 論理演算子（ボーナス）
minishell$ echo hello && echo world
minishell$ false || echo "fallback"
```

### エラーハンドリング
```bash
# 存在しないコマンド
minishell$ nonexistent_command
bash: nonexistent_command: command not found

# ファイルアクセスエラー
minishell$ cat nonexistent_file
cat: nonexistent_file: No such file or directory
```

## プロジェクト構成

```
minishell/
├── Makefile                 # ビルド設定
├── includes/
│   └── minishell.h         # メインヘッダー
├── src/
│   ├── main.c              # エントリーポイント
│   ├── parser/             # 解析エンジン
│   │   ├── lexer.c         # 字句解析
│   │   ├── parser.c        # 構文解析
│   │   └── expander.c      # 変数展開
│   ├── executor/           # 実行エンジン
│   │   ├── executor.c      # コマンド実行
│   │   ├── pipes.c         # パイプ処理
│   │   └── redirect.c      # リダイレクト
│   ├── builtins/          # ビルトインコマンド
│   │   ├── echo.c
│   │   ├── cd.c
│   │   ├── pwd.c
│   │   ├── export.c
│   │   ├── unset.c
│   │   ├── env.c
│   │   └── exit.c
│   ├── signals/           # シグナル処理
│   │   └── signals.c
│   └── utils/             # ユーティリティ
│       ├── errors.c       # エラー処理
│       ├── memory.c       # メモリ管理
│       └── strings.c      # 文字列操作
└── tests/                 # テストファイル
    └── test_scripts.sh
```

## 学習成果・スキル習得

### システムプログラミング
- **プロセス制御**: fork/exec モデルの深い理解
- **IPC**: パイプによるプロセス間通信
- **シグナル処理**: 非同期イベント処理
- **ファイルシステム**: POSIX ファイル操作API

### ソフトウェア設計
- **モジュラー設計**: 機能別コンポーネント分割
- **エラーハンドリング**: 例外安全なプログラム設計
- **メモリ管理**: 手動メモリ管理とリーク対策
- **パーサー設計**: 字句・構文解析の実装

### 開発プロセス
- **厳格なコーディング規約**: 42 Norm準拠
- **テスト駆動**: 包括的なテストケース作成
- **デバッグ**: gdb, valgrindを使用した品質保証
- **ドキュメンテーション**: 仕様書理解と実装

## 技術的詳細

### パーサー実装
コマンドライン入力を以下の段階で処理：
1. **字句解析**: 入力文字列をトークンに分割
2. **構文解析**: トークンから抽象構文木(AST)構築
3. **意味解析**: 環境変数展開、クォート処理
4. **実行**: プロセス生成とI/O制御

### メモリ管理戦略
- **RAII パターン**: リソース確保と解放の対応
- **エラー時の適切なクリーンアップ**
- **valgrind による検証**: メモリリーク完全排除

### シグナル処理設計
- **シグナル安全**: 適切なシグナルハンドラ実装
- **状態管理**: グローバル変数の最小化
- **ユーザビリティ**: 直感的なCtrl+C/D/\\処理

## 今後の拡張予定
- [ ] より高度なワイルドカード (`?`, `[...]`)
- [ ] コマンド補完機能
- [ ] 設定ファイル対応
- [ ] 追加ビルトインコマンド
- [ ] スクリプト実行モード

---

> **Note**: このプロジェクトは42 Schoolの教育プログラムの一環として開発されました。システムプログラミング、Unix/Linux、Cプログラミングの実践的なスキルを習得しています。

**連絡先**: [gdlnolgs@gmail.com](mailto:gdlnolgs@gmail.com)
