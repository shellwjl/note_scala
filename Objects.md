# Objects

Use objects when you need a class with a single instance or when you want to find a home for miscellaneous values or functions.


## Singletons

An object defines a single instance of a class with the features that you want.
```scala
	object Accounts {
		private var lastNumber = 0
		def newUniqueNumber() = {lastNumber += 1; lastNumber}
	}
```
when you need a new unique account number in your application,call *Accounts.newUniqueNumber*

The constructor of an object is executed when the object is first used.

## Companion Objects

In Java,you often have a class with both instance methods and static methods.
In Scala,you achieve this by having a class and a "companion" object of the same name.

```scala
	class Account{
		val id = Account.newUniqueNumber()
		private var balance = 0.0
		def deposit(amount:Double){ balance += amount}
		...
	}
	object Account{
		private var lastNumber = 0
		private def newUniqueNumber() = {lastNumber += 1;lastNumber}
	}
```
The class and its companion object can access each other's private features.**They must be located in the same source file**

## Objects Extending a class or trait


## The apply Method

The *apply* method is called for expressions of the form `Object(arg1,...,argN)`
returns an object of the companion class.

Here is an example of defining an *apply* method:
```scala
class Account private (val id: Int, initialBalance: Double){
  
  private var balance = initialBalance
  def current = initialBalance
}
object Account { // The companion object
  private var lastNumber = 0
  private def newUniqueNumber() = { lastNumber += 1; lastNumber }
  def apply(initialBalance: Double) =
    new Account(newUniqueNumber(), initialBalance)
}
// you can construct an account as
val acct = Account(1000.0)
println(acct.current)
```

- tip

`Array(100)` calls `apply(100)`,yield an Array[Int] with a single element,the integer 100. 

`new Array(100)` invokes the constructor `this(100)`. The result is an Array[Nothing] with 100 null elements.

## Application Objects

Each Scala program must start with an object's *main* method of type *Array[String] => Unit*

```scala
	object Hello{
		def main(args:Array[String]){
			println("Hello, World!")
		}
	}
```

Instead of providing a main method for your application,you can extend the *app* trait and place the program code into the constructor body:

```scala
	object Hello extends App{
		println("Hello, World!")
	}
```
you can get command-line arguments from the args property

```scala
	
```
## Enumerations

the standard library provides an *Enumeration* helper class that you can use to produce enumerations.

```scala
	object TrafficLightColor extends Enumeration {
		val red,yellow,green =Value   
		//initialize each value with a call to the value Method
	}
```
This is a shortcut for 
```scala
	val Red = Value
	val Yellow = Value
	val Green =Value
```

- you can pass IDs,names,or both to the value method:

```scala
	val Red = Value(0,"stop")
	val Yellow = Value(10)  //Name "Yellow"
	val Green = Value("Go")  //ID 11
```
the default name is the field name

- you can refer to the enumeration values as *TrafficLightColor.Red,TrafficLightColor.Yellow*. If that gets too tedious,use a statement `import TrafficLightColor._`

**remember that the type of the enumeration is TrafficLightColor.Value**.

- The call *TrafficLightColor.values* yields a set of all values:

```scala
	for (c <- TrafficLightColor.values) println(c.id + ":"+ c)
```

- Both of the following yield the object *TrafficLightColor.Red*

```scala
	TrafficLightColor(0) //Calls Enumeration.apply
	TrafficLightColor.withName("Red")
```


