# Maps and Tuples

## Maps

### Constructing a Map

```scala
val scores = Map("Alice"->10,"Bob"->3,"Cindy"-> 8)
```
this construcus an immutable *Map[String,Int]* whose contents can't be changed.
 
 - a mutalbe map:
 	
```scala
val scores = scala.collection.mutable.Map("Alice"->10,"Bob"->3,"Cindy"->8)
```
 	
 - start out with a blank map:
 	
```scala
val scores = new scala.collection.mutable.HashMap[String,Int]
```
 	
 - a map is a collection of pairs. The *->* operator males a pair. the value of `"Alice"->10` is `("Alice",10)`. 
 
 ### Accessing Map Values
 
 - Use the *()* notation to look up key values.
 
 ```scala
 	val bobsScore = scores("Bob")
 ```
 
 - Check whether there is a key
 
 ```scala
 	val bobScore = if (scores.contains("Bob")) scores("Bob") else 0
 ```
 
 or
 
 ```scala
 	val bobsScore = scores.getOrEles("Bob",0) 
 ```
 
 ### Updating Map Values
 
 - update or add (assuming scores is mutable)
 
 ```scala
 	scores("Bob")=10   // Updates the existing value
 	scores("Fred") = 7 // Adds a new key/value pair
 ```
 
 - use *+=* operation to add multiple associations: 
 
 ```scala
 	scores += ("Bob"-> 10,"Fred"->7)
 ```
 
 - remove a key and its associated value
 
 ```scala
 	scores -= "Alice"
 ```
 
 - you can not update an immutable map ,but you can obtain a new map that has the desired update:
 
 ```scala
 	val newScores = scores + ("Bob"->10,"Fred"->7)
 ```
 
 - Iterating over Maps
 
 ```scala
 	for ((k,v)<- map) process k and v
 ```
 	
 - just visit the keys or values, use the *keySet* and *values* methods.
 	
 ```scala
 	scores.keySet // A set such as Set("Bob","Fred","Alice")
 	for (v <- scores.values) println(v)
 ```
 	
 - reverse a map
 
 ```scala
 	for((k,v)<- map) yield (v,k)
 ```
 
 ### Sorted Maps
 
 By default,Scala gives you a hash table,you might want a tree map
 
 - get an immutable tree map
 
 ```scala
 	val scores = scala.collection.immutable.SortedMap("Alice"->10 ,"Fred"->7,"Bob" ->3,"Cindy"->8)
 ```
 
 ## Tuples
 
 A tuple value is formed by enclosing individual values in parentheses.`(1,3.14,"Fred")` is tuple of type `(Int,Double,java.lang.String)`
 
 ```scala
 	val t = (1,3.14,"Fred")
 ```
 
 - access its components with the methods *_1,_2,_3* 
 
 ```scala
 	val second =t._2 // the component positions of a tuple start with 1 ,not 0
 ```
 
 - use pattern matching to get at the components of a tuple
 
 ```scala
 	val (first, second, third ) = t //Sets first to 1,second to 3.14, third to "Fred"
 ```
 
 ### Zipping
 
 One reason for using tuples is to bundle together values so that they can be processed together.
 
 ```scala
 	val symbols = Array("<","-",">")
 	val counts = Array(2,10,2)
 	val pairs = symbols.zip(counts)
 ```
 yields an array of pairs `Array(("<",2),("-",10),(">",2)).The pair can then be processed together:
 
 ```scala
 	for ((s,n)<- pairs) Console.print(s*n)
 ```
 
 - Tip
 	if you have a collection of keys and a parallel collection of values,then zip them up and turn them into a map like this:
 	`keys.zip(values).toMap` 
  