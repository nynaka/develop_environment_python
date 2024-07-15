[mypy](https://pypi.org/project/mypy/)
===

MyPy は、Python プログラミング言語用の静的型チェッカーで、Python コードに型アノテーションが付与されているかチェックしてくれます。  
Python は動的型付けの言語として知られていますが、コードに型アノテーションを追加し、静的な型チェックを行うことで、コードのエラーを早期に発見し、コードの可読性と保守性を向上させることができます。

[mypy documentation](https://mypy.readthedocs.io/en/stable/)

## インストール

```bash
pip install mypy
```

## 実行例

- サンプルコード

    ```python
    def add(x1, x2):
        return x1 + x2

    if __name__ == "__main__":
        print(add(2,3))
    ```

- 実行例

    ```bash
    mypy --strict mypy_sample.py

    mypy_sample.py:2: error: Function is missing a type annotation  [no-untyped-def]
    mypy_sample.py:6: error: Call to untyped function "add" in typed context  [no-untyped-call]
    Found 2 errors in 1 file (checked 1 source file)
    ```

    - `--strict` オプションを付けないとチェックエラーはなくなります。
    - 型アノテーションのチェックはしてくれるのですが、自動修正はしてくれません。

- 修正例

    下記のように add 関数の引数と戻り値に型アノテーションを追加するとエラーが解消します。

    ```python
    def add(x1: int, x2: int) -> int:
        return x1 + x2

    if __name__ == "__main__":
        print(add(2, 3))
    ```


