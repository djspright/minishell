# module_diagram

```mermaid
graph TB
    subgraph "main"
        M1[main.c]
        M2[readline<br/>コマンドライン読み込み]
        M3[process_line<br/>入力処理]
        M4[signal_handler<br/>シグナル処理]
    end

    subgraph "parser"
        P1[lexer.c<br/>字句解析]
        P2[parser.c<br/>構文解析]
        P3[expand.c<br/>変数展開]
        P4[tokenizer.c<br/>トークン化]
    end

    subgraph "execute"
        E1[executor.c<br/>実行制御]
        E2[pipeline.c<br/>パイプライン処理]
        E3[redirect.c<br/>リダイレクト処理]
        E4[builtin.c<br/>ビルトイン実行]
        E5[external.c<br/>外部コマンド実行]
    end

    subgraph "builtin"
        B1[echo.c]
        B2[cd.c]
        B3[pwd.c]
        B4[export.c]
        B5[unset.c]
        B6[env.c]
        B7[exit.c]
    end

    subgraph "utils"
        U1[env_utils.c<br/>環境変数管理]
        U2[error.c<br/>エラー処理]
        U3[memory.c<br/>メモリ管理]
        U4[string_utils.c<br/>文字列処理]
    end

    M1 --> M2
    M2 --> M3
    M3 --> P1
    P1 --> P4
    P4 --> P2
    P2 --> P3
    P3 --> E1
    E1 --> E2
    E1 --> E3
    E2 --> E4
    E2 --> E5
    E4 --> B1
    E4 --> B2
    E4 --> B3
    E4 --> B4
    E4 --> B5
    E4 --> B6
    E4 --> B7
```

## 階層型ディレクトリ構造

```
minishell/
│
├── Makefile
├── includes/
│   ├── minishell.h
│   ├── parser.h
│   ├── executor.h
│   └── builtin.h
│
├── srcs/
│   ├── main/
│   │   ├── main.c
│   │   ├── signal.c
│   │   └── prompt.c
│   │
│   ├── parser/
│   │   ├── lexer.c
│   │   ├── parser.c
│   │   ├── tokenizer.c
│   │   ├── expand.c
│   │   └── quotes.c
│   │
│   ├── executor/
│   │   ├── executor.c
│   │   ├── pipeline.c
│   │   ├── redirect.c
│   │   ├── process.c
│   │   └── path.c
│   │
│   ├── builtin/
│   │   ├── echo.c
│   │   ├── cd.c
│   │   ├── pwd.c
│   │   ├── export.c
│   │   ├── unset.c
│   │   ├── env.c
│   │   └── exit.c
│   │
│   └── utils/
│       ├── env_utils.c
│       ├── error.c
│       ├── memory.c
│       ├── string_utils.c
│       └── list_utils.c
│
└── libft/
    └── [libft files]
```

## 主要データフロー図
```mermaid
sequenceDiagram
    participant User
    participant Main
    participant Parser
    participant Executor
    participant Process

    User->>Main: コマンド入力
    Main->>Parser: readline()
    Parser->>Parser: lexer()
    Parser->>Parser: tokenize()
    Parser->>Parser: expand_variables()
    Parser->>Executor: AST(抽象構文木)

    alt ビルトインコマンド
        Executor->>Executor: execute_builtin()
    else 外部コマンド
        Executor->>Process: fork()
        Process->>Process: execve()
        Process-->>Executor: 終了ステータス
    end

    Executor-->>Main: 実行結果
    Main-->>User: プロンプト表示
```
