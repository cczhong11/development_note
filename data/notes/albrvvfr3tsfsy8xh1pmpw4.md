
I found one of my unittest always return the prod data and it is very strange to me.

After digging into the python unittest I finally know why.

For example

```py
# file1.py

def foo():
	return "2"

# file2.py
from file1 import foo

def bar()
	return foo()

```

Now we want to test it

if we do this
```
@patch("file1.foo", return_value="3")
def test_bar(self, mock:MagicMock):
	self.assertTrue(bar(), "3")
```

It would throw errors

We need to do this

```
@patch("file2.foo", return_value="3")
def test_bar(self, mock:MagicMock):
	self.assertTrue(bar(), "3")
```
