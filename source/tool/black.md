[black](https://pypi.org/project/black/)
===

```{note}
**Black** は Python のコードフォーマットツールです。  
スペースや空行の数、1 行あたりの文字数をいい感じに調整してくれます。
```

## インストール

```bash
pip install black
```

## 実行例

- 使用前のソース

    ```python
    x1 = 10
    x2 = 20
    print(x1,        x2)

    array1 = [1,2,3,         4, 5,  6]






    print(array1)
    ```

- black コマンド実行

    ```bash
    black 解析対象ファイル
    ```

- 使用後のソース

    ```python
    x1 = 10
    x2 = 20
    print(x1, x2)

    array1 = [1, 2, 3, 4, 5, 6]


    print(array1)
    ```

    - カンマの後ろには半角スペースを 1 個置くのがいいらしい。
    - 空行は 2 行以上の場合は、2 行に整形されるようです。