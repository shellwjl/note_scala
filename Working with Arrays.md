# Working with Arrays

## Fixed-Length Arrays

```scala
	// An array of ten integers,all initialized with zero
	val nums = new Array[Int](10)
	// A String array with ten elements,all initialized with null
	val s = new Array[String](10)
	// An Array[String] of length 2 --the type is inferred
	//**Note: No new when you supply initial values**
	val s = Array("hello","world")
	//Use () instead of [] to access elements
	s(0) = "Goodbye" 
```

## Variable-Length Arrays:Array Buffers

```scala
import scala.collection.mutable.ArrayBuffer
// Or new ArrayBuffer[int]
val b = ArrayBuffer[int]()

// Add an element at the end with +=
b += 1
// ArrayBuffer(1)

b += (1,2,3,5)
//ArrayBuffer(1,1,2,3,5)
//Add multiple elements at the end by enclosing them in parenthese

b ++= Array(8,13,21)
//ArrayBuffer(1,1,2,3,5,8,13,21)

b.trimEnd(5)
// ArrayBuffer(1,1,2)
// Removes the last five elements
```

the next operations are not as efficient--all elements afer that location must be shifted. 

```scala
	b.insert(2,6)
	// ArrayBuffer(1,1,6,2)
	
	b.insert(2,7,8,9)
	//ArrayBuffer(1,1,7,8,9,6,2)
	
	b.remove(2)
	// ArrayBuffer(1,1,8,9,6,2)
	
	b.remove(2,3)
	// ArrayBuffer(1,1,2)
```

If you want to build up an *Array* ,but you don't yet know how many elements you will need.In that case , first make an array buffer.then call 

```scala
	b.toArray
```
## Traversing Arrays and Array Buffers

Here is how you traverse an array or array buffer with *for* loop:

```scala
	for (i <- 0 until a.length)
		println(i+ ": " + a(i))
```

the constract `for (i<- range)` makes the variable i traverse all values of the range.To visit every second element, let i traverse

```scala
	0 until (a.length,2)
	 // Range(0,2,4,6,...)
```

To visit the elements starting from the end of the array,traverse

```scala
	(0 until a.length).reverse
	//Range(...,2,1,0) 
```

If you don't need the array index in the loop body,visit the array elements directly,like this:

```scala
	for (elem <- a)
		println(elem)
```
## Transforming Arrays

- Such transformations don't modify the original array ,but they yield a new one.

```scala
	val a = Array(2,3,5,7,11)
	val result = for(elem <- a) yield 2 * elem
		//result is Array(4,6,10,14,22)
```

Achieved with a *guard*:an *if* inside the *for*.

```scala
	for (elem <- a if elem%2 ==0) yield 2 *elem 
```
**the result is a new collection --the original collection is not affected**

Alternatively,you could write ```a.filter(_ % 2).map(2 * _)``` or even ``` a filter {_ % 2 == 0 } map {2 * _}```

- Given an array buffer of integers,we want to remove all but the first negative number.

```scala
	var first = true
	var n = a.length
	var i = 0
	while(i<n){
		if (a(i)>=0) i+=1
		else{
			if (first) {first = false;i+=1}
			else{a.remove(i); n -=1}
		}
	}
```
the better version:

```scala
	var first = false
	val indexes = for (i <- 0 until a.length if first || a(i)>=0)
		yield{
			if (a(i)<0 )first = false; i
		}
	for(j<- 0 until indexes.length) a(j) = a(indexes(j))
	a.trimEnd(a.length - indexes.length)
```
 