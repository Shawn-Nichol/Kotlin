# Classes
Classes in Kotlin are declared using the keyword 'class'
```
class MyClass {
  ...
}
```
The class declaration consists of the class name, the class header(primary constructor), and the class body, surrounded by curly braces. Both header and the body are optional;
if the class has no body, the curly braces can be omitted. 
```
class Empty
```

Constructors
 A class in kotlin can have a primary constructor and one or more secondary constructors. The primary constructor is part of the class header: it goes after the classname
 and optional type parameters.
 
 ```
 class Person constructor(firstName: String) {
 
 }
 ```
 
 If the primary constructor doesn't have any annotations or visiblity modifiers, the 'constructor' keyword can be omitted. The primiary constructor can not contain any code, 
 initialization code can be entered in the 'init' block. During an instance initialization the initializer blocks are executed in the same order as they appear in the class body,
 interleaved with the property initializers.
 
 ```
 class MyClass(name: String) {
  val firstProperty = "First property: $name".also(::println)
  
  init {
    println("First initializer block that prints ${name}")
  }
  
  val secondProperty = "Second property: ${name.length}")
  
  init {
    println("Second initializer: ${name.length}")
  }
}
```
The parameters of the primary constructor can be used in the 'init' block. In fact for declaring properties and intializing them from the primary constructor, kotlin has concise
syntax:
```
class Person(val firstName: String, val lastName: String, var age: Int) {
}
```

If the construtor has annotations or visibility modifiers, the constructor keyword is required, and the modifiers go before it. 
```
class Customer public @Inject constructor(name: String) {

}
```


Secondary Constructors
The class can also declare secondary constructgors, which are prefixed with constructor
```
class Person {
  var children: Mutablist<Person> = mutableListOf<>()
  constructore(paretn: Person) {
    parent.children.add(this)
  }
}
```

If the class has a primary constructor, each scondary constructor needs to delegate to the primary constructor, either
directly or indirectly through another secondary construcor. Delegation to another constructor of the same class is done using
the 'this' keyword
```
class Person(val name: String) {
  var childre: MutableList<Person> = mutableListof<>()
  
  constructor(name: String, parent: Person) : this(name) {
    parent.children.add(this)
  }
}
```

Note code in the initializer blocks effectively becomes part of the primary constructor. Delegation to the primary constructor happens 
as the first statement of a secondary constructor, so the code in all initializer blocks and property initializers is executed before the
secondary constructor body.  Even if the calls has no primary constructor, the delegatioin stil happens implicityly, and the intializer 
blocks are still executed:
```
class Constructors {
  int {
    println("Init block")
  }
  
  constructor(i: Int) {
    println("Constructor")
  }
}
```

If a non-abstract class does not declare any constructors, it will have a generated primary constructor with no arguments. The visibility of the constructor will be public.
If you do not want your class to have a public constructor, you need to dceclare an empty primary constructor with non-deafult visibility. 
```
class DontCreateMe private constructor() { /******/}
```

Create Instances of classes
To create an instance of a class, we call the constructor as if it were a regular function:
```
val invoice = Invoice()
val customer = Customer("Joe Smith")
```


Class members
Class members can contain the following
- Constructors and initializer blocks
- Functions
- Properties
- Nested and inner classe
- Object Declarations







