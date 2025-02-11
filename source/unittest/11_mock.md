MagicMock
===

## ディレクトリ構成

```text
WorkDir/
|-- main.py
|-- sub_module/
|  |-- __init__.py
|  |-- mod1.py
|  |-- mod2.py
```

## コードサンプル

- mod1.py

    ```python
    import mod2

    class Mod1Class:
        def __init__(self):
            print("sample module1")

        def function1(self):
            return "mod1 function1"

        def function2(self):
            return mod2.Mod2Class().function()
    ```

- mod2.py

    ```python
    class Mod2Class:
        def __init__(self):
            print("sample module2")

        def function(self):
            return "mod2 function"
    ```

- main.py

    ```python
    import unittest
    from unittest.mock import MagicMock, patch

    # モジュールを最初にインポートしておく
    import sub_module.mod1 as mod1

    class TestModule(unittest.TestCase):
        def test_mod1_function1(self):
            '''直接インポートしているモジュールのテスト
            '''
            submod = mod1.Mod1Class()
            self.assertEqual(submod.function1(), "mod1 function1")

        def test_mod1_function2(self):
            '''インポートしているモジュールの中でインポートしているモジュールのテスト
            '''
            submod = mod1.Mod1Class()
            self.assertEqual(submod.function2(), "mod2 function")

        @patch("sub_module.mod1.Mod1Class")  # モック対象を指定する
        def test_mod1_function1_external(self, mock_mod1_class):
            '''直接インポートしているモジュールをモックに置き換えるテスト
            '''
            # モックを作成
            mock_instance = MagicMock()
            # モックインスタンスの function1 を設定
            mock_instance.function1.return_value = "mocked return_value"
            # モックインスタンスを返すように設定
            mock_mod1_class.return_value = mock_instance

            submod = mod1.Mod1Class()  # モック対象のインスタンス化
            result = submod.function1()  # モック対象ではなく、モックのfunction1が実行される

            self.assertEqual(
                result, "mocked return_value"
            )

        @patch("mod2.Mod2Class") # モック対象を指定する
        def test_mod2_function1_external(self, mock_mod2_class):
            '''インポートしているモジュールの中でインポートしているモジュールをモックに置き換えるテスト
            '''
            # モックを作成
            mock_instance = MagicMock()
            # モックインスタンスの function1 を設定
            mock_instance.function.return_value = "mocked return_value"
            # モックインスタンスを返すように設定
            mock_mod2_class.return_value = mock_instance

            submod = mod1.Mod1Class()
            result = submod.function2()
            print(result)
            self.assertEqual(result, "mocked return_value")


    if __name__ == "__main__":
        unittest.main()
    ```

## 実行方法

```bash
PYTHONPATH="./sub_module/:$PYTHONPATH" python main.py
```
