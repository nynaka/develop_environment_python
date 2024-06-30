[flake8](https://flake8.pycqa.org/en/latest/)
===

```{note}
**flake8** は Python の静的コード解析ツールで、解析対象のソースコードが PEP8 に違反しているコードがないかをチェックしてくれます。
```

## インストール

```bash
pip install flake8
```

## 使い方

- 一般的な実行方法

    ```bash
    flake8 解析対象ファイル
    ```

- 特定のエラーを無視する場合の実行方法

    ```bash
    flake8 解析対象ファイル --ignore E402
    ```

## 参照サイト

- [flake8の使い方とオプション](https://qiita.com/raku_taro/items/93dacae018fb192d05b5)
