# Python Object Oriented Cheat Sheet

**Table of contents**
- [Class declaration](#Class_declaration)
- [Attributes](#Attributes)
- [Constructor](#Constructor)
- [Methods](#Methods)
    - [Overloading](#Overloading)
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
- **Class attributes** Known in JAVA as static, they are shared by all class objects
- **Instance attributes** Variable exclusive to one object  
- **Dynamic attributes** Defined dinamically

The first one is declared at the class and the second inside the constructor.

The last one is one of the reasons that makes Python a very flexible language, as **dynamic attributes** are defined as the name suggests dynamically during the execution of the program, without ever being referenced before.
```Python
# Dynamic attribute
<object>.wtvAttributeNeverDeclared = <someValue>
```



## Constructor 

Speaking of which, the **constructor** is created with the method *__init__(self, arg1, arg2, ...)*. 
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

To create an object, you just need to call the constructor:
 
```Python
# If you are not using it in the same file, you must import it!
from Book.py import Book

book1 = Book("Book name", "Author name", 123, True)
```

If you've learnend JAVA, probably *self* reminded you of *this*, and if that was the case you are not wrong!



## Methods

*Self* specifies the instance on which you call the method. In JAVA, *this* is done by default, but here it is not the case. It is always used on all **instance methods**, the most common method type.

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

Just like I did on the constructor up there with the variable *published* ;) With that, you can create objects giving or not this attribute as argument.
```Python
book1 = Book("Book name", "Author name", 123, False) 
# Will have published=False

book1 = Book("Book name", "Author name", 123)
# Will have published=True (default)
``` 



## Inheritance

*Soon available*



## Sources

- [Finxter](https://finxter.com/)
- [MakeUseOf](https://www.makeuseof.com/)
- [GeeksforGeeks](https://www.geeksforgeeks.org/)
