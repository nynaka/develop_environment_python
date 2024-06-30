sphinx.ext.autodoc
===

## ディレクトリ構成

- `app` ディレクトリに Python アプリのコードを格納する。
- `app` ディレクトリと同階層に `doc` ディレクトリを配置する。

```text
(Current)
 |-- app
 | |-- calc.py
 |-- apidoc
 | |-- Makefile
 | |-- source
 | | |-- _static
 | | |-- _templates
 | | |-- conf.py
 | | |-- index.rst
 |-- requirements.txt
```

## ソースコードの編集

ソースコードの編集といってもクラス、メソッドにコメントを追加しただけです。

```diff
--- /tmp/calc.py        2024-06-27 21:26:50.103927549 +0900
+++ calc.py     2024-06-27 21:27:31.071925920 +0900
@@ -1,8 +1,23 @@
 class Calc:
-
+    """四則演算クラス (予定)
+    """
     def __init__(self) -> None:
+        """constructor
+
+        Args: None
+        Returns: None
+        """
         pass

     def add(self, x1:int, x2:int) -> int:
+        """加算処理
+
+        Args:
+            x1 (int): 引数1
+            x2 (int): 引数2
+
+        Returns:
+            int: 加算結果
+        """
         return x1 + x2
```

- requirements.txt

    ```text
    sphinx
    sphinx_rtd_theme
    ```

## ドキュメント作成

1. Sphinx プロジェクト作成

    ```bash
    sphinx-quickstart apidoc \
        --sep \
        --project "APP API Doc" \
        --author "{Your Name}" \
        -v 0.1 \
        --release 0.1 \
        --language='en' \
        --no-batchfile
    ```

2. reStructuredText 格納ディレクトリ作成

    ```bash
    mkdir apidoc/source/resources
    ```

3. apidoc/souce/conf.py の編集

    ```diff
    --- /tmp/conf.py        2024-06-27 21:32:16.171914587 +0900
    +++ conf.py     2024-06-27 21:40:53.859894009 +0900
    @@ -16,7 +16,10 @@
     # -- General configuration ---------------------------------------------------
     # https://www.sphinx-doc.org/en/master/usage/configuration.html#general-configuration
    
    -extensions = []
    +extensions = [
    +    'sphinx.ext.autodoc',
    +    'sphinx.ext.napoleon'
    +]
    
     templates_path = ['_templates']
     exclude_patterns = []
    @@ -26,5 +29,11 @@
     # -- Options for HTML output -------------------------------------------------
     # https://www.sphinx-doc.org/en/master/usage/configuration.html#options-for-html-output
    
    -html_theme = 'alabaster'
    +html_theme = 'sphinx_rtd_theme'
     html_static_path = ['_static']
    +
    +import os
    +import sys
    +# conf.py から見て calc.py を格納しているディレクトリを指定する
    +sys.path.insert(0, os.path.abspath('../../app'))
    +
    ```

4. apidoc/souce/index.rst の編集

    ```diff
    --- /tmp/index.rst      2024-06-27 21:46:50.619879827 +0900
    +++ index.rst   2024-06-27 21:42:26.219890337 +0900
    @@ -10,6 +10,7 @@
        :maxdepth: 2
        :caption: Contents:
    
    +   resources/modules
    
     Indices and tables
     ==================
    ```

5. API ドキュメント作成

    ```bash
    sphinx-apidoc --force -o apidoc/source/resources app
    ```

6. HTML 形式ドキュメントに変換

    ```bash
    sphinx-build -b html apidoc/source apidoc/build
    ```

## 参考サイト

- [Sphinxの使い方．docstringを読み込んで仕様書を生成](https://qiita.com/futakuchi0117/items/4d3997c1ca1323259844)
- [Python ライブラリ仕様ドキュメントの自動生成](https://zenn.dev/yamagishihrd/articles/8b5279a07000a4)
- [sphinx.ext.autodoc -- docstringからのドキュメントの取り込み](https://www.sphinx-doc.org/ja/master/usage/extensions/autodoc.html)
- [SphinxでのAPIドキュメント作成　まとめ](https://bonbonbe.hatenablog.com/entry/2016/09/22/211445)
