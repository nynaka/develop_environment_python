[os](https://docs.python.org/ja/3.14/library/os.html)
===

## サンプルコード

- カレントディレクトリの取得

    ```python
    import os
    pwd = os.getcwd()
    print(pwd)
    ```

- パスが存在するか確認する

    ```python
    import os
    path = "/dev/zero"
    os.path.exists(path)
    ```

- パスがディレクトリ／ファイルかをチェックする

    ```python
    import os
    path = "/dev/zero"
    os.path.isdir(path)
    os.path.isfile(path)
    ```

- ディレクトリ名とファイル名を取り出す

    ```python
    import os
    path = "/dev/zero"
    os.path.dirname(path)
    os.path.basename(path)
    ```

- ファイル拡張子の取得

    ```python
    import os
    path = "/tmp/test.txt"
    name, ext = os.path.splitext(
        os.path.basename(path)
    )
    print(name, ext)
    ```


## 参考サイト

- [【Python】OSモジュールについてまとめました](https://zenn.dev/robes/articles/ae1f1dbcb0c5aa)