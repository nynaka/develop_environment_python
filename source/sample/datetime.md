[datetime](https://docs.python.org/ja/3.14/library/datetime.html)
===

## サンプルコード

```python
import calendar
import datetime

weekdays = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]


def date_function() -> None:
    # 本日の日付の取得
    today = datetime.date.today()

    # 取得した日付の確認
    print(today)
    # 取得した日付の各プロパティの確認
    print(
        "year=%s month=%s day=%s week=%s"
        % (today.year, today.month, today.day, weekdays[today.weekday()])
    )
    # datetime ⇒ 文字列変換
    str_today = today.strftime("%Y/%m/%d")
    print(str_today)

    # 文字列 ⇒ 日付変換
    date1 = datetime.date(2025, 1, 1)
    print(date1)


def datetime_function() -> None:
    # 日時の取得
    now = datetime.datetime.now()

    # 取得した日時の確認
    print(now)
    # 取得した日付の各プロパティの確認
    print(
        "year=%s month=%s day=%s week=%s hour=%s minute=%s second=%s microsecond=%s"
        % (
            now.year,
            now.month,
            now.day,
            weekdays[now.weekday()],
            now.hour,
            now.minute,
            now.second,
            now.microsecond,
        )
    )

    # datetime ⇒ 文字列変換
    str_now = now.strftime("%Y/%m/%d %H:%M:%S.%f")
    print(str_now)

    # 文字列 ⇒ 日時変換
    timestamp1 = datetime.datetime(2024, 12, 31, 23, 59, 59, 999999)
    print(timestamp1)


def get_lastdate(y: int, m: int) -> datetime:
    """
    指定された年月の最終日を計算する。

    Args:
        y (int): 年を表す4桁の整数（例: 2023）
        m (int): 月を表す整数（1は1月、12は12月）

    Returns:
        datetime: 指定された年と月の最終日を表すdatetime.dateオブジェクト

    Raises:
        ValueError: 指定された月が1から12の範囲外の場合
    """
    _, days = calendar.monthrange(y, m)
    return datetime.date(y, m, days)


if __name__ == "__main__":
    date_function()
    datetime_function()

    lastday = get_lastdate(2025, 3)
    print(lastday)
```

## 参考サイト

- [pythonのdatetimeについて](https://qiita.com/t-iguchi/items/a0bb8a5f273b319e5755)
