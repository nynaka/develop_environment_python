[bandit](https://bandit.readthedocs.io/en/latest/)
===

```{note}
**bandit** は Python コードのセキュリティチェッカーです。
```

## インストール

```bash
pip install bandit
```

## 使い方

- 単一ファイル

    ```bash
    bandit 対象ファイル
    ```

- ディレクトリ単位 (サブディレクトリ含む)

    ```bash
    bandit -r 対象デレクとり
    ```

## セキュリティエラーの例

- サンプルコード

    ```python
    import sys
    eval(sys.args[1])
    ```

- チェック結果

    全体で何件問題があったのか？と問題点の内容、重要度、詳細解説サイトへのリンクが表示されます。

    ```text
    $ bandit test.py
    [main]  INFO    profile include tests: None
    [main]  INFO    profile exclude tests: None
    [main]  INFO    cli include tests: None
    [main]  INFO    cli exclude tests: None
    [main]  INFO    running on Python 3.11.2
    Run started:2024-06-27 13:09:58.548291
    
    Test results:
    >> Issue: [B307:blacklist] Use of possibly insecure function - consider using safer ast.literal_eval.
       Severity: Medium   Confidence: High
       CWE: CWE-78 (https://cwe.mitre.org/data/definitions/78.html)
       More Info: https://bandit.readthedocs.io/en/1.7.9/blacklists/blacklist_calls.html#b307-eval
       Location: ./test.py:4:0
    3
    4       eval(sys.args[1])
    
    --------------------------------------------------
    
    Code scanned:
            Total lines of code: 2
            Total lines skipped (#nosec): 0
    
    Run metrics:
            Total issues (by severity):
                    Undefined: 0
                    Low: 0
                    Medium: 1
                    High: 0
            Total issues (by confidence):
                    Undefined: 0
                    Low: 0
                    Medium: 0
                    High: 1
    Files skipped (0):
    ```
