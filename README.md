README.md
===

## このリポジトリ

Python の開発環境構築メモです。

## ドキュメントのビルド方法

- venv のインストール

    ```bash
    sudo apt install python3-venv
    ```

- venv 環境作成と有効化

    ```bash
    python3 -m venv venv
    source ./venv/bin/activate
    ```

- ドキュメントのビルドツールのインストール

    ```bash
    pip install --break-system-packages \
        sphinx sphinx_rtd_theme \
        sphinxcontrib-actdiag \
        sphinxcontrib-blockdiag \
        sphinxcontrib-nwdiag \
        sphinxcontrib-seqdiag \
        myst_parser sphinx_markdown_tables
    ```

- 日本語フォントのインストール

    - Debian / Ubuntu

        ```bash
        sudo apt install -y \
            fonts-ipafont fonts-ipaexfont
        ```

    - macOS

        ```bash
        brew install font-ipafont font-ipaexfont
        ```

- source/conf.py の設定

    <details>
    <summary>主な変更差分</summary>
    
    ```diff
    --- /tmp/conf.py        2024-06-23 21:22:26.891931254 +0900
    +++ source/conf.py      2024-06-23 21:24:13.763927006 +0900
    @@ -14,7 +14,23 @@
     # -- General configuration ---------------------------------------------------
     # https://www.sphinx-doc.org/en/master/usage/configuration.html#general-configuration
    
    -extensions = []
    +extensions = [
    +    'sphinx.ext.autodoc',  # 自動生成ドキュメントの拡張
    +    'sphinx.ext.napoleon', # GoogleスタイルおよびNumpyスタイルのdocstringを解釈するための拡張
    +    'myst_parser',         # MyST (Markedly Structured Text) パーサーを有効化
    +    'sphinx_markdown_tables',
    +    'sphinxcontrib.blockdiag',
    +    'sphinxcontrib.seqdiag',
    +    'sphinxcontrib.actdiag',
    +    'sphinxcontrib.nwdiag',
    +    'sphinxcontrib.rackdiag',
    +    'sphinxcontrib.packetdiag',
    +]
    +
    +source_suffix = {
    +    '.rst': 'restructuredtext',
    +    '.md': 'markdown',  # Markdownの拡張子を設定
    +}
    
     templates_path = ['_templates']
     exclude_patterns = []
    @@ -24,5 +40,5 @@
     # -- Options for HTML output -------------------------------------------------
     # https://www.sphinx-doc.org/en/master/usage/configuration.html#options-for-html-output
    
    -html_theme = 'alabaster'
    +html_theme = 'sphinx_rtd_theme'
     html_static_path = ['_static']
    ```
    <details>

- ドキュメントのビルド

    ```bash
    make html
    ```

    カレントディレクトリ配下に `build` ディレクトリが作成されます。
