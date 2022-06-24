# Python `raise from`

- `raise from` sets the cause of the exception you raised
- Possible use case is when you catch a lot of different exceptions, then raise a new generic exception but you still want to keep a record of which specific exception was raised

## Sample

```python
class FooException(Exception):
    pass


class BarException(Exception):
    pass


def raise_foo():
    raise FooException("foo raised!")


def catch_foo_raise_bar():
    try:
        raise_foo()
    except FooException:
        raise BarException("bar raised!")


def catch_foo_raise_bar_with_from():
    try:
        raise_foo()
    except FooException as e:
        raise BarException("bar raised!") from e


def catch_bar(with_from: bool = True):
    try:
        catch_foo_raise_bar_with_from() if with_from else catch_foo_raise_bar()
    except BarException as e:
        print(e)
        if isinstance(e.__cause__, FooException):
            print("it's from foo!")
            print(f"foo message: {e.__cause__}")
        else:
            print("it's from something else!")


catch_bar()
print("=" * 50)
catch_bar(with_from=False)
```

Output:
```
bar raised!
it's from foo!
foo message: foo raised!
==================================================
bar raised!
it's from something else!
```
