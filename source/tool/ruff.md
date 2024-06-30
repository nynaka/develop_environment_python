[ruff](https://pypi.org/project/ruff/)
===

```{note}
**ruff** は Python の静的コード解析ツールおよびコードフォーマットツールです。  
flake8 と black を合わせたようなツール、というとイメージつくかもしれないです。
```

## インストール

```bash
pip install ruff
```

## 使い方

- formatter

    ```bash
    ruff format 対象ファイル
    ```

    対象ファイルの書式 (一行あたりの文字数やスペース等) を自動整形してくれます。  
    `black` のような機能です。

- linter

    ```bash
    ruff check --output-format=github 対象ファイル 
    ```

    対象ファイルが Python コードとして問題点がないかチェックしてくれます。  
    `Flake8` のような機能です。
