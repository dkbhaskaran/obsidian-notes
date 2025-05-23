## Constructors in C++

To set the terminology, there are following types of constructors
- Default : The constructor that does not accepts any arguments.
- Parameterized  :  Constructor that accepts a parameter.
- Copy constructor : Constructor that accepts a copy of an existing object.
```
	MyObject(const MyObject &other);
```
- Move constructor : Constructor that accepts a temporary object (rvalue) and transfers the ownership.
```
MyObject(MyObject &&other);
```
- Delegating constructor : One that calls other constructor.
- explicit : One that uses **explicit** keyword to avoid implicit conversions.

Lets try to understand the object life cycle in C++ through some important questions

- What should be looked into when implementing a move constructor and move assignment for a class?
- The

