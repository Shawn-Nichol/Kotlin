# Inheritance

All classes in kotlin have a common `superclass Any`, `Any` is the default supercalss for a class with no sypertypes declared. 
```
call Example // Implicityly inherits from Any
```

Any has three methods equals(), hashCode() and to String(). Thus they are defined for all kotlin classes.

By default kotlin classess are final: they can't be inherited. To make a class inheritable, mark it with the open keyword. 
```
open class Base // Class is open for inheritance
```

To delcare an explicit supertype, place the type after a colon in the class header:
```
open class Base(p: Int)
class Derived(p: Int) : Base(p)
```

If the derived class has a primary constructor, the base class can be initialized right there, using teh parameters of the primary constructor. 

If the derived class has no primary constructor, then each secondary constructor has to initialize the base type using the super keyword, or to 
delegate to another constructor which does that. Note that in this case different secondary constructors can call different secondary constructors
can call different constructors of the base type:

```
Class MyView :View {
  constructor(ctx: Context) : super(ctx)
  constructor (ctx: Context, attrs: AttributeSet) : super(ctx, attrs)
}
```


Overriding methdos 
Kotlin requires explicit modifiers for overridable members and for overrides:
```
open class Shape {
  open fun draw()( {
    ... 
  }
  fun fill() { 
    ...
  }
}

class Circle() : Shape() {
  override fun draw() {
    ...
  }
}
```

The override modifier is required for Circle.draw(). If it were missing, the compiler would complain. If there is no open modifier on a function, like shape.fill(), 
declaring a method with the same signature in a subclass is illegal, either with override or without it. The open modifier has no effect when added on memebers of a final class

A member marked override is itself open, i.e. it may be overridden in subclasses. If you want to prohbit re-overriding , use final
```
open class Rectangle() : Shape()
  final override fun draw() {
    ...
  }
```


Overriding properties
Overriding proeprties works in a simliar way to overriding methods; properties declared on a superclass that are then redclared on a derived class must be prefaced with Override, and they must have a compatible type. Each declared property can be overridden by a propety with an initializer or by a property with a get method. 
```
open class Shape {
  open val vertextCount: Int = 0
}

class Rectangle : Shape() {
  override val vertexCount = 4
}
```
You can also override a val property with a var property, but not vice versa. Thjis allowed becuase a val property essentially declares a get methdo, and overriding it as a var 
additionally declars a set method in the derived class. 

Note that you can use teh override keywordd as part of the proerty declaration in a primary constructor. 
interface Shape {
  val vertexCount: int
}

class Rectangle (overide val vertexCount: int = 4) : Shape // alwayus has 4 vertices

class Polygon : Shape {
  override var vertextCount: Int = 0 // can be set to any number later
}
```


Derived class initialization order
During construction of a new instance of a derived class, teh base class initialization is done as the first step (preceded only by evaluation of the argumetns for the base class constructor)
and thus happens before the initialization logic of the derived class is run
```
open class Base(val name: String) {
  inti {
    println("Initializing base")
  }
  
  open val size: int = name.length.also {
    println("Initializing size in Base: $it")
}

class Derived (
  name: String,
  val lastName: String 
) : Base(name.capitalize().also {print("Argument for Base: $it) }) {
  init { println("initializing Derived") }
  override val size: Int = (super.size + lastName.length).also {println("Initializing size in Derived: $it")}
  
}
```

It means that, by the time of the base class constructor execution, the properties declared or overridden in the derived class are not yet initialized. If an of those
properties are used in teh base calss initialization logic(either directly or indirectly, thourgh another overridden open member implementation), it may lead to
incorrect behavior or a runtime failure. When designing base class, you should there fore avoid using open members in teh constructors, property initializers and init blocks. 


Calling superclass implementation
code in a derived class can call  its supercalls functions and property accessors implementations using 'super' keyword.
```
open class Rectangle {
  open fun draw() {println("Drawing a rectangle") }
  val borderColor: String get() = "black"
}

class FilledRectangle : Rectangle() {
  Override fun draw() {
    super.draw()
    println("Filling the rectangel")
  }
  
  val fillColor: String get = super.borderColor
}
```

Inside an inner class, accessing teh superclass of the outer class is done with teh super keyword qualified with the outer class name super@Outer:
```
class FilledRectangel: Rectangel() {
  fun draw() { /*****/}
  val borderColor: Stirng get() = "black"
  
  inner class filler {
    fun fill() { /****/}
    fun drawAndFill() {
      super@FilledRectangle.draw() // Calls Rectangle's implemenatation of draww()
      fill()
      println("Drawn a filled rectangle with color ${super@filledRectangle.borderColor}")
    }
  }
```


Overriding rules
In Kotlin, implementation inheritance is regulated by the folowing rule: if a class inherits multiple implementations of teh same member from its immediate superclasses,
it muset override this member and provide its own implementaion (perhaps, using one of the inherited ones). To denote the supertype from which the inherited implementation
is taken, we use super qualified by teh supertype name in agnle brackets. 

```
open class Rectangle {
  open fun draw() { /*****/ }
}

interface Polygon {
  fun draw() { /*****/ } // interface members are 'open' by default
}

class Square() : Rectangle, Polygon {
  // The compiler requires draw() to be overrideen:
  override fun draw() {
    super<Rectangel>.draw() // Call to Rectangle.draw()
    super<Polygon>.draw() // call to Polygon.draw()
  }

}
```
It's fine to inherit from both Rectangle and Polygon, but both of them have their implementations of draw(), so we have to override draw() in Square and provided its own implementations that eliminates
the ambiguity.

Abstract classes
A class and some of its members may be declared abastrac. An abstract member does not have an implementation in its class. Note that we do not need to annotate an abstract class or function with open
- it goes without saying. 

We can override a non-abstract open memeber with an abstract one. 
```
open class Plygon {
  open fun  draw() {}
}

abstract class Rectangle : Polygon() {
  abstract override fun draw()
}
```


Companion Objects
If you need to write a function that can be called without having a class instance but needs access to the internals of a class (for example, factory method, you can write it as a member of an object
delcartation inside that class.

Even more specifically, if you declare a companion object inside your class, you can access its member using
only the class name as a qualifier. 
