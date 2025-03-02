# Tags Language

Built off of [monkey](https://github.com/haifenghuang)

Table of Contents
=================

* [Language Tour](#language-tour)
  * [Comments](#comments)
  * [Data Types](#data-types)
  * [Constants(Literal)](#constantsliteral)
  * [Variables](#variables)
  * [Reserved keywords](#reserved-keywords)
  * [Type conversion](#type-conversion)
  * [qw(Quote word) keyword](#qwquote-word-keyword)
  * [enum keyword](#enum-keyword)
  * [Meta\-Operators](#meta-operators)
  * [Control flow](#control-flow)
  * [User Defined Operator](#user-defined-operator)
  * [Integer](#integer)
  * [Float](#float)
  * [Decimal](#decimal)
  * [Array](#array)
  * [String](#string)
  * [Hash](#hash)
  * [Tuple](#tuple)
  * [class](#class)
    * [inheritance and polymorphism](#inheritance-and-polymorphism)
    * [operator overloading](#operator-overloading)
    * [property(like c\#)](#propertylike-c)
    * [indexer](#indexer)
    * [static members/methods/properties](#static-membersmethodsproperties)
    * [Class Category](#class-category)
    * [Annotations](#annotations)
  * [About defer keyword](#about-defer-keyword)
  * [Concatenation of different types](#concatenation-of-different-types)
  * [Comprehensions](#comprehensions)
  * [grep and map](#grep-and-map)
  * [Function](#function)
  * [Pipe Operator](#pipe-operator)
* [Standard module introduction](#standard-module-introduction)
  * [time module](#time-module)
  * [json module(for json marshal &amp; unmarshal)](#json-modulefor-json-marshal--unmarshal)
  * [linq module](#linq-module)
* [About regular expression](#about-regular-expression)
* [Useful Utilities](#useful-utilities)

## Language Tour

### Comments

Tags support two kinds of single line comment and also block comment.

```swift
// this is a single line comment
# this is another single line comment

/* This is a 
   block comment.
*/
```

### Data Types

Tags supports 9 basic data types: `String`, `Int`, `UInt`, `Float`, `Bool`, `Array`, `Hash`, `Tuple` and `Nil`

```swift
s1 = "hello, 黄"       # strings are UTF-8 encoded
s2 = `hello, "world"`  # raw string
i = 10                 # int
u = 10u                # uint
f = 10.0               # float
b = true               # bool
a = [1, "2"]           # array
h = {"a": 1, "b": 2}   # hash
t = (1,2,3)            # tuple
n = nil
```

### Constants(Literal)

In tags, there are mainly eleven types of constants(Literals).

* Integer
* UInteger
* Float
* String
* Regular expression
* Array
* Hash
* Tuple
* Nil
* Boolean
* Function

```swift
// Integer literals
i1 = 10
i2 = 20_000_000     //for more readable
i3 = 0x80           // hex
i4 = 0b10101        // binary
i5 = 0o127          // octal

// Unsigned Integer literals
ui1 = 10u
ui2 = 20_000_000u     //for more readable
ui3 = 0x80u           // hex
ui4 = 0b10101u        // binary
ui5 = 0o127u          // octal

// Float literals
f1 = 10.25
f2 = 1.02E3
f3 = 123_456.789_012 //for more readable

// String literals
s1 = "123"
s2 = "Hello world"

// Regular expression literals
r = /\d+/.match("12")
if (r) { prinln("regex matched!") }

// Array literals
a = [1+2, 3, 4, "5", 3]

// Hash literals
h = { "a": 1, "b": 2, "c": 2}

//Tuple literals
t = (1, 2+3, "Hello", 5)

// Nil literal
n = nil

// Boolean literals
t = true
f = false

// Function literals
let f1 = add(x, y) { return x + y }
println(f1(1,2))

//fat-arrow function literals
let f2 = (x, y) => x + y
println(f2(1,2))
```

### Variables

Variables in Tags could start with the keyword `let`, or nothing with the
form `variable=value`.

```swift
let a, b, c = 1, "hello world", [1,2,3]
d = 4
e = 5
姓 = "黄"
```

You can also use `Destructuring assignment`.
Note, the left-hand side must be included using the '()'.

```swift
//righ-hand side is an array
let (d,e,f) = [1,5,8]
//d=1, e=5, f=8

//right-hand side is a tuple
let (g, h, i) = (10, 20, "hhf")
//g=10, h=20, i=hhf

//righ-hand side is a hash
let (j, k, l) = {"j": 50, "l": "good"}
//j=50, k=nil, l=good

```

Note however, if you do not use the keyword `let`, you could not do multiple variable assignments.
Below code is not correct：

```swift
//Error, multiple variable assignments must be use `let` keyword
a, b, c = 1, "hello world", [1,2,3]
```

Note：Starting from Tags 5.0，when the decalared variable already exists, it's value will be overwritten:

```swift
let x, y = 10, 20;
let x, y = y, x //Swap the value of x and y
printf("x=%v, y=%v\n", x, y)  //result: x=20, y=10
```
`let` also support the placeholder(_), when assigned a value, it will just ignore it.

```swift
let x, _, y = 10, 20, 30
printf("x=%d, y=%d\n", x, y) //result: x=10, y=30
```

### Reserved keywords

Keywords are predefined, reserved identifiers that have special meanings to the compiler. They cannot be used as identifiers. Below is a list of reserved keywords

* fn
* let
* true false nil
* if elsif elseif elif else
* unless
* return
* include
* and or
* enum
* struct # reserved, not used
* do while for break continue where
* grep map
* case is in
* try catch finally throw
* defer
* spawn
* qw
* using
* class new property set get static default
* interface public private protected # reserved, not used

### Type conversion

You can use the builtin `int()`, `uint()`, `float()`, `str()`, `array()`, `tuple()`, `hash`, `decimal` functions for type conversion.

```swift
let i = 0xa
let u = uint(i)                 // result: 10
let s = str(i)                  // result: "10"
let f = float(i)                // result: 10
let a = array(i)                // result: [10]
let t = tuple(i)                // result:(10,)
let h = hash(("key", "value"))  // result: {"key": "value}
let d = decimal("123.45634567") // result: 123.45634567
```

You could create a tuple from an array:

```swift
let t = tuple([10, 20])   //result:(10,20)
```

Similarly, you could also create an array from a tuple:

```swift
let arr = array((10,20))  //result:[10,20]
```

You could only create a hash from an array or a tuple:

```swift
//create an empty hash
let h1 = hash()  //same as h1 = {}

//create a hash from an array
let h1 = hash([10, 20])     //result: {10 : 20}
let h2 = hash([10,20,30])   //result: {10 : 20, 30 : nil}

//create a hash from a tuple
let h3 = hash((10, 20))     //result: {10 : 20}
let h4 = hash((10,20,30))   //result: {10 : 20, 30 : nil}
```

### `qw`(Quote word) keyword

The `qw` keyword is like perl's `qw` keyword. When you want to use a lot of quoted strings,
the `qw` keyword can make it a lot easier for those strings.

```swift
for str in qw<abc, def, ghi, jkl, mno> { //allowed 'qw' pair is '{}', '<>', '()'
  println('str={str}')
}

newArr = qw(1,2,3.5) //array with string values, not number values.
fmt.printf("newArr=%v\n", newArr)
```

### `enum` keyword

In tags, you can use enum to define constants.

```swift
LogOption = enum {
    Ldate         = 1 << 0,
    Ltime         = 1 << 1,
    Lmicroseconds = 1 << 2,
    Llongfile     = 1 << 3,
    Lshortfile    = 1 << 4,
    LUTC          = 1 << 5,
    LstdFlags     = 1 << 4 | 1 << 5
}

opt = LogOption.LstdFlags
println(opt)

//get all names of the `enum`
for s in LogOption.getNames() { //not ordered
    println(s)
}

//get all values of the `enum`
for s in LogOption.getValues() { //not ordered
    println(s)
}

// get a specific name of the `enum`
println(LogOption.getName(LogOption.Lshortfile))
```

### Meta-Operators
Tags has some meta-operators borrowed from perl6.
There are strict rules for meta-operators:

* Meta-operators can only operator on arrays.
* Each array's element must be number type(uint, int, float) or string type.
* If the meat-operators serve as an infix operator, and if the left and right are all arrays, they must have the same number of elements.

```swift
let arr1 = [1,2,3] ~* [4,5,6]
let arr2 = [1,2,3] ~* 4
let arr3 = [1,2,"HELLO"] ~* 2
let value1 = ~*[10,2,2]
let value2 = ~+[2,"HELLO",2]

println(arr1)   //result: [4, 10, 18]
println(arr2)   //result: [4, 8, 12]
println(arr3)   //result: [2,4,"HELLOHELLO"]
println(value1) //result: 40
println(value2) //result: 2HELLO2
```

At the moment, Tags has six meta-operators：
* <p>~+</p>
* <p>~-</p>
* <p>~*</p>
* <p>~/</p>
* <p>~%</p>
* <p>~^</p>

The six meta-operators could be served as either infix expression or prefix expression.

The meta-operator for infix expression will return an array.
The meta-operator for prefix expression will return a value(uint, int, float, string).

Below talbe give an example of meta-operator and their meanings:(only `~+` is showed):
<table>
  <tr>
    <th>Meta-Operator</td>
    <th>Expression</td>
    <th>Example</td>
    <th>Result</td>
  </tr>
  <tr>
    <td>~+</td>
    <td>Infix Expression</td>
    <td>[x1, y1, z1] ~+ [x2, y2, z2]</td>
    <td>[x1+x2, y1+y2, z1+z2] (Array)</td>
  </tr>
  <tr>
    <td>~+</td>
    <td>Infix Expression</td>
    <td>[x1, y1, z1] ~+ 4</td>
    <td>[x1+4, y1+4, z1+4] (Array)</td>
  </tr>
  <tr>
    <td>~+</td>
    <td>Prefix Expression</td>
    <td>~+[x1, y1, z1]</td>
    <td>x1+y1+z1 (Note: a value, not an array)</td>
  </tr>
</table>

### Control flow

* if/if-else/if-elif-else/if-elsif-else/if-elseif-else/if-else if-else
* unless/unless-else
* for/for-in
* while
* do
* try-catch-finally
* case-in/case-is

```swift
// if-else
let a, b = 10, 5
if (a > b) {
    println("a > b")
} elseif a == b { // could also use 'elsif', 'elseif' or 'elif'
    println("a = b")
} else {
    println("a < b")
}

//unless-else
unless b > a {
    println("a >= b")
} else {
    println("b > a")
}

// for
i = 9
for { // forever loop
    i = i + 2
    if (i > 20) { break }
    println('i = {i}')
}

i = 0
for (i = 0; i < 5; i++) {  // c-like for, '()' is a must
    if (i > 4) { break }
    if (i == 2) { continue }
    println('i is {i}')
}

i = 0
for (; i < 5; i++) {  // no initialization statement.
    if (i > 4) { break }
    if (i == 2) { continue }
    println('i is {i}')
}

i = 0
for (; i < 5;;) {  // no updater statement.
    if (i > 4) { break }
    if (i == 2) { continue }
    println('i is {i}')
    i++ // Updater statement
}

i = 0
for (;;;) {  // same as 'for { block }'
    if (i > 4) { break }
    println('i is {i}')
    i++ //update the 'i'
}

for i in range(10) {
    println('i = {i}')
}

a = [1,2,3,4]
for i in a where i % 2 != 0 {
    println(i)
}


hs = {"a": 1, "b": 2, "c": 3, "d": 4, "e": 5, "f": 6, "g": 7}
for k, v in hs where v % 2 == 0 {
    println('{k} : {v}')
}


for i in 1..5 {
    println('i={i}')
}

for item in 10..20 where $_ % 2 == 0 { // $_ is the index
    printf("idx=%d, item=%d\n", $_, item)
}


for c in "m".."a" {
    println('c={c}')
}


for idx, v in "abcd" {
    printf("idx=%d, v=%s\n", idx, v)
}


for idx, v in ["a", "b", "c", "d"] {
    printf("idx=%d, v=%s\n", idx, v)
}

for item in ["a", "b", "c", "d"] where $_ % 2 == 0 { // $_ is the index
    printf("idx=%d, item=%s\n", $_, v)
}


//for loop is an expression, not statement, so it could be assigned to a variable
let plus_one = for i in [1,2,3,4] { i + 1 }
fmt.println(plus_one)

// while
i = 10
while (i>3) {
    i--
    println('i={i}')
}

// do
i = 10
do {
    i--
    if (i==3) { break }
}

// try-catch-finally(only support string type)
let exceptStr = "SUMERROR"
try {
    let th = 1 + 2
    if (th == 3) { throw exceptStr }
} catch "OTHERERROR" {
    println("Catched OTHERERROR")
} catch exceptStr {
    println("Catched is SUMERROR")
} catch {
    println("Catched ALL")
} finally {
    println("finally running")
}

// case-in/case-is
let testStr = "123"
case testStr in { // in(exact/partial match), is(only exact match)
    "abc", "mno" { println("testStr is 'abc' or 'mno'") }
    "def"        { println("testStr is 'def'") }
    `\d+`        { println("testStr contains digit") } else         { println("testStr not matched") }
}

let i = [{"a": 1, "b": 2}, 10]
let x = [{"a": 1, "b": 2},10]
case i in {
    1, 2 { println("i matched 1, 2") }
    3    { println("i matched 3") }
    x    { println("i matched x") } else { println("i not matched anything")}
}

```

### User Defined Operator
In tags, you are free to define some operators, but you cannot
overwrite predefined operators.

> Note: Not all operators could be user defined.

Below is an example for showing how to write User Defined Operators:

```swift
//infix operator '=@' which accept two parameters.
fn =@(x, y) {
    return x + y * y
}

//prefix operator '=^' which accept only one parameter.
fn =^(x) {
    return -x
}

let pp = 10 =@ 5 // Use the '=@' user defined infix operator
printf("pp=%d\n", pp) // result: pp=35

let hh = =^10 // Use the '=^' prefix operator
printf("hh=%d\n", hh) // result: hh=-10
```

```swift
fn .^(x, y) {
    arr = []
    while x <= y {
        arr += x
        x += 2
    }
    return arr
}

let pp = 10.^20
printf("pp=%v\n", pp) // result: pp=[10, 12, 14, 16, 18, 20]
```

Below is a list of predefined operators and user defined operators:

<table>
  <tr>
    <th>Predefined Operators</td>
    <th>User Defined Operators</td>
  </tr>
  <tr>
    <td>==<br/>=~<br/>=></td>
    <td>=X</td>
  </tr>
  <tr>
    <td>++<br/>+=</td>
    <td>+X</td>
  </tr>
  <tr>
    <td>--<br/>-=<br/>-></td>
    <td>-X</td>
  </tr>
  <tr>
    <td>&gt;=<br/>&lt;&gt;</td>
    <td>&gt;X</td>
  </tr>
  <tr>
    <td>&lt;=<br/>&lt;&lt;</td>
    <td>&lt;X</td>
  </tr>
  <tr>
    <td>!=<br/>!~</td>
    <td>!X</td>
  </tr>
  <tr>
    <td>*=<br/>**</td>
    <td>*X</td>
  </tr>
  <tr>
    <td>..<br/>..</td>
    <td>.X</td>
  </tr>
  <tr>
    <td>&amp;=<br/>&amp;&amp;</td>
    <td>&amp;X</td>
  </tr>
  <tr>
    <td>|=<br/>||</td>
    <td>|X</td>
  </tr>
  <tr>
    <td>^=</td>
    <td>^X</td>
  </tr>
</table>

> In the table above, `X` could be `.=+-*/%&,|^~<,>},!?@#$`

### Integer

In tags, integer is treated as an object, so you could call it's methods.
Please see below examples:

```swift
x = (-1).next()
println(x) //0

x = -1.next() //equals 'x = -(1.next())
println(x) //-2

x = (-1).prev()
println(x) //-2

x = -1.prev() //equals 'x = -(1.prev())
println(x) //0

x = [i for i in 10.upto(15)]
println(x) //[10, 11, 12, 13, 14, 15]

for i in 10.downto(5) {
    print(i, "") //10 9 8 7 6 5

}
println()

if 10.isEven() {
    println("10 is even")
}

if 9.isOdd() {
    println("9 is odd")
}
```

### Float

In tags, float is also treated as an object, so you could call it's methods.
Please see below examples:

```swift
f0 = 15.20
println(f0)

f1 = 15.20.ceil()
println(f1)

f2 = 15.20.floor()
println(f2)
```

### Decimal

In tags, decimal is Arbitrary-precision fixed-point decimal numbers.
And the code mainly based on [decimal](https://github.com/shopspring/decimal).

Please see below examples:

```swift
d1 = decimal.fromString("123.45678901234567")  //create decimal from string
d2 = decimal.fromFloat(3)  //create decimal from float

//set decimal division precision.
//Note: this will affect all other code that follows
decimal.setDivisionPrecision(50)

fmt.println("123.45678901234567/3 = ", d1.div(d2))  //print d1/d2
fmt.println(d1.div(d2)) //same as above

fmt.println(decimal.fromString("123.456").trunc(2)) //truncate decimal

//convert string to decimal
d3=decimal("123.45678901234567")
fmt.println(d3)
fmt.println("123.45678901234567/3 = ", d3.div(d2))
```

### Array

In tags, you could use [] to initialize an empty array:

```swift
emptyArr = []
emptyArr[3] = 3 //will auto expand the array
println(emptyArr)
```

You can create an array with the given size(or length) using below two ways:

```swift
//create an array with 10 elements initialized to nil.
//Note: this only support integer literal.
let arr = []10
println(arr)

//use the builtin 'newArray' method.
let anotherArr = newArray(len(arr))
println(anotherArr) //result: [nil, nil, nil, nil, nil, nil, nil, nil, nil, nil]

let arr1 = ["1","a5","5", "5b","4","cc", "7", "dd", "9"]
let arr2 = newArray(6, arr1, 10, 11, 12) //first parameter is the size
println(arr2) //result: ["1", "a5", "5", "5b", "4", "cc", "7", "dd", "9", 10, 11, 12]

let arr3 = newArray(20, arr1, 10, 11, 12)
println(arr3) //result : ["1", "a5", "5", "5b", "4", "cc", "7", "dd", "9", 10, 11, 12, nil, nil, nil, nil, nil, nil, nil, nil]
```

Array could contain any number of different data types.
Note: the last comma before the closing ']' is optional.

```swift
mixedArr = [1, 2.5, "Hello", ["Another", "Array"], {"Name": "HHF", "SEX": "Male"}]
```

You could use index to access array element.

```swift
println('mixedArr[2]={mixedArr[2]}')
println(["a", "b", "c", "d"][2])
```

Because array is an object, so you could use the object's method to operate on it.

```swift
if ([].empty()) {
    println("array is empty")
}

emptyArr.push("Hello")
println(emptyArr)

//you could also use 'addition' to add an item to an array
emptyArr += 2
println(emptyArr)

//You could also use `<<(insertion operator)` to add item(s) to an array, the insertion operator support chain operation.
emptyArr << 2 << 3
println(emptyArr)
```

Array could be iterated using `for` loop

```swift
numArr = [1,3,5,2,4,6,7,8,9]
for item in numArr where item % 2 == 0 {
    println(item)
}

let strArr = ["1","a5","5", "5b","4","cc", "7", "dd", "9"]
for item in strArr where /^\d+/.match(item) {
    println(item)
}

for item in ["a", "b", "c", "d"] where $_ % 2 == 0 {  //$_ is the index
    printf("idx=%d, v=%s\n", $_, item)
}
```

You could also use the builtin `reverse` function to reverse array element:

```swift
let arr = [1,3,5,2,4,6,7,8,9]
println("Source Array =", arr)

revArr = reverse(arr)
println("Reverse Array =", revArr)
```


Array also support `array multiplication operator`(*):

```swift
let arr = [3,4] * 3
println(arr) // result: [3,4,3,4,3,4]
```

### String

In tags, there are three types of `string`:

* Raw string
* Double quoted string(Could not contains newline)
* Single quoted string(Interpolated String)

Raw string literals are character sequences between back quotes, as in `foo`. Within the quotes, any character may appear except back quote.

See below for some examples:

```swift
normalStr = "Hello " + "world!"
println(normalStr)

println("123456"[2])

rawStr = `Welcome to
visit us!`
println(rawStr)

//when you use single quoted string, and want variable to be interpolated,
//you just put the variable into '{}'. see below:
str = "Hello world"
println('str={str}') //output: "Hello world"
str[6]="W"
println('str={str}') //output: "Hello World"

```

In tags, strings are utf8-encoded, you could use utf-8 encoded name as a variable name.

```swift
三 = 3
五 = 5
println(三 + 五) //output : 8
```

strings are also object, so you could use some of the methods provided by `strings` module.

```swift
upperStr = "hello world".upper()
println(upperStr) //output : HELLO WORLD
```

string could also be iterated:

```swift
for idx, v in "abcd" {
    printf("idx=%d, v=%s\n", idx, v)
}

for v in "Hello World" {
    printf("idx=%d, v=%s\n", $_, v) //$_ is the index
}
```

You could concatenate an object to a string:

```swift
joinedStr = "Hello " + "World"
joinedStr += "!"
println(joinedStr)
```

You could also can use the builtin `reverse` function to reverse character of the string:

```swift
let str = "Hello world!"
println("Source Str =", str)
revStr = reverse(str)
println("Reverse str =", revStr)
```

If you hava a string, you want to convert it to number, you could add a "+" prefix before the string.

```swift
a = +"121314" // a is an int
println(a) // result: 121314

// Integer also support "0x"(hex), "0b"(binary), "0o"(octal) prefix
a = +"0x10" // a is an int
println(a) // result: 16

a = +"121314.6789" // a is a float
println(a) // result: 121314.6789
```

### Hash
In tags, the builtin hash will keep the order of keys when they are added to the hash, just like python's orderedDict.

You could use {} to initialize an empty hash:

```swift
emptyHash = {}
emptyHash["key1"] = "value1"
println(emptyHash)
```

Hash's key could be string, int, boolean:

```swift
hashObj = {
    12     : "twelve",
    true   : 1,
    "Name" : "HHF"
}
println(hashObj)
```

Note: the last comma before the closing '}' is optional.

You could use '+' or '-' to add or remove an item from a hash:

```swift
hashObj += {"key1" : "value1"}
hashObj += {"key2" : "value2"}
hashObj += {5 : "five"}
hashObj -= "key2"
hashObj -= 5
println(hash)
```

In tags, Hash is also an object, so you could use them to operate on hash object:

```swift

hashObj.push(15, "fifteen") //first parameter is the key, second is the value
hashObj.pop(15)

keys = hashObj.keys()
println(keys)

values = hashObj.values()
println(values)
```

You could also use the builtin `reverse` function to reverse hash's key and value:

```swift
let hs = {"key1": 12, "key2": "HHF", "key3": false}
println("Source Hash =", hs)
revHash = reverse(hs)
println("Reverse Hash =", revHash)
```

### Tuple

In tags, `tuple` is just like array, but it could not be changed once it has been created.

Tuples are constructed using parenthesized list notation:

```swift
//Create an empty tuple
let t1 = tuple()

//Same as above.
let t2 = ()

// Create a one element tuple.
// Note: the trailing comma is necessary to distinguish it from the
//       parenthesized expression (1).
// 1-tuples are seldom used.
let t3 = (1,)

//Create a two elements tuple
let t4 = (2,3)
```

Any object may be converted to a tuple by using the built-in `tuple` function.

```swift
let t = tuple("hello")
println(t)  // result: ("hello")
```

Like arrays, tuples are indexed sequences, so they may be indexed and sliced.
The index expression tuple[i] returns the tuple element at index i, and the slice
expression tuple[i:j] returns a subsequence of a tuple.

```swift
let t = (1,2,3)[2]
print(t) // result:3
```

Tuples are iterable sequences, so they may be used as the operand of a for-loop,
a list comprehension, or various built-in functions.

```swift
//for-loop
for i in (1,2,3) {
    println(i)
}

//tuple comprehension
let t1 =  [x+1 for x in (2,4,6)]
println(t1) //result: [3, 5, 7]. Note: Result is array, not tuple
```

Unlike arrays, tuples cannot be modified. However, the mutable elements of a tuple may be modified.

```swift
arr1 = [1,2,3]
t = (0, arr1, 5, 6)
println(t)    // result: (0, [1, 2, 3], 5, 6)
arr1.push(4)
println(t)    //result:  (0, [1, 2, 3, 4], 5, 6)
```

Tuples are hashable (assuming their elements are hashable), so they may be used as keys of a hash.

```swift
key1=(1,2,3)
key2=(2,3,4)
let ht = {key1 : 10, key2 : 20}
println(ht[key1]) // result: 10
println(ht[key2]) // result: 20

//Below is not supported(will issue a syntax error):
let ht = {(1,2,3) : 10, (2,3,4) : 20} //error!
println(ht[(1,2,3)])  //error!
println(ht[(2,3,4)])  //error!
```

Tuples may be concatenated using the + operator, it will create a new tuple.

```swift
let t = (1, 2) + (3, 4)
println(t) // result: (1, 2, 3, 4)
```

A tuple used in a Boolean context is considered true if it is non-empty.

```swift
let t = (1,)
if t {
    println("t is not empty!")
} else {
    println("t is empty!")
}

//result : "t is not empty!"
```

Tuple's json marshaling and unmarshaling will be treated as array:

```swift
let tupleJson = ("key1","key2")
let tupleStr = json.marshal(tupleJson)
//Result:[
//        "key1"，
//        "key2"，
//       ]
println(json.indent(tupleStr, "  "))

let tupleJson1 = json.unmarshal(tupleStr)
println(tupleJson1) //result: ["key1", "key2"]
```

Tuple plus an array will return an new array, not a tuple

```swift
t2 = (1,2,3) + [4,5,6]
println(t2) // result: [(1, 2, 3), 4, 5, 6]
```

You could also use the builtin `reverse` function to reverse tuples's elements:

```swift
let tp = (1,3,5,2,4,6,7,8,9)
println(tp) //result: (1, 3, 5, 2, 4, 6, 7, 8, 9)

revTuple = reverse(tp)
println(revTuple) //result: (9, 8, 7, 6, 4, 2, 5, 3, 1)
```

### class

Tags has limited support for the oop concept, below is a list of features:

* inheritance and polymorphism
* operator overloading
* property(with getter or setter or both)
* static member/method/property
* indexer
* class category
* class annotations(limited support)
* constructor method and normal methods support default value and variadic parameters

The tags parser could parse `public`, `private`, `protected`, but it has no effect in the evaluation phase.
That means tags do not support access modifiers at present.

You use `class` keyword to declare a class and use `new class(xxx)` to create an instance of a `class`.

```swift
class Animal {
    let name = ""
    fn init(name) {    //'init' is the constructor
        //do somthing
    }
}
```

In tags, all class is inherited from the root class `object`. 
`object` class include some common method like `toString()`, `instanceOf()`, `is_a()`, `classOf()`, `hashCode`.

Above code is same as:

```swift
class Animal : object {
    let name = ""
    fn init(name) {    //'init' is the constructor
        //do somthing
    }
}
```

#### inheritance and polymorphism

You can inherit a class using `:`:

```swift
class Dog : Animal { //Dog inherits from Animal
}
```

In the child class, you can use the `parent` to access parent class's members or methods.

please see below for an example：

```swift
class Animal {
    let Name;

    fn MakeNoise() {
        println("generic noise")
    }
    fn ToString() {
        return "oooooooo"
    }
}

class Cat : Animal {
    fn init(name) {
        this.Name = name
    }

    fn MakeNoise() {
        println("Meow")
    }

    fn ToString() {
        return Name + " cat"
    }
}

class Dog : Animal {
    fn init(name) {
        this.Name = name
    }

    fn MakeNoise() {
        println("Woof!")
    }

    fn ToString() {
        return Name + " dog"
    }

    fn OnlyDogMethod() {
        println("secret dog only method")
    }
}


cat = new Cat("pearl")
dog = new Dog("cole")
randomAnimal = new Animal()

animals = [cat, dog, randomAnimal]

for animal in animals
{
    println("Animal name: " + animal.Name)
    animal.MakeNoise()
    println(animal.ToString())
    if is_a(animal, "Dog") {
        animal.OnlyDogMethod()
    }
}
```

The result is:

```
Animal name: pearl
Meow
pearl cat
Animal name: cole
Woof!
cole dog
secret dog only method
Animal name: nil
generic noise
oooooooo
```

#### operator overloading

```swift
class Vector {
    let x = 0;
    let y = 0;

    // constructor
    fn init (a, b, c) {
        if (!a) { a = 0;}
        if (!b) {b = 0;}
        x = a; y = b
    }

    fn +(v) { //overloading '+'
        if (type(v) == "INTEGER" {
            return new Vector(x + v, y + v);
        } elseif v.is_a(Vector) {
            return new Vector(x + v.x, y + v.y);
        }
        return nil;
    }

    fn String() {
        return fmt.sprintf("(%v),(%v)", this.x, this.y);
    }
}

fn Vectormain() {
    v1 = new Vector(1,2);
    v2 = new Vector(4,5);
    
    // call + function in the vector object
    v3 = v1 + v2 //same as 'v3 = v1.+(v2)'
    // returns string "(5),(7)"
    println(v3.String());
    
    v4 = v1 + 10 //same as v4 = v1.+(10);
    //returns string "(11),(12)"
    println(v4.String());
}

Vectormain()
```

#### property(like c#)

```swift
class Date {
    let month = 7;  // Backing store
    property Month
    {
        get { return month } set {
            if ((value > 0) && (value < 13)) {
                month = value
            } else {
               println("BAD, month is invalid")
            }
        }
    }

    property Year; // same as 'property Year { get; set;}'

    property Day { get; }

    property OtherInfo1 { get; }
    property OtherInfo2 { set; }

    fn init(year, month, day) {
        this.Year = year
        this.Month = month
        this.Day = day
    }

    fn getDateInfo() {
        printf("Year:%v, Month:%v, Day:%v\n", this.Year, this.Month, this.Day) //note here, you need to use 'this.Property', not 'Property'
    }
}

dateObj = new Date(2000, 5, 11)
//printf("Calling Date's getter, month=%d\n", dateObj.Month)
dateObj.getDateInfo()

println()
dateObj.Month = 10
printf("dateObj.Month=%d\n", dateObj.Month)

dateObj.Year = 2018
println()
dateObj.getDateInfo()

//Below code will raise an execution error! Because OtherInfo1 is a READONLY property.
//dateObj.OtherInfo1 = "Other Date Info"
//println(dateObj.OtherInfo1)

//Below code will raise an execution error! Because OtherInfo2 is a WRITEONLY property.
//dateObj.OtherInfo2 = "Other Date Info2"
//println(dateObj.OtherInfo2)

//Below code will raise an execution error! Because Day is a READONLY property.
//dateObj.Day = 18
```

#### indexer

Tags has support for class `indexer`(like c#). 
An indexer is a member that enables an object to be indexed in the same way as an array.

You declare an Indexer using `property this[parameter]`.

```swift
property this[index] {
    get { xxx } set { xxx }
}
```

Please see the example code:

```swift
class IndexedNames {
    let namelist = []
    let size = 10
    fn init() {
        let i = 0
        for (i = 0; i < size; i++) {
            namelist[i] = "N. A."
        }
    }

    fn getNameList() {
        println(namelist)
    }

    property this[index] {
        get {
            let tmp;
            if ( index >= 0 && index <= size - 1 ) {
               tmp = namelist[index]
            } else {
               tmp = ""
            }
     
            return tmp
         } set {
             if ( index >= 0 && index <= size-1 ) {
                 namelist[index] = value
             }
         }
    }
}

fn Main() {
    namesObj = new IndexedNames()

    //Below code will call Indexer's setter function
    namesObj[0] = "Zara"
    namesObj[1] = "Riz"
    namesObj[2] = "Nuha"
    namesObj[3] = "Asif"
    namesObj[4] = "Davinder"
    namesObj[5] = "Sunil"
    namesObj[6] = "Rubic"

    namesObj.getNameList()

    for (i = 0; i < namesObj.size; i++) {
        println(namesObj[i]) //Calling Indexer's getter function
    }
}

Main()
```

#### static members/methods/properties

```swift
class Test {
   static let x = 0;
   static let y = 5;

   static fn Main() {
      println(Test.x);
      println(Test.y);

      Test.x = 99;
      println(Test.x);
   }
}

Test.Main()
```

Note：Non-static variable/method/property could access static variable/method/property.
      On the other hand, static variable/method/property cannot access Non-static variable/method/property.

#### Class Category

Tags also support class Category like objective-c（C# is called 'extension methods'）.

```swift
class Animal {
    fn Walk() {
        println("Animal Walk!")
    }
}

//Class category like objective-c
class Animal (Run) { //Create an 'Run' category of Animal class.
    fn Run() {
        println("Animal Run!")
        this.Walk() //can call Walk() method of Animal class.
    }
}

animal = new Animal()
animal.Walk()

println()
animal.Run()
```

#### Annotations

Tags also has very simple annotation support like java：

* Only mehotd and property of class can have annotations(not class itself, or other simple functions)
* In the body of `Annotation` class, only support property, do not support methods.
* When use annotations, you must create an object.


You could use `class @annotationName {}` to declare an annotation class.
Tags also include some builtin annotations:

* @Override annotation(just like java's @Override).
* @NotNull
* @NotEmpty

Please see below example：

```swift

//Declare annotation class
//Note: In the body, you must use property, not method.
class @MinMaxValidator {
  property MinLength
  property MaxLength default 10 //Same as 'property MaxLength = 10'
}

//This is a marker annotation
class @NoSpaceValidator {}

class @DepartmentValidator {
  property Department
}

//The 'Request' class
class Request {
  @MinMaxValidator(MinLength=1)
  property FirstName; // getter and setter is implicit

  @NoSpaceValidator
  property LastName;

  @DepartmentValidator(Department=["Department of Education", "Department of Labors", "Department of Justice"])
  property Dept;
}

//This class is responsible for processing the annotation.
class RequestHandler {
  static fn handle(o) {
    props = o.getProperties()
    for p in props {
      annos = p.getAnnotations()
      for anno in annos {
        if anno.instanceOf(MinMaxValidator) {
          //p.value is the property real value.
          if len(p.value) > anno.MaxLength || len(p.value) < anno.MinLength {
            printf("Property '%s' is not valid!\n", p.name)
          }
        } elseif anno.instanceOf(NoSpaceValidator) {
          for c in p.value {
            if c == " " || c == "\t" {
              printf("Property '%s' is not valid!\n", p.name)
              break
            }
          }
        } elseif anno.instanceOf(DepartmentValidator) {
          found = false
          for d in anno.Department {
            if p.value == d {
              found = true
            }
          }
          if !found {
            printf("Property '%s' is not valid!\n", p.name)
          }
        }
      }
    }
  }
}

class RequestMain {
  static fn main() {
    request = new Request()
    request.FirstName = "Haifeng123456789"
    request.LastName = "Huang     "
    request.Dept = "Department of Justice"
    RequestHandler.handle(request)
  }
}

RequestMain.main()
```

Below is the result：

```
Property 'FirstName' is not valid!
Property 'LastName' is not valid!
```

### About `defer` keyword

A defer statement defers the execution of a function until the surrounding function returns.

The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

```swift
let add = fn(x, y) {
    defer println("I'm defer1")
    println("I'm in add")
    defer println("I'm defer2")
    return x + y
}
println(add(2,2))
```

The result is as below:

```sh
I'm in add
I'm defer2
I'm defer1
4
```

### Concatenation of different types

In tags, you could concatenate of different types. See below for examples:

```swift
// Number plus assignment
num = 10
num += 10 + 15.6
num += 20
println(num)

// String plus assignment
str = "Hello "
str += "world! "
str += [1, 2, 3]
println(str)

// Array plus assignment
arr = []
arr += 1
arr += 10.5
arr += [1, 2, 3]
arr += {"key": "value"}
println(arr)

// Array compare
arr1 = [1, 10.5, [1, 2, 3], {"key" : "value"}]
println(arr1)
if arr == arr1 { //support ARRAY compare
    println("arr1 = arr")
} else {
    println("arr1 != arr")
}

// Hash assignment("+=", "-=")
hash = {}
hash += {"key1" : "value1"}
hash += {"key2" : "value2"}
hash += {5 : "five"}
println(hash)
hash -= "key2"
hash -= 5
println(hash)
```

### Comprehensions

Tags support list(array,string, range, tuple) comprehensions.
list comprehension will return an array.
please see following examples:

```swift
//array comprehension
x = [[word.upper(), word.lower(), word.title()] for word in ["hello", "world", "good", "morning"]]
println(x) //result: [["HELLO", "hello", "Hello"], ["WORLD", "world", "World"], ["GOOD", "good", "Good"], ["MORNING", "morning", "Morning"]]

//string comprehension (here string is treated like an array)
y = [ c.upper() for c in "huanghaifeng" where $_ % 2 != 0] //$_ is the index
println(y) //result: ["U", "N", "H", "I", "E", "G"]

//range comprehension
w = [i + 1 for i in 2..10]
println(w) //result: [2, 3, 4, 5, 6, 7, 8, 9, 10, 11]

//tuple comprehension
v = [x+1 for x in (12,34,56)]
println(v) //result: [13, 35, 57]

//hash comprehension
z = [v * 10 for k,v in {"key1": 10, "key2": 20, "key3": 30}]
println(z) //result: [100, 200, 300]
```

Tags also support hash comprehension.
hash comprehension will return a hash.
please see following examples:

```swift
//hash comprehension (from hash)
z1 = { v:k for k,v in {"key1": 10, "key2": 20, "key3": 30}} //reverse key-value pair
println(z1) // result: {10 : "key1", 20 : "key2", 30 : "key3"}. Order may differ

//hash comprehension (from array)
z2 = {x:x**2 for x in [1,2,3]}
println(z2) // result: {1 : 1, 2 : 4, 3 : 9}. Order may differ

//hash comprehension (from .. range)
z3 = {x:x**2 for x in 5..7}
println(z3) // result: {5 : 25, 6 : 36, 7 : 49}. Order may differ

//hash comprehension (from string)
z4 = {x:x.upper() for x in "hi"}
println(z4) // result: {"h" : "H", "i" : "I"}. Order may differ

//hash comprehension (from tuple)
z5 = {x+1:x+2 for x in (1,2,3)}
println(z5) // result: {4 : 5, 2 : 3, 3 : 4}. Order may differ
```

### grep and map

The `grep` and `map` operators are just like perl's `grep` and `map`.

The grep operator takes a list of values and a "testing expression." For each item in the list of values,
the item is placed temporarily into the $_ variable, and the testing expression is evaluated. If the
expression results in a true value, the item is considered selected.

The map operator has a very similar syntax to the grep operator and shares a lot of the same operational steps.
For example, items from a list of values are temporarily placed into $_ one at a time. However,
the testing expression becomes a mapping expression.

```swift
let sourceArr = [2,4,6,8,10,12]

let m = grep  $_ > 5, sourceArr
println('m is {m}')

let cp = map $_ * 2 , sourceArr
println('cp is {cp}')

//a little bit more complex example
let fields = {
                "animal"   : "dog",
                "building" : "house",
                "colour"   : "red",
                "fruit"    : "apple"
             }
let pattern = `animal|fruit`
// =~(match), !~(unmatch)
let values = map { fields[$_] } grep { $_ =~ pattern } fields.keys()
println(values)
```

### Function

Function in tags is a first-class object. This means the language supports passing functions as arguments to
other functions, returning them as the values from other functions, and assigning them to variables or storing
them in data structures.

Function also could have default parameters and variadic parameters.

```swift
//define a function
let add = fn() { [5,6] }
let n = [1, 2] + [3, 4] + add()
println(n)


let complex = {
   "add" : fn(x, y) { return fn(z) {x + y + z } }, //function with closure
   "sub" : fn(x, y) { x - y },
   "other" : [1,2,3,4]
}
println(complex["add"](1, 2)(3))
println(complex["sub"](10, 2))
println(complex["other"][2])


let warr = [1+1, 3, fn(x) { x + 1}(2),"abc","def"]
println(warr)


println("\nfor i in 5..1 where i > 2 :")
for i in fn(x){ x+1 }(4)..fn(x){ x+1 }(0) where i > 2 {
  if (i == 3) { continue }
  println('i={i}')
}


// default parameter and variadic parameters
add = fn (x, y=5, z=7, args...) {
    w = x + y + z
    for i in args {
        w += i
    }
    return w
}

w = add(2,3,4,5,6,7)
println(w)
```

You could also declare named function like below:

```swift
fn sub(x,y=2) {
    return x - y
}
println(sub(10)) //output : 8
```

You could also create a function using the `fat arrow` syntax:

```swift
let x = () => 5 + 5
println(x())  //result: 10

let y = (x) => x * 5
println(y(2)) //result: 10

let z = (x,y) => x * y + 5
println(z(3,4)) //result :17


let add = fn (x, factor) {
  x + factor(x)
}
result = add(5, (x) => x * 2)
println(result)  //result : 15
```

If the function has no parameter, then you could omit the parentheses. e.g.

```swift
println("hhf".upper)  //result: "HHF"
//Same as above
println("hhf".upper())
```

Tags support multiple return values using 'let'.
The returned values are wrapped as a tuple.

```swift
fn testReturn(a, b, c, d=40) {
    return a, b, c, d
}

let (x, y, c, d) = testReturn(10, 20, 30)
// let x, y, c, d = testReturn(10, 20, 30)  same as above

printf("x=%v, y=%v, c=%v, d=%v\n", x, y, c, d)
//Result: x=10, y=20, c=30, d=40
```

Note：You must use `let` to support multiple return values, below statement will issue a compile error.

```swift
(x, y, c, d) = testReturn(10, 20, 30) // no 'let', compile error
x, y, c, d = testReturn(10, 20, 30)   // no 'let', compile error
```

### Pipe Operator

The pipe operator, inspired by [Elixir](https://elixir-lang.org/).
And thanks for the project [Aria](https://github.com/fadion/aria), I got the idea and some code from this project.

See below for examples:

```swift
# Test pipe operator(|>)
x = ["hello", "world"] |> strings.join(" ") |> strings.upper() |> strings.lower() |> strings.title()
printf("x=<%s>\n", x)

let add = fn(x,y) { return x + y }
let pow = fn(x) { return x ** 2}
let subtract = fn(x) { return x - 1}

let mm = add(1,2) |> pow() |> subtract()
printf("mm=%d\n", mm)

"Hello %s!\n" |> fmt.printf("world")
```

## Standard module introduction

In tags, there are some standard modules provided for you: json, sort, time
This is a brief introduction of some of the tags standard modules, don't expect it to be thorough.
If you are curious, please read the language documentation.

#### time module

```swift
t1 = newTime()
format = t1.strftime("%F %R")
println(t1.toStr(format))
Epoch = t1.toEpoch()
println(Epoch)

t2 = t1.fromEpoch(Epoch)
println(t2.toStr(format))
```

#### json module(for json marshal & unmarshal)

```swift
let hsJson = {"key1" : 10,
              "key2" : "Hello Json %s %s Module",
              "key3" : 15.8912,
              "key4" : [1,2,3.5, "Hello"],
              "key5" : true,
              "key6" : {"subkey1": 12, "subkey2": "Json"},
              "key7" : fn(x,y){x+y}(1,2)
}
let hashStr = json.marshal(hsJson) //same as `json.toJson(hsJson)`
println(json.indent(hashStr, "  "))

let hsJson1 = json.unmarshal(hashStr)
println(hsJson1)


let arrJson = [1,2.3,"HHF",[],{ "key" : 10, "key1" : 11}]
let arrStr = json.marshal(arrJson)
println(json.indent(arrStr))
let arr1Json = json.unmarshal(arrStr)  //same as `json.fromJson(arrStr)`
println(arr1Json)
```

#### linq module

In tags, the `linq` module support four types of object:

* String object
* Array object
* Tuple object
* Hash object

```swift
let mm = [1,2,3,4,5,6,7,8,9,10]
println('before mm={mm}')

result = linq.from(mm).where(fn(x) {
    x % 2 == 0
}).select(fn(x) {
    x = x + 2
}).toSlice()
println('after result={result}')

result = linq.from(mm).where(fn(x) {
    x % 2 == 0
}).select(fn(x) {
    x = x + 2
}).last()
println('after result={result}')

let sortArr = [1,2,3,4,5,6,7,8,9,10]
result = linq.from(sortArr).sort(fn(x,y){
    return x > y
})
println('[1,2,3,4,5,6,7,8,9,10] sort(x>y)={result}')

result = linq.from(sortArr).sort(fn(x,y){
    return x < y
})
println('[1,2,3,4,5,6,7,8,9,10] sort(x<y)={result}')

thenByDescendingArr = [
    {"Owner" : "Google",    "Name" : "Chrome"},
    {"Owner" : "Microsoft", "Name" : "Windows"},
    {"Owner" : "Google",    "Name" : "GMail"},
    {"Owner" : "Microsoft", "Name" : "VisualStudio"},
    {"Owner" : "Google",    "Name" : "GMail"},
    {"Owner" : "Microsoft", "Name" : "XBox"},
    {"Owner" : "Google",    "Name" : "GMail"},
    {"Owner" : "Google",    "Name" : "AppEngine"},
    {"Owner" : "Intel",     "Name" : "ParallelStudio"},
    {"Owner" : "Intel",     "Name" : "VTune"},
    {"Owner" : "Microsoft", "Name" : "Office"},
    {"Owner" : "Intel",     "Name" : "Edison"},
    {"Owner" : "Google",    "Name" : "GMail"},
    {"Owner" : "Microsoft", "Name" : "PowerShell"},
    {"Owner" : "Google",    "Name" : "GMail"},
    {"Owner" : "Google",    "Name" : "GDrive"}
]

result = linq.from(thenByDescendingArr).orderBy(fn(x) {
    return x["Owner"]
}).thenByDescending(fn(x){
    return x["Name"]
}).toOrderedSlice()    //Note: You need to use toOrderedSlice

//use json.indent() for formatting the output
let thenByDescendingArrStr = json.marshal(result)
println(json.indent(thenByDescendingArrStr, "  "))

//test 'selectManyByIndexed'
println()
let selectManyByIndexedArr1 = [[1, 2, 3], [4, 5, 6, 7]]
result = linq.from(selectManyByIndexedArr1).selectManyByIndexed(
fn(idx, x){
    if idx == 0 { return linq.from([10, 20, 30]) }
    return linq.from(x)
}, fn(x,y){
    return x + 1
})
println('[[1, 2, 3], [4, 5, 6, 7]] selectManyByIndexed() = {result}')

let selectManyByIndexedArr2 = ["st", "ng"]
result = linq.from(selectManyByIndexedArr2).selectManyByIndexed(
fn(idx,x){
    if idx == 0 { return linq.from(x + "r") }
    return linq.from("i" + x)
},fn(x,y){
    return x + "_"
})
println('["st", "ng"] selectManyByIndexed() = {result}')
```

## About regular expression

In tags, regard to regular expression, you could use:

* Regular expression literal
* 'regexp' module
* =&#126; and !&#126; operators(like perl's)

```swift
//Use regular expression literal( /pattern/.match(str) )
let regex = /\d+\t/.match("abc 123	mnj")
if (regex) { println("regex matched using regular expression literal") }

//Use 'regexp' module
if regexp.compile(`\d+\t`).match("abc 123	mnj") {
    println("regex matched using 'regexp' module")
}

//Use '=~'(str =~ pattern)
if "abc 123	mnj" =~ `\d+\t` {
    println("regex matched using '=~'")
}else {
    println("regex not matched using '=~'")
}

```

```sh
Note: For detailed explanation of 'Regular Expression' pattern matching, you could see golang's regexp module for reference.
```
