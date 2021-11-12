# Unit tests snippets (parameterized, mocking)

tips, commands, links

## sources 

[Pytest + tests coverage] (https://github.com/VictorZakharenko/fizzbuzz-unit-testing-snippets)

[Miguel's blog] (https://blog.miguelgrinberg.com/post/how-to-write-unit-tests-in-python-part-2-game-of-life)

## parameterized 

```bash
pip install parameterized
```

```bash
pytest --cov=life --cov-report=term-missing --cov-branch
```

Pytest provides parametrization support, but unfortunately this functionality is not very flexible and in particular does not work for tests that are implemented as methods in a class. I use a standalone package called parameterized, which supports tests written as functions or methods and is fully compatible with both pytest and unittest.

In its simplest form, parameterized allows you to duplicate a test function or method on the fly using different arguments. 

When you need to test a piece of code that raises an exception, the standard assert statement is not sufficient, because it cannot catch the exception. Both pytest and unittest provide specialized asserts for exceptions. Use pytest.raises() assertion to check that the load() method crashes with a RuntimeError when given a bad file

```python
with pytest.raises(RuntimeError):
    # code that raise exception
```

The idea of using a mock is to temporarily hijack part of the application and replace it with a fake version that is easily controlled from the test. The unittest framework that we are using has a mock sub-package that implements this.

```python
>>> from unittest import mock
>>> fake = mock.MagicMock()
>>> fake.return_value = 42
>>> fake()
42
>>> fake('hello')
42
>>> fake('hello', number=123)
42
```

In addition to being a fake function, the MagicMock instance keeps track of the calls it receives.
```python
>>> fake.call_count
3
>>> fake.call_args_list
[call(), call('hello'), call('hello', number=123)]
```

The mock package provides a few patching functions that allow you to inject a MagicMock instance into a part of your application. To replace a method within a class we can use the mock.patch_object() function