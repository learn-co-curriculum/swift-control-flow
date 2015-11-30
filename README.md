# Swift — Control Flow

## Objectives

1. Understand the category of control flow statements as decision-making instructions built into a program.
2. Use `if`, `else`, and `else if` statements to build a logic tree.
3. Use Swift's `switch` statement with integers, ranges, and objects.
4. Get introduced to creating, accessing, and modifying implicitly-typed arrays.
5. Use a `for-in` loop to iterate over every element in a sequence.
6. Use a `for` loop to iterate through a sequence in a way that requires a counter.
7. Recognize the syntax for `while` and `repeat-while` loops.

## Control Flow

In the context of programming, [Control Flow](https://en.wikipedia.org/wiki/Control_flow) refers to code structures that make decisions based upon various conditions; they are a way to define dynamic behavior for a variety of situations.

A good metaphor for visualizing control flow is to imagine the branching tracks in a train-yard. Different control flow statements are analogous to different structures in the train tracks.

![](https://curriculum-content.s3.amazonaws.com/swift/swift-control-flow/1024px-Beacon_Park_Rail_Yard.jpg)  
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

The `else` statement can be used to follow an `if` block to define behavior if the original condition is false:

```swift
if false {
    // will not run
} else {
    // will run
}
```

The `else if` statements can be used to add more branching options in a hierarchical way. An `else if` statement will only have the opportunity to run if *all* of the `if` and `else if` statements preceding it in its tree fail in their evaluations:

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

**Objective-C:** *Swift's* `switch` *statement allows comparing objects. This is in contrast to Objective-C, where* `switch` *can only be used with primitives.*

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

Swift's `switch` statement does **not** "fall through" by default (i.e., you don't need `break`s at the end of each `case`), but *does* allow multiple values to be defined for a single `case` statement, and allows ranges (defined by two numbers joined by an ellipsis `...`) to be used for number comparisons.

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

Swift's array literal employs the square brackets `[` `]` to wrap its initial object list separated by commas. When declaring an instance with `let` or `var`, that instance will be implicitly typed as an array if it assigned to an array literal. 

```swift
let numbers = [1, 2, 3, 4, 5]
let vowels = ["a", "e", "i", "o", "u"]
```

Swift collections have knowledge of the types of the objects they contain. The compiler disallows inserting mismatched types into the same array. We'll discuss this concept in more detail in the future, but for the time being be aware that the arrays you create will be implicitly-typed and may not permit you to insert a new instance of a different type.

```swift
// implicitly creates an array of type Int
let numbers = [1, 2, 3, 4, 5]

// implicitly creates an array of type String
let vowels = ["a", "e", "i", "o", "u"]
```

**Important:** *Avoid creating an empty implicitly-typed array. Swift 2.1 will make it an* `NSArray` *which is the Objective-C array class that will* ***not*** *respond to Swift's Array method calls.*

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
// will generate errors on immutable arrays declared with let
numbers[0] = 0
vowels.append("y")
```

![](https://curriculum-content.s3.amazonaws.com/swift/swift-control-flow/error_mutating_arrays_made_with_let.png)

Instead, declare the arrays using `var` in order to mutate them:

```swift
var numbers = [1, 2, 3, 4, 5]
var vowels = ["a", "e", "i", "o", "u"]

numbers[0] = 9      // the first object with 9
vowels.append("y")  // adds "y" to the end of the array
```

##### The `count` Property

Arrays have a (read-only) `count` property which holds an integer value of the number of objects held in the array; this property can be accessed with dot notation. **An array's `count` will always be one more than its maximum index.**

```swift
print(numbers.count)
print(vowels.count)
```

### The `for-in` Loop

A `for-in` loop can be used to step through everything in a given sequence, either a range or a collection such as an array or a dictionary.

```swift
for <element> in <sequence> {
    <statements>
}
```

An implementation that steps through the integers in a range might look like this:

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

Iterating over an array of strings might look like this:

```swift
let names = ["Joe", "Tim", "Jim", "Tom", "Mark"]

for name in names {
    print("\(name) works at the Flatiron School.")
}
```

This will print:

```
Joe works at the Flatiron School.
Tim works at the Flatiron School.
Jim works at the Flatiron School.
Tom works at the Flatiron School.
Mark works at the Flatiron School.
```

### The `for` Loop

The `for` loop in Swift provides context for defining the familiar counter, conditional, and increment statements which must be separated by semicolons (`;`). Objective-C developers will find this syntax familiar, with the exception of omitting the parenthesis on the opening line:

```swift
for <initialization>; <condition>; <increment> {
    <statements>
}
```

Though `for-in` loops provide a cleaner syntax when working with whole sequences, a `for` loop provides a structured syntax whenever a counter variable within the loop's implementation is necessary, such as when accessing synchronized values across more than one array:

```swift
let firstNames = ["Joe", "Tim", "Jim", "Tom", "Mark"]
let lastNames = ["Burgess", "Clem", "Campagno", "O'Malley", "Murray"]

for var i = 0; i < firstNames.count; i++ {
    print("\(firstNames[i]) \(lastNames[i]) works at the Flatiron School.")
}
``` 

This will print:

```swift
Joe Burgess works at the Flatiron School.
Tim Clem works at the Flatiron School.
Jim Campagno works at the Flatiron School.
Tom O'Malley works at the Flatiron School.
Mark Murray works at the Flatiron School.
```

### The `while` and `repeat-while` Loops

These "simple" loops are synonymous to the `while` and `do-while` loops in Objective-C. These loops differ from `for` and `for-in` loops in that they do not safeguard against not defining a limitation; **it is easy to accidentally write infinite loops with these loop types.**

The primary difference between them is that the `while` loop evaluates its condition before the loop executes, but a `repeat-while` loop evaluates its condition *after* the first iteration of the loop. If you want to ensure that the loop always runs at least once, a `repeat-while` loop may be your best option.

##### The `while` loop:

```swift
while <condition> {
    <statements>
}
```

We can use a `while` loop to generate a string of letters of a certain length:

```swift
var eek = "";

while eek.characters.count < 10 {
    eek += "e"
}
eek += "k!"

print(eek)
```

This will print: `eeeeeeeeeek!`.

##### The `repeat-while` loop:

```swift
repeat {
    <statements>
} while <condition>
```

We can use a `repeat-while` loop to write a dice roller that will continue totaling new rolls until a `1` is rolled:

```swift
var roll = 0;
var total = 0;

repeat {
    roll = 1 + Int(arc4random_uniform(6));
    total += roll
    print("Rolled a \(roll)")
} while roll > 1

print("Total score: \(total)")
```

This may print:

```
Rolled a 4
Rolled a 3
Rolled a 1
Total score: 8
```

However, if you find yourself needing to create a counter variable, it is recommended that you convert your loop to a `for` loop which explicitly includes it in its opening syntax:

```swift
var grammarQuirk = ""

for var i = 0; i < 8; i++ {
    var buffalo = "buffalo"
    if i == 0 || i == 2 || i == 6 {
        buffalo = buffalo.capitalizedString
    }
    grammarQuirk += buffalo
    
    if i == 7 {
        grammarQuirk += "."
    } else {
        grammarQuirk += " "
    }
}
print(grammarQuirk)
```

This will print:

```
Buffalo buffalo Buffalo buffalo buffalo buffalo Buffalo buffalo.
```

### Trainyard Express

As a tie-in to the trainyard metaphor, take a look at the [Trainyard Express (free)](http://trainyard.ca/aboutExpress) and Trainyard (full version) games written by [Matt Rix](https://github.com/MattRix) back in 2010. It's still available on the iOS App Store (and on Android for anyone without an iPhone). Matt also wrote a [history of his development process](http://struct.ca/2010/the-story-so-far/) for the game that he has hosted on his website.
