[isort](https://pypi.org/project/isort/)
===

`isort` は、Python コードのインポート文の整理ツールです。  
Pythonコードにおけるインポート文の順序やスタイルの統一を行います。

## インストール

```bash
pip install isort
```

## 使い方

```bash
isort isort_sample.py
```

- 使用前

    こんな感じの import 文を書いていたとします。

    ```python
    import osv
    import datetime

    from cryptography.hazmat.primitives.asymmetric import rsa
    from cryptography.hazmat.primitives import serialization
    ```

- 使用後

    isort を使うと下記のような形に整形してくれます。  

    - import でアルファベット昇順
    - その後ろに from でアルファベット昇順

    のような並び替えのように見えました。

    ```python
    import datetime
    import os

    from cryptography.hazmat.primitives import serialization
    from cryptography.hazmat.primitives.asymmetric import rsa
    ```
