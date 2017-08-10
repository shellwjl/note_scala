# Control Structures and Functions

---
## Conditional Expressions
- if/else 

```scala
	if (x >0) 1 else -1
	val s = if (x>0) 1 else -1  
```
- mixed-type expression

```scala
	if (x>0) "positive" else -1
```
- every expression is suposed to have *some* value
think of **Unit** as the analog of **void** in Java or C++. 

```scala
	if (x>0) 1  //no else
```
## Block Expression and Assignments

- a {} block contains a sequence of expressions,and the result is also an expression. **The value of the block is the value of the last expression**

```scala
	val distance = {val dx = x - x0;val dy = y - y0; sqrt(dx*dx +dy*dy)}
```
- **assignment have no value--strictly speaking,they have a value of type Unit**

```scala
	x=y=1 //No y=1->>Unit 
```
## Input and Output

```scala
print("Answer: ")
println(42)
println("Answer: " + 42)
```

- readLine function

```scala
 val name = readLine("Your name: ")
 print("Your age: ")
 val age = readInt()
 printf("Hello, %s! Next year, you will be %d.\n",name,age+1)
```

## Loops

- while

```scala
	while(n>0){
		r = r * n
		n -= 1
	}
```

- for 

```scala
	for (i <- 1 to n)
		r = r * i
```

The call *1 to n* returns a Range of numbers from 1 to n

```scala
	for(i <- expr)
```
- until 

```scala
	val s = "Hello"
	val sum = 0
	for(i <- 0 until s.length) // **Last value for i is s.Length-1**
		sum += s(i)
```
the better case:
```scala
	var sum = 0
	for(ch<- "Hello") sum +=ch
```

- What to do if you need a **break**?

1. Use a Boolean control variable instead.
2. Use nested functions-- you can *return* from the middle of a function
3. Use the *break* method in the *Breaks* object

## Advanced for Loops and for Comprehensions

- multiple *generators* of the form *variable <- expression*

```scala
	for (i <- 1 to 3; j <- 1 to 3) print((10*i+j) + " ")
```
- guard

```scala
	for (i <- 1 to 3; j<- 1 to 3 if i!=j) print((10 * i + j) + " ")
```

- definitions

```scala
	for (i <- 1 to 3; from = 4 - i ; j<- from to 3) print((10*i+j)+" "))
```

- yield

```scala
	for(i <- 1 to 10) yield i % 3
	// Yield Vector(1,2,0,1,2,0,1,2,0,1)
```

---
## Functions


