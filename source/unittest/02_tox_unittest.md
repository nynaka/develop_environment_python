[tox - unittest](https://pypi.org/project/tox/)
===

```{note}
[UnitTest Skeleton](01_skeleton.md) からの続きを想定しています。
```

## インストール

```bash
pip install tox
```

## tox.ini の作成

- ファイル構成

    ```text
    |-- app
    | |-- calc.py
    |-- requirements.txt
    |-- tests
    | |-- test_calc.py
    |-- tox.ini
    ```

- tox.ini

    `coverage` がインストールされていない場合はインストールして、テストコードを実行し、カバレッジを tox を実行したプロンプト、HTML、XML 形式で出力します。  
    XML は後ほど SonarQube で利用します。

    ```ini
    [tox]
    envlist = py312

    [testenv]
    deps =
        coverage
    commands =
        coverage erase
        coverage run -m unittest discover -s tests -p 'test_*.py'
        coverage report -m
        coverage html -d cover
        coverage xml -i
    ```

## bandit のセキュリティチェックと ruff のコードチェックを導入する

- tox.ini の修正

    ```ini
    [tox]
    envlist = bandit, ruff, py312

    [testenv]
    deps =
        coverage
    commands =
        coverage erase
        coverage run -m unittest discover -s tests -p 'test_*.py'
        coverage report -m
        coverage html -d cover
        coverage xml -i

    [testenv:bandit]
    deps = bandit
    commands = bandit -r ./app

    [testenv:ruff]
    deps = ruff
    commands =
        ruff format ./app
        ruff check --output-format=github ./app
    ```
