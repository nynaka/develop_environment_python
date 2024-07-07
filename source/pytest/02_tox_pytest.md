[tox - pytest](https://pypi.org/project/tox/)
===

```{note}
[Pytest Skeleton](01_skeleton.md) からの続きを想定しています。
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

    requirements に書かれている Python モジュールをインストールし、テストコード実行後、カバレッジを tox を実行したプロンプト、HTML、XML 形式で出力します。  
    XML は後ほど SonarQube で利用します。

    ```ini
    [tox]
    envlist = py312

    [testenv]
    setenv =
        PYTHONPATH = {toxinidir}:{envdir}
    deps = -rrequirements.txt
    commands =
        pytest -v --cov=./ --cov-report=term-missing
        pytest -v --cov=./ --cov-report=html
        pytest -v --cov=./ --cov-report=xml
    ```

## bandit のセキュリティチェックと ruff のコードチェックを導入する

```ini
[tox]
envlist = bandit, ruff, py312

[testenv]
setenv =
    PYTHONPATH = {toxinidir}:{envdir}
deps = -rrequirements.txt
commands =
    pytest -v --cov=./ --cov-report=term-missing
    pytest -v --cov=./ --cov-report=html
    pytest -v --cov=./ --cov-report=xml

[testenv:bandit]
deps = bandit
commands = bandit -r ./app

[testenv:ruff]
deps = ruff
commands =
    ruff format ./app
    ruff check --output-format=github ./app
```
