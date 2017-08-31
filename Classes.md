# Classes

## Simple Classes and Parameterless Method

```scala
	class Counter{
		private var value = 0 //must initialize the field
		def increment() {value +=1 } // Methods are public by default
		def current() = value
	}
```

- construct objects and invoke methods

```scala
	val myCounter = new Counter // Or new Counter()
	myCounter.increment()
	println(myCounter.current)
```
It is considered good style to use () for a mutator method(a method that changes the object state),and to drop the () for an accessor method (a method that does not change the object state)

- enforce this style by declaring *current* without ():

```scala
	class Counter{
	...
	def current = value // No () in definition
	}
```
Now the class user must use *myCounter.current*,without parentheses.

## Properties with Getters and Setters

Scala provides getter and setter methods for every field.For a private field,the getter and setter methods are private.

```scala
	println(fred.age) //Calls the method fred.age()
	fred.age = 21 //Calls fred.age_=(21)
```

- redefine the getter and setter methods yourself.

```scala
	class Person{
		private var privateAge = 0
		def age = privateAge
		def age_=(newValue:Int){
			if(newValue > privateAge) privateAge = newValue;
		}
	}
``` 

- Tip
 1. if the field is private, the getter and setter are private.
 2. if the field is a *val*,only a getter is generated.
 3. if you don't want any getter or setter,declare the field as *private[this]*
 
## Properties with Only Getters

- *read-only property* with a getter but no setter. use a *val* field: 

```scala
	class Message{
		val timeStamp = new java.util.Date
		...
	}
```

- a property that a client can not set at will,but that is mutated in some other way.

```scala
	class Counter{
		private var value =0
		def increment(){value +=1 }
		def current=value // NO () 
	}
	
	val n = myCounter.current 
```

- To summarize

1. var foo : Scala synthesizes a getter and a setter
2. val foo : Scala synthesizes a getter
3. You define methods *foo* and *foo_=*.
4. You define a method *foo*

you can not have a write-only property

## Object-Private Fields 

- a method can access the private fields of all of its class.
```scala
	class Counter{
	private var value = 0
	def increment() {value += 1}
	def isLess(other :Counter) = value < other.value
	// Can access private field of other object
	}
```

- an more severe access restriction, with the **private[this]** qualifier

```scala
	private[this] var value = 0 // Accessing someObject.value is not allowed
``` 

Now, the methods of the *Counter* class can only access the *value* field of the  
current object,not of other objects of type *Counter*. This access is sometimes called **object-private**.


With a class-private field,Scala generates private getter and setter methods.However, for an object-private field,no getters and setters are generated at all.

## Bean Properties

- annotate a Scala field with *@BeanProperty*, then such methods are automatically generated.

```scala
	import scala.reflect.BeanProperty
	class Person{
		@BeanProperty var name: String = _
	}
```
generate four methods:
1. name:String
2. name_=(newValue:String): Unit
3. getName():String
4. setName(newValue:String):Unit

| Scala Field | Generated Method  | when to Use |
| :----: |:---: | :---: | 
| val/var name | public name <br> name_=(var only) | To implement a property that is publicly accessible and backed by a field |
|@BeanProperty val/var name| public name <br> getName() <br> name_=(var only) <br> setName(...) (var only) | To interoperate with JavaBeans. |
|private val/var name | private name <br> name_=(var only) | To confine the field to the methods of this class,just like in java. Use private unless you really want a public property.|
|private[this] val/var name | none | To confine the field to methods invoked on the same object.Not commonly used |
| private [ClassName] val/var name | implementation-dependent | To grant access to an enclosing class.Not commonly used.


## Auxiliary Contructors

A Scala class has one constructor that is more important than all the others,called the *primary constructor*. In addition, a class may have any number of *auxiliary constructors*

just two differences with Java or C++
1. The auxiliary constructors are called *this*. (In Java or C++, constructors have the same name as the class)
2. Each auxiliary constructor must start with a call to a previously defined auxiliary constructor or the primary constructor.

```Scala
	class Person{
		private var name =""
		private var age = 0
		def this(name : String){   // An auxiliary constructor
			this() // Calls primary constructor
			this.name = name
		}
		def this(name:String , age: Int){
			this(name) // Calls previous auxiliary constructor
			this.age =age
		}
	}
	
	// You can construct objects of this class in three ways:
	val p1 = new Person //Primary constructor
	val p2 = new Person("Fred") // First auxiliary constructor
	val p3 = new Person("Fred" , 23) //Second auxiliary constructor
```  

## The Primary Constructor

The primary constructor is interwoven with the class definition.

1. The parameters of the primary constructor are placed immediately after the class name.

```Scala
	class Person(val name:String, val age:Int){
		...
	} 
```

*name* and *age* become fields of the *person* class. A construction call such as *new Person("Fred",42)* sets the name and age field.

Half a line of Scala is the equivalent of seven lines of Java

```Java
	public class Person { 
		private int age;
		private String name;
		public Person (String name, int age){
			this.name=name;
			this.age=age;
		}
		public String name(){return this.name;}
		public int age(){return this.age;}
	}
```

2. The primary constructor executes **all statements** in the class definition.

```Scala
	class Person(val name:String,val age:Int){
		println("Just constructed another person")
		def description = name + " is " + age + " years old "
	}
```
the println statement is a part of the primary constructor.It is executed whenever an object is constructed

- Primary constructor parameters can have any of the forms. 
	for example, `class Person(val name: String, private var age: Int)` declares and initializes fields 
` val name:String`

` private var age:Int`

- Construction parameters can also be regular method parameters,without val or var.


| Primary Constructor Parameter | Generated Field/Methods  | 
| :----: |:---: | 
| name: String | object-private field,or no field if no method uses name|
| private val/var name : String | private field, private getter/setter |
| var/val name: String | private field,public getter/setter |
| @BeanProperty val/var name: String | private field,public Scala and JavaBeans getter/setter |


## Nested Class

You can define functions inside other functions, and classes inside other classes.

```scala
	import scala.collection.mutable.ArrayBuffer
	class Network{
		class Member(val name:String){
			val contacts = new ArrayBuffer[Member]
		}
		private val members = new ArrayBuffer[Member]
		
		def join (name :String)={
			val m = new Member(name)
			members += m 
			m
		}
	}
	
	val chatter = new Network
	val myFace = new Network
```
each instance has its own class *Member*,just like each instance has its own field members. 

Add a member within its own network,but not across networks

```scala
	val fred = chatter.join("Fred")
	val wilma = chatter.join("Wilma")
	fred.contacts += wilma //OK
	val barney = myFace.join("Barney") //has type myFace.Member
	fred.contacts += barney  //No can't add a myFace.Member to a buffer of chatter.Member
```
if you don't want this bebavior, there are two solutions.

1. move the Member type to the Network *companion object*

```scala
	object Network{
		class Member(val name : String){
			val contacts = new ArrayBuffer[Member]
		}
	}
	class Network{
		private val members = new ArrayBuffer[Network.Member]
		...
	}
```

2. use *type projection* Network#Member,which means "a Member of any Network"

```scala
	class Network{
		class Member(val name :String){
			val contacts = new ArrayBuffer[Network#Member]
		}
		...
	}
```