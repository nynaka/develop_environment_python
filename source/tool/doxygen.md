Doxygen
===

## インストール

```bash
sudo apt update
sudo apt install -y graphviz doxygen
```

## Doxyfile

通常はいろいろ情報を集めて頑張るのですが、[Doxygenでドキュメント自動生成](https://zenn.dev/karaage0703/articles/50c7445bc65350) の解説のなかで [いい感じのDoxyfile](https://gist.github.com/karaage0703/31d7be40796ad58f205120d707daacaf) がアップロードされているので、そちらを使用させて頂きます。

## 使い方

- ファイル構成

    ```text
    CurrentDir
     |-- app
     | |-- calc.py
     |-- requirements.txt
     |-- tests
     | |-- test_calc.py
    ```

- ドキュメント生成

    CurrentDir で下記コマンドを実行します。

    - Doxyfile の取得

        ```bash
        wget \
            https://gist.githubusercontent.com/karaage0703/31d7be40796ad58f205120d707daacaf/raw/e31dffbbf326790e47b95e64aa340371dc240734/Doxyfile
        ```

    - ドキュメント生成

        ```bash
        doxygen Doxyfile
        ```

    CurrentDir/html ディレクトリが生成されます。  
    CurrentDir/html/index.html をブラウザで参照すると、生成されたドキュメントを確認できます。  
    しっかりドキュメントを書いておくとクラス連携図まで作成してくれます。

## Doxyfile の編集

- GUI ツールのインストール

    ```bash
    sudo apt install doxygen-gui
    ```

- ツールの起動

    ```bash
    doxywizard
    ```

- メニューバーの `File` ⇒ `Open` で表示されるダイアログで編集対象の Doxyfile を開くと GUI ツールで編集できます。
