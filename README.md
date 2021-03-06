# Swift Coding Standard -- WIP

Note: This is a work in progress, nothing is final.

*Status: pre-first draft*


## Table of Contents

1. [Tools, Versions](#1-tools--versions)
1. [Objective-C Interfacing](#objective-c-interfacing)
1. [Formatting](#formatting)
1. [Naming and Binding](#4-naming-and-binding)
1. [Type Inference](#5-type-inference)
1. [Optionals](#6-optionals)
1. [Errors](#7-errors)
1. [Commenting](#8-commenting)
1. [Dependencies](#9-dependencies)
1. [License](#10-license)

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

#### [1.3.1](#131-language-stability) Language stability
Swift is still an unstable language, with breaking changes happening multiple
times per year that require significant effort to remedy. Apple does not
typically provide good support for older versions of their tooling, so
upgrading your Swift compiler is not really optional.

This can be very expensive for large projects with very complex dependencies,
so if you're not sure you can afford to spend considerable time multiple times
per year rewriting code to conform to new Swift changes, you should probably
stick to Objective-C.

#### [1.3.2](#132-existing-codebases) Existing codebases
If you have a large existing Objective-C application, there can be some
opportunities to write newer classes in Swift depending on your application
architecture. The Objective-C compatible interfaces in Swift limit your
ability to use convenient Swift constructs like enums and structs, so
deliberate design will be required if you make use of these features to
ensure the remaining Objective-C code can leverage the new work.

Very large, complicated codebases built by large, disparate teams may struggle
to integrate new Swift code with an older Objective-C codebase, so in these
cases it may be best to wait for a replatforming strategy rather than shoe-horn
Swift code in without a well-defined migration plan.

#### [1.3.3](#133-new-teams) New teams
If you have a new team that's unexperienced in Objective-C, and you're working
on a brand new project, your team might have an easier time learning Swift
than learning Objective-C.

#### [1.3.4](#134-libraries) Libraries
Unless your library's entire purpose for being is to provide fundamental
support that only makes sense in Swift, you should consider writing your
library in Objective-C. If you include nullability specifiers, their
interfaces should still be relatively intuitive to a Swift programmer.
You also guarantee the ability to be accessed by both Objective-C codebases
and Swift codebases, whereas a Swift library will generally need to put in
extra effort to expose an Objective-C friendly interface.

Consider the advantages of an Objective-C library:

* The Objective-C ABI is stable, which means you can ship binary versions of
your library to developers and not worry about them failing to link with future
versions of the compiler.
* It doesn't require projects that use your library to embed the large Swift
runtimes.
* Objective-C libraries can be embedded frameworks *or* static libraries. Swift
libraries must be embedded frameworks.


#### [1.3.5](#135-c-support) C support
While it is *possible* to interface directly with C interfaces in Swift, it is
necessarily cumbersome since Swift is a high level language with a lot of
safety checks, and C is a low level language with very few. C interfaces in
Swift make heavy use of `UnsafePointer`s and can be hard enough to understand
even *if* you are already intimiately familiar with the eccentricities of the
specific C interface you're using.

If your application will be doing a considerable amount of low level systems
stuff, you will have a much easier time with Objective-C, which integrates with
C seamlessly. You can also create object-oriented wrappers around your C
interfaces using Objective-C, and then use those wrappers from Swift.

#### [1.3.6](#136-c---support) C++ support
Swift does not (yet) support C++ interfaces, so if you need to integrate with
any C++ libraries you should stick to Objective-C++.

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
if isJedi {
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
if foo {
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
if baz {

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
if baz {
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
let foo = ( 1,2 )
let ( bar , baz ) = giveMeATuple()
```

Good:
```swift
func bar(foo: Foo) -> Foo {
    return foo
}
```

Good:
```swift
let foo = (1, 2)
let (bar, baz) = giveMeATuple()
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

#### [3.1.15](#3115-trailing-whitespace) Trailing whitespace
Do not leave trailing whitespace, even leading identation on blank lines.

Bad: line 5 has trailing whitespace:
```swift
func foo(bar: Int) -> Int {
....if bar > 0 {
........return 0
....}
....
....return 1
}
```

Good: no trailing whitespace:
```swift
func foo(bar: Int) -> Int {
....if bar > 0 {
........return 0
....}

....return 1
}
```

You should enable an option in Xcode to automatically filter this whitespace:

![Trailing whitepsace](/images/trailing-whitespace.png)

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

### 3.3 Collections

#### [3.3.1](#331-always-use-shorthand-syntax) Always use shorthand syntax

Bad: uses the generic type specifier syntax:
```swift
var foo = Dictionary<String, Double>()
var bar = Dictionary<Dictionary<String, Double>>()
var baz = Array<Int>()
```

Good: uses the shorthand Swift syntax:
```swift
var foo = [String: Double]()
var bar = [String: [String: Double]]()
var baz = [Int]()
```

#### [3.3.2](#332-newlines-in-literals) Newlines in literals
Add newlines between elements in long dictionary and array literals.

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

#### [3.3.3](#333-indentation-in-nested-collections) Indentation in nested collections
Increase indentation when using nested collections.

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

### [3.4](#34-control-flow) Control flow

#### [3.4.1](#341-parentheses) Parentheses
Include a single space around the condition or binding expression. 
Do not include parentheses. Keep the opening brace on the same line, put the
closing brace on its own line.

Bad:
```swift
for (index in 1...5){
    print("\(index)")
}

while (alive) {
    fight()
}

if (cond) {

}

switch (value) {

}
```

Good:
```swift
for index in 1...5 {
    print("\(index)")
}

while alive {
    fight()
}

if cond {

}

switch value {

}
```

#### [3.4.1](#341-switch-cases) Switch cases
Indent switch cases to align with the `switch`.

#### [3.4.2](#342-fallthrough) Fallthrough
Avoid fallthrough. Break reused logic into a function if multiple cases run
similar logic.

Bad:
```swift
var description = "The number \(value) is"
switch value {
case 2, 3, 5, 7, 11, 13, 17, 19:
    description += " a prime number, and also"
    fallthrough
default:
    description += " an integer."
}
```

Good, if a little contrived:
```swift
switch value {
case 2, 3, 5, 7, 11, 13, 17, 19:
    return numberDescription(prime: true)
default:
    return numberDescription(prime: false)
}

func numberDescription(prime: Bool) -> String {
    if prime {
        return "The number \(value) is a prime number, and also an integer"
    } else {
        return "The number \(value) is an integer"
    }
}
```

**[🔝](#table-of-contents)**

### [3.5](#35-structures--classes) Structures & Classes

#### [3.5.1](#351-properties--Methods) Properties & Methods
Arrange properties and methods in the following order:

1. Public structures (such as enums, structs, inner classes)
1. Public constants (let)
1. Public properties (var)
1. Public computed properties (get/set)
1. Public methods
1. Internal structures (such as enums, structs, inner classes)
1. Internal constants (let)
1. Internal properties (var)
1. Internal computed properties (get/set)
1. Internal methods
1. Private structures (such as enums, structs, inner classes)
1. Private constants (let)
1. Private properties (var)
1. Private computed properties (get/set)
1. Private methods

#### [3.5.2](#352-extensions) Extensions
Break a large class or struct that conforms to many protocols into extensions
that each focus on their specific conformance, if possible.

```swift
struct Foo: Fooable, Barable, CustomFooFrobnicatable {
    // ...
}
```

```swift
struct Foo {
    // ...
}

extension Foo: Fooable {
    // ...
}

extension Foo: Barable {
    // ...
}

extension Foo: CustomFooFrobnicatable {
    // ...
}
```

#### [3.5.3](#353-implicit-getters) Implicit getters
When possible, omit the `get` keyword on read-only computed properties and
read-only subscripts.

Bad:
```swift
class Foo {
    var bar: Int {
        get {
            return 42
        }
    }

    subscript(index: Int) -> Int {
        get {
            return index + 42
        }
    }
}
```

Good:
```swift
class Foo {
    var bar: Int {
        return 42
    }

    subscript(index: Int) -> Int {
        return index + 42
    }
}
```

**[🔝](#table-of-contents)**

### [3.6](#36-blocks) Blocks

#### [3.6.1](#361-as-return-types) As return types
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

## [4.](#4-naming-and-binding) Naming and Binding

### [4.1](#41-let-versus-var) `let` versus `var`
Prefer `let` over `var` wherever possible. Only use `var` if you definitely
are mutating the value. Even if in the future you may introduce mutating logic,
use a `let` until you actually introduce the code that does.

### [4.2](#42-implicit-self) Implicit self
Only explicitly refer to `self` when required. When accessing properties or
methods on `self`, leave the reference to `self` implicit by default.

Bad:
```swift
class Foo {
    var bars: Bar

    func clearBars() {
        self.bars = []
    }

    func resetBars() {
        self.clearBars()
        self.bars.append(Bar())
    }
}
```

Good:
```swift
class Foo {
    var bars: Bar

    func clearBars() {
        bars = []
    }

    func resetBars() {
        clearBars()
        bars.append(Bar())
    }
}
```

### [4.3](#43-final-by-default) `final` by default
Classes should start as `final`, and only be changed to allow subclassing if
a valid need for inheritance has been identified. Even in that case, as many
definitions as possible *within* the class should be `final` as well, following
the same rules.

## [5.](#5-type-inference) Type Inference

### [5.1](#51-obvious-types) Obvious types
Do not specify the type if it's obvious.

Bad: the type `Bool` here is obvious:
```swift
let exists: Bool = false
```

Bad: the type `[String: String]` is obvious because it's in the initializer:
```swift
var mapping: [String: String] = [String: String]()
```

### [5.2](#52-use-initializers-to-specify-the-type) Use initializers to specify the type
Bad: using a type specifier when you could've used an initializer:
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

Good: using the initializer to specify the type:
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

### [5.4](#54-fully-qualified-names-and-getters) Fully qualified names and getters
Do not include fully qualified enum names or class vars.

Bad: redundant fully qualified names:
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

let america: [UIColor] = [
    UIColor.red,
    UIColor.white,
    UIColor.blue
]
```

Good: using type inference to infer the containing type:
```swift
let foo: [FooOptions] = [
    .bar,
    .qux
]

let america: [UIColor] = [
    .red,
    .white,
    .blue
]
```
>Note: this is a special exception to 
[Rule 5.2](#52-use-initializers-to-specify-the-type).

Bad:
```swift
let json = try JSONSerialization.data(withJSONObject: foo,
                                      options: [JSONWritingOptions.prettyPrinted])
```

Good:
```swift
let json = try JSONSerialization.data(withJSONObject: foo,
                                      options: [.prettyPrinted])
```

### [5.5](#55-type-parameters) Type parameters
Omit type parameters where possible. Methods of parameterized types can omit
type parameters on the receiving type when they're identical to the receiver's.

Bad:
```swift
struct Foo<T> {
    func frobnicate(other: Foo<T>) -> Foo<T> {
        return Foo<T>()
    }
}
```

Good:
```swift
struct Foo<T> {
    func frobnicate(other: Foo) -> Foo {
        return Foo()
    }
}
```

## [6.](#6-optionals) Optionals

### [6.1](#61-pyramid-of-doom) Pyramid of doom
Use `guard`s and the "early exit" pattern to avoid creating large indentation
levels for complex checks. Combine multiple checks into a single `guard` or
`if let` to avoid overly verbose checks at the start of your function.

Bad:
```swift
if let foo = maybeFoo() {
    if foo.enabled {
        if let baz = foo.bar?.baz() {
            frobnicate(baz)
        }
    }
}
```

Bad: these checks can be combined:
```swift
guard let foo = maybeFoo() else {
    return
}

if foo.enabled {
    guard let baz = foo.bar?.baz() else {
        return
    }

    frobnicate(baz)
}
```

Good:
```swift
guard let foo = maybeFoo(),
    foo.enabled,
    let baz = foo.bar?.baz() else {
        return
}

frobnicate(baz)
```

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

## [7.](#7-errors) Errors

## [8.](#8-commenting) Commenting

## [9.](#9-dependencies) Dependencies

## [10.](#10-license) License

This *document* is licensed under the MIT license. You are free to pick
whichever license you want for your source code.

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
