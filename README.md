# Swift Coding Standard -- WIP

Note: This is a work in progress, nothing is final.

## Table of Contents

1. [Tools, Versions](#1-tools--versions)
1. [Objective-C Interfacing](#objective-c-interfacing)
1. [Formatting](#formatting)
1. [Naming](#naming)
1. [Type Inference](#typeinference)
1. [Optionals](#optionals)
1. [Commenting](#commenting)
1. [Dependencies](#dependencies)
1. [License](#license)

<a name="tools--versions"></a>
## [1.](#tools--versions) Tools, Versions

<a name="tools--xcode-swift"></a><a name="1.1"></a>
### [1.1](#tools--xcode-swift) Use Xcode 8 and Swift 3.0
Our current supported Swift version is 3.0. You must write all of your Swift
code against the Swift 3.0 compiler. If developing on macOS, this means you are
required to use Xcode 8. Always use the latest stable Xcode and Swift compiler,
and always build against the latest Base SDK in your project, even if you 
target older versions of your target platforms.

Do not use Objective-C for Unit Tests or UI Tests.

<a name="tools--quick-nimble"></a><a name="1.2"></a>
### [1.2](#tools--quick-nimble) Use Quick & Nimble in Tests
All unit tests should be written as `QuickSpec`s, using Nimble as your
assertion/expectation engine. You should never directly call into XCTest
functions such as `XCTAssertTrue` unless you are building some kind of helper
function or building your own custom Nimble matcher.

All UI tests are written using `XCTestCase` as its root class. Note: you are
free to create class hierarchies, but they must eventually inherit from
`XCTestCase`.

[1]: Quick: https://github.com/quick/quick <br>
[2]: Nimble: https://github.com/quick/nimble

<a name="tools--when-to-use-swift"></a><a name="1.3"></a>
### [1.3](#tools--when-to-use-swift) When To Use Swift

<a name="objective-c-interfacing"></a>
## [2.](#objective-c-interfacing) Objective-C Interfacing

<a name="formatting"></a>
## [3.](#formatting) Formatting

All formatting rules are based on a combination of Apple sample code, the
official Swift Programming Language book (for Swift 3), and the wider Swift
community consensus.

<a name="formatting--spaces-tabs"></a><a name="3.1"></a>
### [3.1](#formatting-spaces-tabs) Whitespace

<a name="formatting--indentation"></a><a name="3.1.1"></a>
#### [3.1.1](#formatting--indentation) Indentation
Use *four spaces* for indentation. Tab characters are banned. Indentation with
less spaces is banned.

<a name="formatting--multi-line-functions"></a><a name="3.1.2"></a>
#### [3.1.2](#formatting--multi-line-functions) Multi-line function calls
Line up all the parameters with the opening parenthesis from the first line.

Good: parameters are lined up:
```swift
performFrobnicationStrategy(frobnicator: foo,
                            frobnostications: [bar, baz],
                            frobnifier: Frobnifier.self)
```

Bad:
```swift
performFrobnicationStrategy(frobnicator: foo,
    frobnostications: [bar, baz], frobnifier: Frobnifier.self)
```

Good:
```swift
func foo(completion: (Int) -> (), failure: (Int) -> ()) {
    
}

foo(completion: { value in
    
    },
    failure: { errorCode in
        
    }
)
```

<a name="formatting--before-blocks"></a><a name="3.1.3"></a>
#### [3.1.3](#formatting--before-blocks) Place 1 space before the leading brace.

Bad:
```swift
func test(){
    print("test")
}
```

Good:
```swift
func test() {
    print("test")
}
```

Bad:
```swift
dog.barkAsync("woof"){
    print("woofed!")
}
```

Good:
```swift
dog.barkAsync("woof") {
    print("woofed!")
}
```

<a name="formatting--around-keywords"></a><a name="3.1.4"></a>
#### [3.1.4](#whitespace--around-keywords) Around keywords and arguments

Place 1 space before the opening parenthesis in control statements (`if`, 
`while` etc.). Place no space between the argument list and the function name
in function calls and declarations.

Bad:
```swift
if(isJedi) {
    fight ()
}
```

Good:
```swift
if (isJedi) {
    fight()
}
```

Bad:
```swift
// bad
func fight () {
    print ("Swooosh!")
}
```

Good:
```swift
// good
func fight() {
    print("Swooosh!")
}
```

<a name="formatting--infix-ops"></a><a name="3.1.5"></a>
#### [3.1.5](#formatting--infix-ops) Set off operators with spaces.

Bad:
```swift
let x=y+5
```

Good:
```swift
let x = y + 5
```

<a name="formatting--newline-at-end"></a><a name="3.1.6"></a>
#### [3.1.6](#formatting--newline-at-end) End files with a single newline character.

Bad:
```swift
class Foo {

}
```

Bad:
```swift
class Foo {
    
}↵
↵
```

Good:
```swift
class Foo {
    
}↵
```

<a name="formatting--chains"></a><a name="3.1.7"></a>
#### [3.1.7](#formatting--chains) Use indentation when making long method chains.
Especially with more than 2 method chains. Use a leading dot, which emphasizes that the line is a method call, not a new statement.

Bad:
```swift
let searchResults = searchBar.rx.text.dictinctUntilChanged().observeOn(MainScheduler.instance)
```

Good:
```swift
let searchResults = searchBar.rx.text
    .throttle(0.3, scheduler: MainScheduler.instance)
    .distinctUntilChanged()
    .flatMapLatest { query -> Observable<[Repository]> in
        if query.isEmpty {
            return .just([])
        }

        return searchGitHub(query)
            .catchErrorJustReturn([])
    }
    .observeOn(MainScheduler.instance)
```

<a name="formatting--after-blocks"></a><a name="3.1.8"></a>
#### [3.1.8](#formatting--after-blocks) Leave a blank line after code blocks and before the next statement.

Bad:
```swift
if (foo) {
    return bar
}
return baz
```

Good:
```swift
if (foo) {
    return bar
}

return baz
```

<a name="formatting--padded-blocks"></a><a name="3.1.9"></a>
#### [3.1.9](#formatting--padded-blocks) Do not pad your blocks with blank lines.

Bad:
```swift
function bar() {

    print("Wubba lubba dub dub!")

}
```

Bad:
```swift
if (baz) {

    print("Wubba lubba dub dub!")
} else {
    print("Get schwifty!")

}
```

Good:
```swift
func bar() {
    print("Wubba lubba dub dub!")
}
```

Good:
```swift
if (baz) {
    print("Wubba lubba dub dub!")
} else {
    print("Get schwifty!")
}
```

<a name="formatting--in-parens"></a><a name="3.1.10"></a>
#### [3.1.10](#formatting--in-parens) Do not add spaces inside parentheses.

Bad:
```swift
func bar( foo: Foo ) -> Foo {
    return foo
}
```

Good:
```swift
func bar(foo: Foo) -> Foo {
    return foo
}
```

Bad:
```swift
if ( foo ) {
    print(foo)
}
```

Good:
```swift
if (foo) {
    print(foo)
}
```

<a name="formatting--in-brackets"></a><a name="3.1.11"></a>
#### [3.1.11](#formatting--in-brackets) Do not add spaces inside brackets.

Bad:
```swift
let foo = [ 1, 2, 3 ]
print("\(foo[ 0 ])")
```

Good:
```swift
let foo = [1, 2, 3]
print("\(foo[ 0 ])")
```

**[🔝](#table-of-contents)**

<a name="formatting--semicolons"></a>
### [3.2](#formatting--semicolons) Semicolons

No.

Bad: semicolons
```swift
let foo = true;
let bar = baz(qux: foo);
```

Good:
```swift
let foo = true
let bar = baz(qux: foo)
```

**[🔝](#table-of-contents)**

### 3.3 Dictionaries

<a name="formatting--dict-types"></a><a name="3.3.1"></a>
#### [3.3.1](#formatting--dict-types) Always use shorthand syntax

Good: uses the shorthand Swift syntax:
```swift
var foo = [String: Double]()
var bar = [String: [String: Double]]()
```

Bad: uses the generic type specifier syntax:
```swift
var foo = Dictionary<String, Double>()
var bar = Dictionary<Dictionary<String, Double>>()
```

<a name="formatting--dict-literals"></a><a name="3.3.2"></a>
#### [3.3.2](#formatting--dict-literals) Add newlines between elements in long dictionary literals

Bad:
```swift
let foo = ["foo": 1, "bar": 2, "baz": 3, "foobar": 12, "foobarbaz": 123]
```

Good:
```swift
let foo = [
    "foo": 1,
    "bar": 2,
    "baz": 3,
    "foobar": 12,
    "foobarbaz": 123
]
```

Good:
```swift
frobnicate(foo: ["bar": 2])
```
> Why? This literal is short enough that it can be specified inline without
> being difficult to read. As a rule of thumb: if your dictionary literal can fit
> on a single line in 80 characters or less, you can write it inline, otherwise,
> use newlines.

<a name="formatting--dict-nested"></a><a name="3.3.3"></a>
#### [3.3.3](#formatting--dict-nested) Increase indentation when using nested dictionaries

Bad:
```swift
let foo = [
    "foo": [
    "bar": 1
]
    "baz": [
    "qux": 2
]
]
```

Good:
```swift
let foo = [
    "foo": [
        "bar": 1
    ],
    "baz": [
        "qux": 2
    ]
]
```

**[🔝](#table-of-contents)**

### 3.4 Arrays

**[🔝](#table-of-contents)**

### 3.5 Loops

**[🔝](#table-of-contents)**

### 3.6 Structures & Classes

**[🔝](#table-of-contents)**

### 3.7 Blocks

<a name="naming"></a>
## [4.](#naming) Naming

<a name="type-inference"></a>
## [5.](#type-inference) Type Inference

### 5.1 Do not specify the type if it's obvious
Bad: the type `Bool` here is obvious:
```swift
let exists: Bool = false
```

Bad: the type `[String: String]` is obvious because it's in the initialiser:
```swift
var mapping: [String: String] = [String: String]()
```

### 5.2 Use initialisers to specify the type
Good: using the initialiser to specify the type:
```swift
var mapping = [String: String]()
```

Bad: using a type specifier when you could've used an initialiser:
```swift
var mapping: [String: String] = [:]
```

Bad: the type specifier here is redundant:
```swift
var mapping: [String: String] = [String: String]()
```

Bad: the type specifier here is redundant:
```swift
let shared: Model = Model()
```


### 5.3 Do not use Array or Dictionary literals of `Any`
Not including type information in collection classes can make it very
difficult to manipulate or access any objects contained in it. If you're
working with an interface that accepts a collection of `Any`, you *can* pass
collections with more specific type information to it. For example:

```swift
func frobnicate(foos: [Any])

let bars = ["foo", "bar", "baz"]
// Note, the type of bars here is [String]

frobnicate(foos: bars) // This is totally ok!
```

Bad: using an `Any` type because JSONSerialization wants it:
```swift
var objects = [AnyHashable: Any]()
objects["blah"] = 1234
do {
    let json = try JSONSerialization.data(withJSONObject: objects, 
                                          options: [])
} catch _ {
    
}
```

Good: using the types properly:
```swift
var objects = [String: Int]()
objects["blah"] = 1234
do {
    let json = try JSONSerialization.data(withJSONObject: objects, 
                                          options: [])
} catch _ {
    
}
```

If you require JSON objects which contain multiple types, consider using
SwiftyJSON or JSONCore (JSONCore dependency is TBD).

### 5.4 Do not include fully qualified enum names

Bad: redundant fully qualified enum name:
```swift
enum FooOptions {
    case bar,
    case baz,
    case qux
}

let foo = [
    FooOptions.bar,
    FooOptions.qux
]
```

Good: using type inference to infer the enum:
Note: this is a special exception to rule 5.2.
```swift
let foo: [FooOptions] = [
    .bar,
    .qux
]
```

Bad:
```
let json = try JSONSerialization.data(withJSONObject: foo,
                                      options: [JSONWritingOptions.prettyPrinted])
```

Good:
```
let json = try JSONSerialization.data(withJSONObject: foo,
                                      options: [.prettyPrinted])
```

<a name="optionals"></a>
## [6.](#optionals) Optionals

### Pyramid of doom

### Implicitly unwrapped optionals

*Never* use an implicitly unwrapped optional outside of a test target, unless
you're creating an `IBOutlet`.

Bad:
```swift
class Foo {
    // Bad: don't use implicitly unwrapped optional types
    var baz: String!
}


```

Good:
```swift
class Foo {
    var baz: String?
}

guard let foo = some.objcFunction(),
      let test = foo["test"] else {
    return
}
if test {
    bar()
}
```

### Force Unwrapping

*Never* force unwrap values outside of a test target.

Bad:
```swift
class Foo {
    func bar() {
        let foo = some.objcFunction()
        if foo!["test"]! {
            bar()
        }
    }
}
```

Good:
```swift
class Foo {
    var foo: String?
}
```

Good:
```swift
class FooSpec: QuickSpec {
    var foo: String!
}
```

<a name="errors"></a>
## [7.](#errors) Errors

<a name="commenting"></a>
## [8.](#commenting) Commenting

<a name="dependencies"></a>
## [9.](#dependencies) Dependencies

## License

(The MIT License)

Copyright (c) 2016 Sportsbet

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[🔝](#table-of-contents)**
