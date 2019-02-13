# Quick Intro to Swift

## Basics

### Constants and variables

Use `var` to set variables; use `let` to set constants.

Constants or variables must have the same type as the assigned value. However, typing is inferred so you don’t always have to explicitly specify the type.

Values are never implicitly converted, you must explicitly make an instance of the desired type. You can denote a variable optional when typing with a `?`

```swift
var variable = 10
let constant = 10
variable = 20
constant = 20    /// Cannot assign to value: 'constant' is a 'let' constant

let implicitInteger = 70
let implicitDouble = 70.0
let explicitDouble: Double = 70
let explicitFloat: Float = 70
var optionalInteger: Integer?

let label = "The width is "
let width = 94
let labelWithWidth = label + String(width)
```

### String Interpolation

```swift
let apples = 3
let oranges = 5
let appleSummary = "I have \(apples) apples."
let fruitSummary = "I have \(apples + oranges) pieces of fruit."
```

### Multiline strings

```swift
let quotation = """
    Even though there's whitespace to the left,
    the actual lines aren't indented.
        Except for this line.
    Double quotes (") can appear without being escaped.

    I still have \(apples + oranges) pieces of fruit.
    """
            // <-- Note the position of the closing quotes

> Even though there's whitespace to the left,
> the actual lines aren't indented.
>     Except for this line.
> Double quotes (") can appear without being escaped.
```

### Arrays and Dictionaries

Create arrays and dictionaries with `[]` brackets. Access their elements by its index or key in `[]` brackets. They will automatically grow as you add items

```swift
var groceries = ["bread", "cheese", "fruits"]
var bballTeams = [
    "Lebron": "Cavaliers",
    "Durant": "Warriors",
    "Kawhi": "Raptors"
]

groceries[1] = "cheddar cheese"
groceries.append("vegetables")

bballTeams["Lebron"] = "Lakers"
bballTeams["Harden"] = "Rockets"

print(groceries)
print(bballTeams)

> ["bread", "cheddar cheese", "fruits", "vegetables"]
> [
>    "Kawhi": "Raptors",
>    "Harden": "Rockets",
>    "Lebron": "Lakers",
>    "Durant": "Warriors"
> ]
```

Create an empty array or dictionary using the initializer syntax. If type can be inferred, you can write an empty array as `[]` and an empty dictionary as `[:]`

```swift
let emptyArray = [String]()
let emptyDictionary = [String: Float]()

groceries = []
bballTeams = [:]
```

## Control Flow

Use if and switch to make conditionals, and use for-in, while, and repeat-while to make loops. Parentheses around the condition or loop variable are optional. Braces around the body are required.

### Conditionals

Use `if` and `switch` to make conditionals.

In an if statement, the condition must be a boolean and there is no implicit conversion.

You can use `if` with `let` to create conditionals with optional values. You can also use the `??` operator to set default values

```swift
let individualScores = [75, 43, 103, 87, 12]
var teamScore = 0
for score in individualScores {
    if score > 50 {
        teamScore += 3
    } else {
        teamScore += 1
    }
}

// if score { ... } is an error, not an implicit comparison to 0

print(teamScore) 
> 11

let fullName: String? = nil
let fallbackGreeting: String = "friend"
var greeting = "Hello!"

if let name = fullName {
    greeting = "Hello, \(name)!"
}
let informalGreeting = "Yo \(nickName ?? fallbackGreeting)"
```

Switches support any kind of data and a wide variety of comparison operations—they aren’t limited to integers and tests for equality. 

`let` can be used in a pattern to assign the value that matched the pattern to a constant. After executing the code inside the switch case that matched, the program exits from the switch statement.
                                                                                                                                      
```swift
let vegetable = "red pepper"
switch vegetable {
case "celery":
    print("Add some raisins and make ants on a log.")
case "cucumber", "watercress":
    print("That would make a good tea sandwich.")
case let x where x.hasSuffix("pepper"):
    print("Is it a spicy \(x)?")
default:
    print("Everything tastes good in soup.")
}
```

### Looping

You can use `for-in` loops to iterate over arrays and dictionaries (note: dictionaries are unordered)

```swift
let interestingNumbers = [
    "Prime": [2, 3, 5, 7, 11, 13],
    "Fibonacci": [1, 1, 2, 3, 5, 8],
    "Square": [1, 4, 9, 16, 25],
]
var largest = 0
for (kind, numbers) in interestingNumbers {
    for number in numbers {
        if number > largest {
            largest = number
        }
    }
}
print(largest)
> 25
```

You can keep an index in a loop by using `..<` to make a range of indexes, omitting the upper value, or with `...<` to include the upper value.

```swift
var total = 0
for i in 0..<4 {
    total += i
}
print(total)
> 6
```

Use while to repeat a block of code until a condition changes. The condition of a loop can be at the end instead, ensuring that the loop is run at least once.

```swift
var n = 2
while n < 100 {
    n *= 2
}

var m = 2
repeat {
    m *= 2
} while m < 100
```

## Functions & Closures

Declare a function with `func` and call it with its arguments. Separate the arguments from the return type with `->`

By default, functions use their parameter names as labels for their arguments. Write a custom argument label before the parameter name, or write `_` to use no argument label.

```swift
func greet(person: String, day: String) -> String {
    return "Hello \(person), today is \(day)."
}
greet(person: "Bob", day: "Tuesday")

func announce(_ person: String, as role: String) -> String {
    return "Hey everyone, please welcome \(person), our \(role) tonight"
}
announce("Matt", as: "host")
```

You can use tuples to return multiple values from a function. Use either the name of the tuple element or the index to refer to the value

```swift
func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
    var min = scores[0]
    var max = scores[0]
    var sum = 0

    for score in scores {
        if score > max {
            max = score
        } else if score < min {
            min = score
        }
        sum += score
    }

    return (min, max, sum)
}
let statistics = calculateStatistics(scores: [5, 3, 100, 3, 9])
print(statistics.sum)
> 120
print(statistics.2)
> 120
```

Functions are a type and can be returned as the value of another function. They can also be nested inside other functions, allowing access within the outer function.

```swift
func makeIncrementer() -> ((Int) -> Int) {
    func addOne(number: Int) -> Int {
        return 1 + number
    }
    return addOne
}
var increment = makeIncrementer()
increment(7)
```

A function can take another function as one of its arguments.

```swift
func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
    for item in list {
        if condition(item) {
            return true
        }
    }
    return false
}
func lessThanTen(number: Int) -> Bool {
    return number < 10
}
var numbers = [20, 19, 7, 12]
hasAnyMatches(list: numbers, condition: lessThanTen)
```

Functions are simply a special type of closure - one you call later. You can create a closure without a name with `{}` and use `in` to separate the arguments and return type from the body

```swift
numbers.map({ (number: Int) -> Int in
    let result = 3 * number
    return result
})
```

If the closure's type is already known, you can infer the type. If the closure is a single statement, the value is implicitly returned. You can also refer to the arguments by index. If the closure is the last argument, you can put it outside and if it is the only argument, you don't need parentheses.

```swift
numbers.map { number in 3 * number }
```

By default, function parameters are constants; use `inout` to mutate the parameter outside the function.

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```
## Enums

Values defined in the enum are its `cases`. They are not implicitly equal to 0, 1, 2, etc. but rather are a value in their own right.

```swift
enum CompassPoint {
    case north
    case south
    case east
    case west
}
```

Multiple cases can appear in a single line

```swift
enum CompassPoint {
    case north, south, east, west
}
```

When type is already defined, you can use short-hand dot when referring to other cases

```swift
var direction = CompassPoint.east
direction = .west
```

You can match on an enum with a switch statement but it must be exhaustive

```swift
switch direction {
case .north:
    print("It's cold out there")
case .south:
    print("Too hot over there")
default:
    print("Never been there")
}
```
## Structures, Classes

## Typecasting, Nested Types

## Extensions, Protocols, Generics
