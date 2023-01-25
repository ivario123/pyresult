# result

Provides a way to handle results in a "rust" like way. This is a very simple
implementation of the Result type in rust.

## Installation

To install this project, simply run the following command:

```bash
> pip install git+https://github.com/ivario123/pyresult
```

The resulting module will be called `result`.
There is no PyPI package, if you need one, look at [result](https://pypi.org/project/result/) that is already published. This package has no intention to replace that one, I was not aware of it when I started this project.

## Usage

```python
from result import Result, Ok, Error, unwrap

def divide(a, b) -> Result:
    if b == 0:
        return Error("Cannot divide by zero")
    return Ok(a / b)

unwrap(divide(10, 2)) # The unwrap will throw exception if the result is Err

# Or you can use match, call the ok function if the result is Ok, otherwise
# call the err function
divide(10,2).match(
    ok=lambda x: print(x),
    err=lambda x: print(x)
)

# Or you can use the is_ok and is_err functions
res = divide(10,2)
if res.is_ok():
    print(res.unwrap())
else:
    print(res.unwrap_err())
```

## Creating custom error types

```python
from result import to_error

NoSuchFile = to_error("No such file","The user tried to open a file that does not exist")
NotEnoughSpace = to_error("Not enough space","The user tried to write to a file but there was not enough space")
InvalidInput = to_error("Invalid input","The user provided invalid input")
```

This code snippet will generate 3 new types which derive from the Error class.
You can then use these types to create a Result.

```python
from result import Ok, Error
from custom_errors import NoSuchFile, NotEnoughSpace, InvalidInput

def open_file(path: str) -> Result:
    if path == "no_such_file":
        return Error(NoSuchFile())
    elif path == "not_enough_space":
        return Error(NotEnoughSpace())
    elif path == "invalid_input":
        return Error(InvalidInput())
    else:
        return Ok("File contents")
```

This will allow you to cleanly handle errors in your code.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details
