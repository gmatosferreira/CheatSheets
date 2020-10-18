# Python Object Oriented Cheat Sheet

**Table of contents**
- [Class declaration](#Class_declaration)
- [Attributes](#Attributes)
- [Default_methods](#Default_methods)
- [Methods](#Methods)
    - [Overloading](#Overloading)
- [Encapsulation](#Encapsulation)
- [Inheritance](#Inheritance)
- [Sources](#Sources)



## Class_declaration

To define the data (attributes) and functionality (methods) of objects we use **classes**. 
An **object** is an instance of a class, representing a piece of encapsulated data with functionality. 

They often represent something in the real world.
```Python
# Book class declaration
class Book:
```



## Attributes

As I just mentioned, objects have attributes and depending on where they are declared, they can assume three types:
- **Class attributes** Known in Java as static, they are shared by objects of the class
- **Instance attributes** Variable exclusive to one object  
- **Dynamic attributes** Defined dinamically

The first one is declared at the class and the second inside the constructor.

The last one is one of the reasons that makes Python a very flexible language, as **dynamic attributes** are defined as the name suggests dynamically during the execution of the program, without ever being referenced before.
```Python
# Dynamic attribute
<object>.wtvAttributeNeverDeclared = <someValue>
```



## Default_methods 

Speaking of which, the **constructor** is one of the default methods in Python and is defined through the method `__init__(self, arg1, arg2, ...)`.

```Python
class Book:
    # Class attributes declaration
    counter = 0

    # Constructor
    def __init__(self, name, author, issn, published=True):
        # Instance attributes declaration 
        self.name = name
        self.author = author
        self.issn = issn
        self.published = published
```

> You might have noticed a strange notation on the argument "published". It has a default value!
>
> Learn more about it at [Methods](#Overloading).

To create an object, you just need to call the constructor:

```Python
# If you are not using it in the same file, you must import it!
from Book.py import Book

book1 = Book("Book name", "Author name", 123, True)
```

>  If you've learnend Java, probably *self* reminded you of *this*, and if that was the case you are not wrong!

Another default method is `__str__(self)`, which returns a string representation of the object for the end user (its goal is to be readable).

The last one, but not least important is `__repr__(self)`. It also returns a string representation of the object, but instead of directed to the end user its purpose is for debugging and development. So, its goal is to unambiguous. 

```python
def __str__(self):
    return f"{self.name} by {self.author}"

def __repr__(self):
    return f"Book[name={self.name}, author={self.author}, issn={self.issn}, published={self.published}]"
```



## Methods

*Self* specifies the instance on which you call the method. In Java, *this* is done by default, but here it is not the case. It is always used on all **instance methods**, the most common method type.

But just like in variables, there are three types of methods:
- **Instance methods** Able to access data and properties unique to each instance
- **Static methods** Cannot access anything else in the class. Totally self-contained code
- **Class methods** Can access limited methods and data in the class (only static ones) and manipulate class state

The first one is created just by declaring the method with the right arguments. 

However, the other two require **decorators**, words that must precede class or function declaration and start with the **@** sign. Unlike normal methods, there is no need to put parentheses at the end of it, unless we are passing arguments.

In addition, **class methods** take as first parameter *cls*.

```Python
# Instance method
def toString(self):
    return f"Book [name={self.name}, author={self.author}, issn={self.issn}, published={self.published}"

# Static method
@staticmethod
def issnConverter(issn):
    # Add functionality here

# Class method
@classmethod
def howManyBooks(cls):
    return cls.counter
```

**Class methods** are usually used for factory methods, whereas **static methods** to create utility functions.



### Overloading

If you want to have a function that can accept several options as arguments, you can use default arguments, by providing arguments a default value when declaring them.

Just like I did on the constructor up there with the variable *published*. With that, you can create objects giving or not this attribute as argument.

> This example uses the `__init__` method, but it applies to any other.

```Python
book1 = Book("Book name", "Author name", 123, False) 
# Will have published=False

book1 = Book("Book name", "Author name", 123)
# Will have published=True (default)
```

When you have multiple default arguments and you don't want to specify them all, just identify which parameter you are passing.

```python
# Let's assume that the Book constructor has another default value: translated
# def __init__(self, name, author, issn, published=True, translated=False)
# We can call the constructor passing all the parameters
book1 = Book("Book name", "Author name", 123, False, True)
# But if we only want to say it has been translated, but don't want to say anything about being published?
book1 = Book("Book name", "Author name", 123, translated=True)
```



## Encapsulation

To define variables and methods visibility Python uses a mechanism based on prefixing their name with one or double underscore.

There are three types of visibility:

- **Private** Visible and accessible only inside the class (one underscore)
- **Protected** Acessible inside the class and subclasses (double underscore)
- **Public** Default mode, everything is acessible inside and out of the class

```Python
# Private
self._<varName>
def _<funcName>:

# Protected
self.__<varName>
def __<funcName>:

# Public
self.<varName>
def <funcName>:
```



## Inheritance

Just like in Java, a class can extend another, inheriting its' attributes and methods, that can be overriden.

To call the superclass constructor, there must be invoked *super()*.

```Python
class Manual(Book):

    # Constructor
    def __init__(self, name, author, issn, subject, published=True):
        # Define Manual class attributes
        self.subject = subject
        # Call superclass constructor to define its' attributes
        super().__init__(name, author, issn, published)

    # Method overriding
    def toString(self):
        return f"Manual [subject={self.subject}\n\t{super.toString()}\n]"
```



## Sources

- [Finxter](https://finxter.com/)
- [MakeUseOf](https://www.makeuseof.com/)
- [GeeksforGeeks](https://www.geeksforgeeks.org/)
