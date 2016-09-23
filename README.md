# Swift Coding Standard -- WIP

Note: This is a work in progress, nothing is final.

*Status: pre-first draft*


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

## [1.](#1-tools-versions) Tools, Versions

### [1.1](#11-use-xcode-8-and-swift-30) Use Xcode 8 and Swift 3.0
Our current supported Swift version is 3.0. You must write all of your Swift
code against the Swift 3.0 compiler. If developing on macOS, this means you are
required to use Xcode 8. Always use the latest stable Xcode and Swift compiler,
and always build against the latest Base SDK in your project, even if you 
target older versions of your target platforms.

Do not use Objective-C for Unit Tests or UI Tests.

### [1.2](#12-use-quick-nimble-in-tests) Use Quick & Nimble in Tests
All unit tests should be written as `QuickSpec`s, using Nimble as your
assertion/expectation engine. You should never directly call into XCTest
functions such as `XCTAssertTrue` unless you are building some kind of helper
function or building your own custom Nimble matcher.

All UI tests are written using `XCTestCase` as its root class. Note: you are
free to create class hierarchies, but they must eventually inherit from
`XCTestCase`.

[1]: Quick: https://github.com/quick/quick <br>
[2]: Nimble: https://github.com/quick/nimble

> Why? At least for Nimble, it makes up for XCTest's assertions being verbose,
> and difficult to express intent with. Any good unit test suite will usually
> abstract helpers around their assertions to make them more readable and to
> assist in marshalling its arguments. Nimble presents a consistent and high
> quality set of tools to do this for you. At Sportsbet and our partners, we
> require use of Quick and Nimble for tests, but you are free to substitute it
> with anything similar, it's just important you don't use XCTest directly.

### [1.3](#13-when-to-use-swift) When To Use Swift

## [2.](#2-objective-c-interfacing) Objective-C Interfacing

## [3.](#3-formatting) Formatting
All formatting rules are based on a combination of Apple sample code, the
official Swift Programming Language book (for Swift 3), and the wider Swift
community consensus.

### [3.1](#31-whitespace) Whitespace

#### [3.1.1](#311-indentation) Indentation
Use *four spaces* for indentation. Tab characters are banned. Indentation with
less spaces is banned.

Bad: two spaces:
```swift
class Foo {
∙∙var bar: Bar?
}
```

Bad: tab character:
```swift
class Foo {
↹       var bar: Bar?
}
```

Good:
```swift
class Foo {
    var bar: Bar?
}
```

#### [3.1.2](#312-leading-brace) Leading brace
Place 1 space before the leading brace.

Bad:
```swift
func test(){
    print("test")
}
```

Bad:
```swift
dog.barkAsync("woof"){
    print("woofed!")
}
```

Bad:
```swift
if condition{
    print("no")
}
````

Bad:
```swift
guard let foo = maybeFoo() else{
    return
}
```

Good:
```swift
func test() {
    print("test")
}
```

Good:
```swift
dog.barkAsync("woof") {
    print("woofed!")
}
```

Good:
```swift
if condition {
    print("yes")
}
```

Good:
```swift
guard let foo = maybeFoo() else {

}
```

#### [3.1.3](#313-around-keywords-and-arguments) Around keywords and arguments
Place 1 space before the opening parenthesis in control statements (`if`, 
`while` etc.). Place no space between the argument list and the function name
in function calls and declarations.

Bad:
```swift
if(isJedi) {
    fight ()
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
if (isJedi) {
    fight()
}
```

Good:
```swift
// good
func fight() {
    print("Swooosh!")
}
```

#### [3.1.4](#314-operators) Operators
Include at least one space before and after an operator. Using multiple spaces
should only be used to line up related elements, usually assignments, across
multiple lines.

Bad:
```swift
let x=y+5
```

Bad: there's no reason for the extra space after the `=`:
```swift
let x =  y + 5
```

Good:
```swift
let x = y + 5
```

Good: multiple spaces can be used to line up the assignment operator:
```swift
let foo    = Foo()
let fooBar = FooBar()
```

#### [3.1.5](#315-end-of-file) End of file
End files with a single newline character.

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

#### [3.1.6](#316-long-method-chains) Long method chains
Use indentation when making long method chains. Especially with more than 2 
method chains. Use a leading dot, which emphasizes that the line is a 
method call, not a new statement.

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

#### [3.1.7](#317-after-code-blocks) After code blocks
Leave a blank line after code blocks and before the next statement.

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

#### [3.1.8](#318-blank-line-padding) Blank line padding
Do not pad your blocks with blank lines.

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

#### [3.1.9](#319-inside-parentheses) Inside parentheses
Do not add spaces inside parentheses.

Bad:
```swift
func bar( foo: Foo ) -> Foo {
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
func bar(foo: Foo) -> Foo {
    return foo
}
```

Good:
```swift
if (foo) {
    print(foo)
}
```

#### [3.1.10](#3110-inside-brackets) Inside brackets
Do not add spaces inside brackets.

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

#### [3.1.11](#3111-multi-line-function-calls) Multi-line function calls
Line up all the parameters with the opening parenthesis from the first line.

Bad:
```swift
performFrobnicationStrategy(frobnicator: foo,
    frobnostications: [bar, baz], frobnifier: Frobnifier.self)
```

Good: parameters are lined up:
```swift
performFrobnicationStrategy(frobnicator: foo,
                            frobnostications: [bar, baz],
                            frobnifier: Frobnifier.self)
```

Good:
```swift
func foo(completion: (Int) -> (), failure: (Int) -> ()) {
    
}

foo(completion: { value in
        print("got \(value)!")
    },
    failure: { errorCode in
        print("got error \(errorCode) :(")
    }
)
```

#### [3.1.12](#3112-return-types) Return types
Add a single space around the `->` sigil for return types.

Bad:
```swift
func frobnicate(value: Int)->Int
```

Bad:
```swift
func frobnicate(handler: ()->Void)->(()->Void)
```

Good:
```swift
func frobnicate(value: Int) -> Int
```

Good:
```swift
func frobnicate(handler: () -> Void) -> (() -> Void)
```

#### [3.1.13](#3113-multi-line-unwrapping) Multi-line unwrapping
When checking multiple conditions and/or unwrapping multiple values in a
`guard` or an `if let`, all statements after the first should be indented one
further level from the `guard` or `if let` level.

Bad:
```swift
guard let foo = maybeFoo(),
let bar = foo.maybeBar(),
let baz = bar.maybeBaz() else {
    return
}
```

Bad:
```swift
if let foo = maybeFoo(),
let bar = foo.maybeBar(),
let baz = bar.maybeBaz() {
    return
}
```

Good:
```swift
guard let foo = maybeFoo(),
    let bar = foo.maybeBar(),
    let baz = bar.maybeBaz() else {
        return
}
```

Good:
```swift
if let foo = maybeFoo(),
    let bar = foo.maybeBar(),
    let baz = bar.maybeBaz() {
        return
}
```

#### [3.1.14](#3114-around-colons) Around colons
Never add spaces between identifiers and the colon, but always include exactly
one space after the colon. This applies to class declarations, properties,
parameters, dictionary types, dictionary literals, basically everywhere you 
use `:`. The one exception is the empty dictionary literal `[:]`, which you
should avoid anyway, due to
[Rule 5.2](#52-use-initializers-to-specify-the-type).

Bad:
```swift
class Foo : NSObject
```

Bad:
```swift
let foo = [
    "bar" : 1
]
```

Bad:
```swift
class Foo {
    var bar : Bar
}
```

Bad:
```swift
func frobnicate(string : String)
```

Bad:
```swift
typealias JSONObject = [String : AnyObject]
```

Good:
```swift
class Foo: NSObject
```

Good:
```swift
let foo = [
    "bar": 1
]
```

Good:
```swift
class Foo {
    var bar: Bar
}
```

Good:
```swift
func frobnicate(string: String)
```

Good:
```swift
typealias JSONObject = [String: AnyObject]
```

**[🔝](#table-of-contents)**

### [3.2](#32-semicolons) Semicolons

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

#### [3.3.1](#331-always-use-shorthand-syntax) Always use shorthand syntax

Bad: uses the generic type specifier syntax:
```swift
var foo = Dictionary<String, Double>()
var bar = Dictionary<Dictionary<String, Double>>()
```

Good: uses the shorthand Swift syntax:
```swift
var foo = [String: Double]()
var bar = [String: [String: Double]]()
```

#### [3.3.2](#332-newlines-in-literals) Newlines in literals
Add newlines between elements in long dictionary literals.

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

#### [3.3.3](#333-indentation-in-nested-dictionaries) Indentation in nested dictionaries
Increase indentation when using nested dictionaries.

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

### [3.4](#34-arrays) Arrays

**[🔝](#table-of-contents)**

### [3.5](#35-loops) Loops

**[🔝](#table-of-contents)**

### [3.6](#36-structures--classes) Structures & Classes

**[🔝](#table-of-contents)**

### [3.7](#37-blocks) Blocks

#### [3.7.1](#371-as-return-types) As return types
Always surround a block return type with parentheses, unless it is a Typealias.

Bad:
```swift
func takeABlockGiveABlock(input: () -> Void) -> () -> Void
``` 

Bad:
```swift
typealias HandlerBlock = () -> Void
func takeABlockGiveABlock(input: HandlerBlock) -> (HandlerBlock)
```

Good:
```swift
func takeABlockGiveABlock(input: () -> Void) -> (() -> Void)
```

Good:
```swift
typealias HandlerBlock = () -> Void
func takeABlockGiveABlock(input: HandlerBlock) -> HandlerBlock
```

## [4.](#4-naming) Naming

## [5.](#5-type-inference) Type Inference

### [5.1](#51-obvious-types) Obvious types
Do not specify the type if it's obvious.

Bad: the type `Bool` here is obvious:
```swift
let exists: Bool = false
```

Bad: the type `[String: String]` is obvious because it's in the initialiser:
```swift
var mapping: [String: String] = [String: String]()
```

### [5.2](#52-use-initializers-to-specify-the-type) Use initialisers to specify the type
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

Good: using the initialiser to specify the type:
```swift
var mapping = [String: String]()
```

### [5.3](#53-array--dictionary-literals-of-any) Array & Dictionary literals of Any 
Avoid Array or Dictionary literals of `Any`.

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

If you require JSON objects which contain multiple types, consider using a
library like SwiftyJSON, Argo or JSONCore.

### [5.4](#54-fully-qualified-enum-names) Fully qualified enum names
Do not include fully qualified enum names.

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
Note: this is a special exception to 
[Rule 5.2](##52-use-initializers-to-specify-the-type).
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

## [6.](#6-optionals) Optionals

### [6.1](#61-pyramid-of-doom) Pyramid of doom

### [6.2](#62-implicitly-unwrapped-optionals) Implicitly unwrapped optionals
*Never* use an implicitly unwrapped optional outside of a test target, unless
you're creating an `IBOutlet`.

Bad:
```swift
class Foo {
    // Bad: don't use implicitly unwrapped optional types
    var baz: String!
}
```

Bad:
```swift
func neverDoThis(very: Foo!, dangerous: Bar!) -> Foo!
```

Even importing from Objective-C is no excuse for dangerous implicitly
unwrapped optionals. You can override an Objective-C method and have the Swift
override be more specific about the optionality of parameters and return types:

Bad:
```objective-c
@interface SBTFoo: NSObject
- (NSString*) frobnicate: (NSString*) input;
@end
```
```swift
class Bar: SBTFoo {
    override func frobnicate(_ input: String!) -> String! {

    }
}
```

Good:
```swift
class Bar: SBTFoo {
    override func frobnicate(_ input: String?) -> String? {

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

Good:
```swift
class FooView: UIView {
    @IBOutlet weak var childView: UIView!
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

## [7.](#errors) Errors

## [8.](#commenting) Commenting

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
