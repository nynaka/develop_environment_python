[pylint](https://pypi.org/project/pylint/)
===

Pylint は、PEP 8 に準拠したコードスタイルガイドラインに基づいてコーディングされているかをチェックするツールです。

## インストール

```bash
pip install pylint
```

## 使い方

- 素の実行

    ```bash
    pylint example.py
    ```

- 一部の指摘を無視する

    ```bash
    pylint --disable=C0301,C0305 aaa.py
    ```

    - C0301: Line too long (110/100) (line-too-long)

        あまり 1 行が長いと可読性にかかわりますが、1 行 100 文字制限は短い気がします。。。

    - C0305: Trailing newlines (trailing-newlines)

        ファイルの末尾に空行を入れることは推奨されていないようです。
