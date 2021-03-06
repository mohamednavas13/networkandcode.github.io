---
title: python >  errors and exceptions
categories: python
---

A Python program can break due to various types of Errors. We shall explore few of these errors in the post. To make best use of this post, you should know certain topics in Python3 such as datatypes, expression, pass statement, etc.

Not all errors are significant, and hence there would be cases where we can skip certain errors to uninterrupt execution of our code, and this is achieved with ```try except``` blocks.

### ZeroDivisionError
This error is returned when we try to divide any number by 0
```
>>> 50 / 0
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ZeroDivisionError: division by zero
```
This time we tell Python not to break execution, by including things in a ```try except``` block.
```
>>> try:
...     0 /0
... except ZeroDivisionError:
...     pass
...
```
The expression 00/0 in the try block should have produced a ZeroDivisionError, but since we are saying ```except ZeroDivisionError```, that error would be excepted. So there wont be any error and there should'nt be any output as well as we have included ```pass``` in the except block. Note that ```pass``` is used as a placeholder where you don't want the program to any program, and just to go the next line of the code in sequence if any.

Let's try a different example, and also try printing some info instead of pass.
```
>>> try:
...     120 / 0
... except:
...     print('oh there was an exception')
...
oh there was an exception
```

### TypeError
We can try  to do an unsupported math operation, for example division, between a string and an integer, to trigger this error
```
>>> 30 / 'hello'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for /: 'int' and 'str'
```
Another example, but we include an exception for the error this time
```
>>> try:
...    'text' + 20
... except TypeError:
...     print('There was a type error')
...
There was a type error
```

### NameError
When we try calling a variable a.k.a object name, that was not defined before, we should get a NameError
```
>>> print(name)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'name' is not defined
```

### Error on improper exception
Error would still occur if the except statement doesn't include the correct error type.
```
>>> try:
...    'text' + 20
... except ValueError:  # wont process as we get TypeError above
...     print('There was a value error')
...
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
TypeError: can only concatenate str (not "int") to str
```
We get an error cause, as we tried to except ValueError, where as the statement in try block was producing TypeError


### Exception for Multiple Errors
We can include exceptions for multiple error types by separating each error type by a comma. The following example should have produced NameError.
```
>>> try:
...     print(name)
... except(NameError, TypeError):
...     print('There was a NameError or TypeError')
...
There was a NameError or TypeError
>>>
```
Another example for multiple exceptions, however it should have generated a TypeError
```
>>> name = 'Michael'
>>>
>>> try:
...     print(name)
...     'text' - 20
... except(NameError, TypeError):
...     print('There was a NameError or TypeError')
...
Michael
There was a NameError or TypeError
```    

Or we could even provide exceptions for any error by jus saying expect with out any error type. Let's try printing the variable city, which is not defined before, this should have caused a NameError
```
>>> try:
...     print(city)
... except:
...     print('oh! there was an exception error')
...
oh! there was an exception error
```
Note that in the snippet above, we have excepted all types of Errors and not just NameError.

One more example for providing exception to all errors
```
>>> try:
...     print(y)
... except:
...     print('There was an error, But that\'s ok, go ahead with the rest of the code')
...
There was an error, But that's ok, go ahead with the rest of the code
>>> print('Thank you')
Thank you
>>>
```
### Print the standard message
Its sometimes much apt to print the message the error would have been generated for, instead of printing a user defined message
```
>>> try:
...    print(name)
... except NameError as n:
...     print(n)
...
name 'name' is not defined
```
So the mesage ```name 'name' is not defined``` is coming straight from the NameError itself, and says why there was a NameError. Here we have caught the error message as variable ```n``` by saying ```NameError as  n```, and then we ```print(n)``` it displays the error message or reason stored.

One more example
```
>>> try:
...     'string ' + 2
... except TypeError as t:
...     print(t)
...
can only concatenate str (not "int") to str
```
This time it printed the reason for the TypeError

### Raise Errors
We could even raise errors manually with the ```raise``` statement.
```
>>> try:
...     raise TypeError()
... except TypeError as t:
...     print(t)
...

```
It shouldn't print anything cause we raised the error manually, and there wasn't any standard error message, providing the reason on why it was generated.

We could however print a user defined message.
```
>>> try:
...     raise TypeError()
... except TypeError:
...     print('The type error was raised manually')
...
The type error was raised manually
```

### Class with Multi level inheritance
The errors we have seen so far belong to a predefined class called Exception.
```
>>> print(ValueError.__bases__)
(<class 'Exception'>,)

>>> print(TypeError.__bases__)
(<class 'Exception'>,)
```
Note that __bases__ is used to find the base class of a class.

Let's define a class called Base that would inherit the Exception class. We are not defininig any unique attributes or methods for this class, 
and hence say pass.
```
>>> class Base(Exception):
...     pass
...
```
Now another class called Child, which would inherit the class 'Base'.
```
>>> class Child(Base):
...     pass
...
```
And the Grandchild class to further inherit from Child.
```
>>> class Grandchild(Child):
...     pass
...
```
We are going to loop over these classes, to raise that exception in each iteration, so that the except block would be executed.
```
>>> for i in [ Base, Child, Grandchild ]:
...     try:
...         raise i()
...     except Grandchild:
...         print("Grand child exception")
...     except Child:
...         print("Child exception")
...     except Base:
...         print("Base exception")
...
Base exception
Child exception
Grand child exception
```
The loop above was straight forward, i.e. when the Base exception was raised, it would print 'Base exception'.

Let's now reverse the order of except statements and try.
```
>>> for i in [ Base, Child, Grandchild ]:
...     try:
...         raise i()
...     except Base:
...         print("Base exception")
...     except Child:
...         print("Child exception")
...     except Grandchild:
...         print("Grand child exception")
...
Base exception
Base exception
Base exception
>>>
```
Each time the Base exception will be caught, as both Child and Grandchild inherit Base. And the except block for Base was written first.

### Custom message
Define a custom exception as a sub / derived class of the Exception class.
Define a new class MyException as a derived class of the Exception class.
```
>>> class MyException(Exception):
...     pass
...
```
Define InputException as a derived class of MyException. So InputException also inherits Exception. 
However a custom message is defined for InputException, so it won't inherit this message from Exception.
```
>>> class InputException(MyException):
...     def __init__(self, message):
...          self.message = message
...
```
Create an instance 'i' of the InputException class, and pass argument for message.
```
>>> i = InputException('This is a user defined exception')
>>>
```
We can now raise i, i.e. an InputException instance, to create an error manually, and the screen would throw the custom message. 
```
>>> raise i
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
__main__.InputException: This is a user defined exception
```
```__main__``` is the value of the ```__name__``` keyword, its used to fetch the name of an object, and by default it's ```__main__```.
It means the value of ```__name__``` in our program or module is always ```__main__```, and it has global scope, its like the name of the 
overall current module.
```
>>> print(__name__)
__main__
```
Each class will also have this attribute. For instance the ```__name__``` atrribute of InputException is its name itself, 
same is the case for Exception.
```
>>> print(InputException.__name__)
InputException

>>> print(Exception.__name__)
Exception
```

### SyntaxError
We have seen errors than can be excepted, but there are also Errors that can not be excepted. 
For instance IndentationError can not be excepted.
```
>>> if True:
...     print('hi')
...   print('hello')
  File "<stdin>", line 3
    print('hello')
                 ^
IndentationError: unindent does not match any outer indentation level
```
Indentation error is derived class of SyntaxError.
```
>>> print(IndentationError.__bases__)
(<class 'SyntaxError'>,)
```

Make a syntax error directly.
```
>>> print('Hello)
  File "<stdin>", line 1
    print('Hello)
                ^
SyntaxError: EOL while scanning string literal
```
SyntaxError inherits Exception, which in turn inherits BaseException.
```
>>> print(SyntaxError.__bases__)
(<class 'Exception'>,)
>>> print(Exception.__bases__)
(<class 'BaseException'>,)
>>> print(BaseException.__bases__)
(<class 'object'>,)
>>> print(object.__bases__)
()
>>>
```

--end-of-post--
