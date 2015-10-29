# Swift — Control Flow

## Objectives

## Control Flow

In the context of programming, [Control Flow](https://en.wikipedia.org/wiki/Control_flow) refers to code structures that instruct the program on how to make decisions based upon various conditions; they are a way to define dynamic behavior for a variety of situations.

A good metaphor for visualizing control flow is to imagine the branching tracks in a train-yard. Different control flow statements are analogous to different structures in the train tracks.

![](trainyard)  
— *Beacon Park Yard in Boston*, 2012, by [Fletcher6](https://commons.wikimedia.org/wiki/File:Beacon_Park_Rail_Yard.jpg), Wikipedia [CC-BY 3.0](https://creativecommons.org/licenses/by-sa/3.0/deed.en)

This reading will discuss the usage of the code structures detailed in the [Control Flow chapter](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ControlFlow.html#//apple_ref/doc/uid/TP40014097-CH9-ID120) of Apple's *The Swift Programming Language*.

## Conditionals

Conditionals are a general category of structures that, well, test a condition. The program is instructed to make an evaluation and then make a selection from different behavior options based upon the result of that evaluation. Conditionals rely on boolean values, whether saved as a named instance or evaluated directly from the result of an operation, function, or method.

### `Bool`

Swift's type for booleans is `Bool` (capitalized) which can hold the values `true` and `false` (lowercase). 

```swift
let truthy = true
let falsy = false
``` 

### `if`, `else`, and `else if`

The `if`, `else`, and `else if` statements are available in Swift for building logic trees.

**Objective-C:** *The syntax for these structures will feel familiar with the simple omission of the parenthesis around the conditional.*

```swift
if true {
    // this code will run
}
```

The `else` statement can be used to follow an `if` block to define a default behavior:

```swift
if false {
    // will not run
} else {
    // default behavior will run
}
```

The `else if` statements can be used to add more branching options in hierarchical way. An `else if` statement will only have the opportunity to run if *all* of the `if` and `else if` statements preceding it in its block fail in their evaluations:

```swift
if false {
    // will not run
} else if true {
    // this code will run
} else if true {
    // will not evaluate
} else {
    // will not run
}
```

```swift
if true {
    // this code will run
} else if true {
    // will not evaluate
}    
```

For example, we could implement an `if` block to define behaviors based on the value of an integer:

```swift
let number = 1

if number > 0 {
    print("\(number) is a positive number.")
} else if number < 0 {
    print("\(number) is a negative number.")
} else {
    print("\(number) is zero.")
}
```

If we were then to add a special condition for the exact value of `1`, we would have to be careful about the order in which we make the evaluations. If we made this evaluation *after* the `number > 0` evaluation, the state we are checking will get lumped into that behavior:

```swift
let number = 1

if number > 0 {
    print("\(number) is a positive number.")
} else if number == 1 {            // will never evaluate
    print("\(number) is the loneliest number!")
} else if number < 0 {
    print("\(number) is a negative number.")
} else {
    print("\(number) is zero.")
}
```

There are two ways that we can solve this, either we can filter the preceding condition for our special case:

```swift
let number = 1

if number > 0 && number != 1 {
    print("\(number) is a positive number.")
} else if number == 1 {
    print("\(number) is the loneliest number!")
} else if number < 0 {
    print("\(number) is a negative number.")
} else {
    print("\(number) is zero.")
}
```
Or, we can change the order of our evaluations, which is the cleaner option:

```swift
let number = 1

if number == 1 {
    print("\(number) is the loneliest number!")
} else if number > 0 {
    print("\(number) is a positive number.")
} else if number < 0 {
    print("\(number) is a negative number.")
} else {
    print("\(number) is zero.")
}
```

This will print: `1 is the loneliest number!`.

### `switch`

Beyond more than a handful of branches, an `if` block starts to get unwieldy and less legible. However, Swift also provides a `switch` statement that provides a cleaner syntax for branched decision-making based on the various potential states of a *single* instance.

**Objective-C:** *Swift's* `switch` *statement overrides C's* `switch` *to permit use with objects. This is in contrast to working in Objective-C which accesses the C version directly that can only be used to evaluate primitive values.*

A `switch` statement is opened by using the keyword `switch` followed by the name of the instance to be evaluated. The curly braces define the scope of the `switch` statement and must contain the `case` statements. A `default` statement is permitted at the very end to catch anything that does not get caught by the `case` statements.

```swift
let people = 3

switch people {
case 2:
    print("Two's a company.")
case 3:
    print("Three's a crowd.")
case 4:
    print("Four's a party.")
case 5:
    print("Five's not allowed.")
default:
	print("\(people) is not a part of the rhyme.")
}
```
This will print: `Three's a crowd.`.

Swift's `switch` statement does **not** permit fallthrough by default, but *does* allow multiple values to be defined for a single `case` statement, and allows ranges (defined by two numbers joined by an ellipsis `...`) to be used for number evaluations.

```swift
let number = 42

switch number {
case 0, 1, 2, 3, 4, 5, 6, 7, 8, 9:
    print("\(number) is in the digits")
case 10...99:
    print("\(number) is in the tens")
case 100...999:
    print("\(number) is in the hundreds")
case 1000...9999:
    print("\(number) is in the thousands")
default:
	print("\(number) is either really big or a negative number")
}
```
This will print: `42 is in the tens`.

Since Swift's `switch` statement is capable of evaluating objects, it can be used with strings:

```swift
let name = "Tim"

switch name {
case "Tim":
    print("\(name) is the Lead Instructor.")
case "Joe":
    print("\(name) is the Director of Faculty.")
case "Jim", "Tom", "Mark":
    print("\(name) is an Assistant Instructor.")
default:
    print("\(name) is probably a student.")
}
```
This will print: `Tim is the Lead Instructor.`

## Loops

Loops are a programming construct which allow the code within them to be executed repeatedly so long as a condition (or set of conditions) remains valid. In iOS Development, loops are primarily used in conjunction with collections (such as arrays and dictionaries) in order to enact behavior upon their contents.

### Implicitly-typed Arrays

Swift's array literal employs the square brackets `[` `]` to wrap its initial object list separated by commas. When declaring an instance instance with `let` or `var`, that instance will be implicitly typed as an array if it assigned to an array literal. 

```swift
let numbers = [1, 2, 3, 4, 5]
let vowels = ["a", "e", "i", "o", "u"]
```
Because Swift is a statically-typed language, collections are given knowledge of the kinds of instances that they contain. The compiler enforces inserting any instances into the collection which do not conform to the specified type. We'll discuss this concept in more detail in the future, but for the time being be aware that the arrays you create will be implicitly-typed and may not permit you insert a new instance of a different type.

```swift
// implicitly creates an array of type Int
let numbers = [1, 2, 3, 4, 5]

// implicitly creates an array of type String
let vowels = ["a", "e", "i", "o", "u"]
```
**Important:** Avoid creating an empty implicitly-typed array. Swift 2.1 will make it an `NSArray` which is the Objective-C array class that will **not** respond to Swift's Array method calls.

```swift
// implicitly creates an NSArray (avoid this)
let empty = []
```

#### Subscripting

Arrays can be subscripted with an integer value or variable using square brackets as a suffix:

```swift
let three = numbers[2]
let e = vowels[1]
```
In order to add or change an element in an array, it must have been declared using `var`. An array declared with `let` cannot be changed. This is similar to Objective-C's distinction between `NSArray` (i.e. using `let`) and `NSMutableArray` (i.e. using `var`).

```swift
// will generate errors on constant arrays declared with let
numbers[0] = 0
vowels.append("y")
```
![](screenshot of error)

Instead, declare the arrays using `var` in order to mutate them:

```swift
var numbers = [1, 2, 3, 4, 5]
var vowels = ["a", "e", "i", "o", "u"]

numbers[0] = 9      // the first object with 9
vowels.append("y")  // adds "y" to the end of the array
```

##### The `count` Property

Arrays have a (read-only) `count` property which holds an integer value of the number of objects held in the array that can be accessed with dot notation. **An array's `count` will always be one more than its maximum index.**

```swift
print(numbers.count)
print(vowels.count)
```

### The `for-in` Loop

A `for-in` loop can be used to step through everything in a given sequence, either a range or a collection such as an array or a dictionary.

```swift
for number in 1...5 {
    print("The square of \(number) is \(number * number).")
}
```
This will print:

```
The square of 1 is 1.
The square of 2 is 4.
The square of 3 is 9.
The square of 4 is 16.
The square of 5 is 25.
```

```swift
let names = ["Tim", "Joe", "Jim", "Tom", "Mark"]

for name in names {
    print("\(name) works at the Flatiron School.")
}
```
This will print:

```
Tim works at the Flatiron School.
Joe works at the Flatiron School.
Jim works at the Flatiron School.
Tom works at the Flatiron School.
Mark works at the Flatiron School.
```

### The `for` Loop

### The `while` and `repeat-while` Loops