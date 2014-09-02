Swift InFlux
===========
The community is creating some incredible analyses and writing about Swift. What I keep asking myself whenever learning and reading about Swift is: how likely is this to change soon?

This document is an attempt to gather the Swift features that are still in flux and likely to change.

### Contributing
To contribute: fork this project, add a section below (don't forget to update the Table of Contents!), and create a pull request.

### Credits

Swift InFlux was created by [Karol S. Mazur](https://github.com/ksm) during [SwiftCrunch](http://swiftcrunch.com). It is maintained by the creator, and two great contributors: [Radek Pietruszewski](https://twitter.com/radexp) and [Jan Klausa](https://github.com/jklausa).

### Table of Contents

* [Absence of math.h macros](#absence-of-mathh-macros)
* [Abstract methods](#abstract-methods)
* [Access control](#limitations-of-current-access-control-design)
* [Better error handling](#better-error-handling-features-possibly-exceptions)
* [Class variables](#class-variables)
* [C++ support](#c-support)
* [C union support](#c-union-support)
* [Enumerating enum types](#enumerating-enum-types)
* [Flow-sensitive optional unwrapping](#flow-sensitive-optional-unwrapping)
* [Initializers that return nil](#initializers-that-return-nil)
* [Optionals in imported Objective-C frameworks](#optionals-in-imported-objective-c-frameworks)
* [Overriding declarations from extensions](#overriding-declarations-from-extensions)
* [Reflection](#reflection)
* [Redefinition of private entities](#redefinition-of-private-entities)
* [Static libraries](#static-libraries)
* [Systems programming features](#systems-programming-features)
* [Date of the next release](#date-of-the-next-release)
* [Enumeration case value labels](#enumeration-case-value-labels)

___

* [Changed in Beta 6](#changed-in-beta-6)
 * [Refinements to nil coalescing operator](#refinements-to-nil-coalescing-operator)
 * [Optionals in Foundation](#optionals-in-foundation)
 * [Boolean semantics of types](#boolean-semantics-of-types)
 * [Other](#other-changes-in-beta-6)

* [Changed in Beta 5](#changed-in-beta-5)
  * [`dynamic` keyword](#dynamic-declaration-modifier)
  * [Mutable optional value types](#mutable-optional-value-types)
  * [Nil coalescing operator](#nil-coalescing-operator)
  * [Attributes](#revised-attributes)
  * [Boolean semantics of optionals](#boolean-semantics-of-optionals)
  * [Ranges](#ranges-intervals-striding)
  * [Initializers](#required-and-designated-initializers-in-subclasses)
  * [Other](#other-changes)

* [Changed in Beta 4](#changed-in-beta-4)
  * [Access control](#access-control)
  * [Unicode string improvements](#unicode-string-improvements)
  * [Numerical data type conversion](#numerical-data-type-conversion-eg-cgfloat-and-swift-doubleswift-float)
  * [Revised declaration modifiers](#revised-declaration-modifiers)
  * [New stride() functions](#new-stride-functions)
  * [Set of legal operator characters](#set-of-legal-operator-characters)
  * [`@IBOutlet`](#iboutlet)
  * [Fixed: Structs with both @lazy and non-lazy properties crashes compiler](#fixed-structs-with-both-lazy-and-non-lazy-properties-crashes-compiler)
  * [Other changes to standard library](#other-changes-to-standard-library)

* [Changed in Beta 3](#changed-in-beta-3)
  * [Array and Dictionary type declaration syntax](#array-and-dictionary-type-declaration-syntax)
  * [Array value semantics](#array-value-semantics)
  * [Modifying constant properties in designated vs. convenience initializers](#modifying-constant-properties-in-designated-vs-convenience-initializers)
  * [Range operators](#range-operators)

---

### Absence of math.h macros

> This is a known problem, it will be fixed in later betas.
>
> — Chris Lattner
>
> FWIW, we consider it to be a bug that M_PI (and a variety of other imported constants) get an arbitrary fixed type assigned to them.  This affects integer constants just as much as floating point ones.
> 
> In principle, there could be a way to provide "typeless named literals" in the language, and constants imported from C macros could be imported like that.  I don't know if that's the approach we'll take, but it is one of several different options we'll evaluate down the road to improve this situation.
>
> — Chris Lattner

Source: https://devforums.apple.com/message/989902#989902 https://devforums.apple.com/message/1032523#1032523

### Abstract methods

> FWIW, we already have many bugs tracking the idea of adding abstract methods to swift.  We'll consider it in future releases, we understand the value :-)
>
> — Chris Lattner

Source: https://devforums.apple.com/message/1006592#1006592

### Limitations of current access control design

Swift [added access control](#access-control) in Beta 4, but there are limitations to its design. Entities marked as `internal` (which is the default) are not visible to other build targets, which means that unit tests (which traditionally live in a separate target) can't access them.

> We're aware that our access control design isn't great for unit testing (and this was in the release notes), we're evaluating the situation to see what we can do.
>
> — Chris Lattner
>
> A limitation of the access control system is that unit tests cannot interact with the classes and methods in an application unless they are marked public. This is because the unit test target is not part of the application module.
>
> — Xcode beta 4 release notes

At the moment, one workaround is to mark all tested entities as public, another is to move tests to the same target as the application code. However, the former ruins the benefits of access control and the latter — of code modularization.

Sources: https://devforums.apple.com/message/1010766#1010766 [Beta 4 release notes](http://ksm.github.io/SwiftInFlux/docs/beta4.pdf)

### Better error handling features (possibly exceptions)

>We're aware of the opportunity and also desire better error handling features in Swift, but they didn't make it in time for 1.0.
>
> — Chris Lattner

Source: https://devforums.apple.com/thread/228324?start=50&tstart=0

### Class variables

At the moment, stored class properties are not supported in Swift. They will come in a later release, but not in 1.0. Possibly, the main reason for this is that Swift largely depends on Objective-C runtime and implementation details, which don't support class variables.

> Class variables not yet supported.
>
> — Swift compiler error
>
> The feature set for 1.0 is nearly final.  'yet' should not be taken to mean Swift 1.0.
>
> — Chris Lattner
>
> Swift in Xcode 6 will not support class variables.
>
> — Greg Parker

Sources: https://devforums.apple.com/message/1022374#1022374 https://devforums.apple.com/message/1030167#1030167

### C++ support

>This is another obviously desirable feature, it is just a lot of work and didn't make it in 1.0 either.
>
> — Chris Lattner

Source: https://devforums.apple.com/thread/228324?start=50&tstart=0

### C union support

> We'll have at least partial support for importing unions in a future seed.
>
> — Joe Groff

Source: https://devforums.apple.com/message/1002630#1002630

### Enumerating enum types

> > Does anyone else think this would be fundamentally useful?  Or is their a good way of apporaoching this in Swift currently that I'm missing?
>
> Yes.  All of this would be super useful.  We have a large number of radars asking for similar functionality, thanks!
>
> — Chris Lattner

Source: https://devforums.apple.com/message/1003674#1003674

### Flow-sensitive optional unwrapping

With this feature, optional types would be implicitly unwrapped in the context of a block checking if an optional has a value, like so:

```swift
let x: Type?

if exists x {
   x.doSomething() // works without ? or !
}
```

> We're definitely aware of the advantages of control-flow sensitive type refinement for optionals (and also for other subtype relationships).  This would be particularly handy in ternary operators, but isn't something on the books for 1.0.  We have several radars requesting that and tracking it for consideration in a future release.
>
> — Chris Lattner

Source: https://devforums.apple.com/message/1005148#1005148

### Initializers that return nil

Swift initializers cannot return an optional type, but imported initializers from Objective-C classes might return nil. If an imported initializer returns nil into a non-optional variable or constant you will crash when trying to access it.

The workaround is to assign the returned variable to an optional variable.

> Yep, this is a known problem and mentioned in the Xcode release notes.  You can expect progress here in a subsequent beta.
>
> — Chris Lattner

Source: https://devforums.apple.com/message/1028107#1028107

### Optionals in imported Objective-C frameworks

As of Beta 7, relatively few APIs have been audited for optional conformance. More are
expected to come in the future.

> A large number of Foundation, UIKit, CoreData, SceneKit, SpriteKit, Metal APIs have been audited for optional conformance, removing a significant number of implicitly unwrapped optionals from their interfaces. This clarifies the nullability of their properties, arguments and return values of their methods. This is an ongoing effort that started shipping in beta 5.
>
> — Xcode 6 Beta 7 release notes
>
> The UIView, NSView, UIFont, and UIApplicationDelegate classes have been audited for optional conformance, removing most implicitly unwrapped optionals from their interfaces.
> (...)
> The changes in Beta 5 are simply the first step; more refinements to the SDK will come in future betas.
>
> — Xcode 6 Beta 5 release notes

> > Is there any plan to audit the Frameworks and do anything to define return values and arguments as optional (?) when nils can happen and automatically unwrapped when nil will never be returned?
>
> Yes.  Look for details in subsequent betas.
>
> — Chris Lattner

Sources: [Beta 7 release notes](http://ksm.github.io/SwiftInFlux/docs/beta7.pdf) [Beta 5 release notes](http://ksm.github.io/SwiftInFlux/docs/beta5.pdf) https://devforums.apple.com/message/1012357#1012357

### Overriding declarations from extensions

At the moment, it's not possible to override entities declared in extension by subclassing, like so:

```swift
class Base { }

extension Base {
    var foo: String { return "foo" }
}

class Sub: Base {
    override var foo: String { return "FOO" } // This is an error
}
```

> Declarations from extensions cannot be overriden yet.
>
> — Swift compiler error
>
> The feature set for 1.0 is nearly final.  'yet' should not be taken to mean Swift 1.0.
>
> — Chris Lattner

Source: https://devforums.apple.com/message/1022374#1022374

### Reflection

> Though it’s not documented in the Swift Standard Library Reference — and is subject to change, and could disappear entirely — Swift has a reflection API.
>
> — Brent Simmons

No official word from anyone inside Apple as to whether it's gonna go public before 1.0.

Sources: http://inessential.com/2014/07/13/swift_reflection https://gist.github.com/peebsjs/9288f79322ed3119ece4

### Redefinition of private entities

It's not yet possible to define two entities (functions, classes, global variables…) with the same name, even if they're private and defined in separate files. The compilation will fail as the linker can't tell them apart.

> Yup. "private" currently only guards access, but doesn't handle redefinition. This is a known issue.
> 
> — SevenTenEleven

Source: https://devforums.apple.com/message/1031354#1031354

### Static libraries

> Xcode does not support building static libraries that include Swift code.
>
> — Xcode beta 5 release notes

Source: [Beta 5 release notes](http://ksm.github.io/SwiftInFlux/docs/beta5.pdf)

### Systems Programming Features

> The focus of Swift 1.0 is definitely on improving general app development, but we do expect Swift to grow capabilities (e.g. perhaps even the ability to write inline assembly code) that allow it to fully span the gamut of programming: from writing the lowest level firmware up to the highest level application programming.  We prefer to do this carefully and deliberately over time, rather than attempting to solve all the world's problems at once.
>
> — Chris Lattner

Source: https://devforums.apple.com/message/1007178#1007178

### Date of the next release

It's unclear when the next version of Swift after 1.0 will be released, but it has been hinted that it might occur sooner than a year from now:

> When there is more to say about the compiler release plan, we'll say it.  However, I'll point out that we have historically updated Clang more than once a year.
>
> — Chris Lattner

Source: https://devforums.apple.com/message/1025424#1025424

### Enumeration case value labels

Beta 6 [removed the ability](#other-changes-in-beta-6) to label the value of a 1-tuple. Because the associated values of enumeration cases act like a tuple, the following code is now illegal:

```swift
enum SomeEnum {
  case SomeCase(label: SomeValue)
}
```

Confusingly, it's valid to label the associated values if there's more than one. This will be fixed:

> We've found that single-element labeled tuples caused more confusion than they provided benefits, because one sometimes expects that they be equivalent to (or convertible to) the value of their single element type, and other times is surprised by the inter-convertibility between the single element type and a single-element labeled tuple. 
> 
> As for the enumeration case that started this thread, we're going to bring back support for the label in the single-element case. The user view of the enumeration case is generally function-like, because one calls them like functions, which favors the use of labels. 
>
> — Doug Gregor

Also, the ability to specify default values to enum cases might be added:

> > And what about default parameter values for those enums?
>
> That would be a reasonable extension that follows from considering enum cases to be function-like.
>
> — Doug Gregor

Source: https://devforums.apple.com/message/1027499#1027499, https://devforums.apple.com/message/1029151#1029151

___

## Changed in Beta 6

### Refinements to nil coalescing operator

Beta 6 improves on the [nil coalescing operator](#nil-coalescing-operator) introduced last Beta. It's now possible to pass an optional as the righthand side operand — if both sides evaluate to nil, the whole expression evaluates to nil. This makes it possible to chain expressions using nil coalescing, like so:

```swift
let a: Int? = nil
let b: Int? = nil

a ?? b ?? 0
```

In the example above, the first chained expression that doesn't evaluate to `nil` will be used as the value of the entire operation.

Previously, passing a non-optional value as the second operand to `??` was technically valid, but its semantics were [very confusing](http://airspeedvelocity.net/2014/08/12/yo-dawg/)

Sources: [Beta 6 release notes](http://ksm.github.io/SwiftInFlux/docs/beta6.pdf), [Airspeed Velocity](http://airspeedvelocity.net/2014/08/12/yo-dawg/)

### Optionals in Foundation

> A large number of Foundation APIs have been audited for optional conformance, removing a significant number of implicitly unwrapped optionals from their interfaces. This clarifies the nullability of their properties and arguments / return values of their methods. This is an ongoing effort since beta 5.
>
> These changes replace T! with either T? or T depending on whether the value can be null (or not) respectively.

Source: [Beta 6 release notes](http://ksm.github.io/SwiftInFlux/docs/beta6.pdf)

### Boolean semantics of types

Implicitly unwrapped optionals no longer conform to `BooleanType`, which means that they now have to be explicitly compared to `nil` in if statements. (This follows an equivalent change to `Optional` [last beta](#boolean-semantics-of-optionals))

Meanwhile, non-optional types may no longer be compared to `nil`.

Source: [Beta 6 release notes](http://ksm.github.io/SwiftInFlux/docs/beta6.pdf)

### Other changes in Beta 6

* The `+` operator can no longer append a `Character` to `String`, clarifying that `+` is only for concatenation. (This is analogous to appending an element to an array which was [removed in Beta 5](#other-changes))
* `Optional.hasValue` was removed
* `RawOptionSetType` (used by imported `NS_OPTIONS`) now supports bitwise assignment operators
* One-element tuples can no longer have a label. In practice, that means that an enum case that stores one value cannot have a label ([this will be fixed](#enumeration-case-value-labels)).
* Messages in `assert` calls can now use string interpolation
* New `precondition()` function. It works similarly to `assert()` (takes a condition and stops the program if it's `false`), however unlike `assert`s, `precondition`s aren't disabled in Release mode builds. (They will be stripped, however, if the application is compiled with `-Ounchecked` setting)
* Arbitrary implicit conversions between types using `__conversion() -> T` was removed. It's now only possible to add implicit conversions from value literals [using language-defined protocols](http://nshipster.com/swift-literal-convertible/).

Further reading:

* [Beta 6 release notes](http://ksm.github.io/SwiftInFlux/docs/beta6.pdf)
* [Airspeed velocity](http://airspeedvelocity.net)

## Changed in Beta 5

### `dynamic` declaration modifier

`dynamic` is a new attribute that can be applied to properties, methods, subscripts and initializers to make all references to them dynamically dispatched (like message passing in Objective-C). This enables KVO, proxying, swizzling and other advanced Cocoa features to work with Swift.

Before Beta 5, classes marked with `@objc` (or inheriting from `NSObject`) got the benefits of `dynamic` "for free", while non-`@objc` classes couldn't access dynamic dispatch at all. Now the two concepts are separate:

> This change also clarifies the `@objc` attribute. Now it only makes a declaration visible to Objective-C (for examples, to `objc_msgSend`), instead of conflating exposure to the Objective-C runtime with guaranteed lack of devirtualization. This is a cleaner conceptual semantic model that enables performance improvements for a wide range of `NSObject` subclasses by allowing the compiler to use more efficient dispatch and access techniques for declarations marked `@objc` (which is usually implicit).
>
> Though the feature is independent of the `@objc` attribute, the implementation still currently relies on Objective-C runtime details, so dynamic currently can only be applied to declarations with `@objc`-compatible types.

Source: [Beta 5 release notes](http://ksm.github.io/SwiftInFlux/docs/beta5.pdf)


### Mutable optional value types

From [Beta 5 release notes](http://ksm.github.io/SwiftInFlux/docs/beta5.pdf):

> The optional unwrapping operator x! can now be assigned through. Mutating methods and operators can be applied through it.
```swift
var x: Int! = 0
x! = 2
x!++
// Nested dictionaries can now be mutated directly:
var sequences = ["fibonacci": [1, 1, 2, 3, 0]]
sequences["fibonacci"]![4] = 5
sequences["fibonacci"]!.append(8)
```
> The ? chaining operator can also be used to conditionally assign through an optional if it has a ! value:
```swift
sequences["fibonacci"]?.append(0)
sequences["fibonacci"]?[6] = 13
sequences["perfect"]?[0] = 6 // Does nothing because the sequence has no 'perfect' key
```

Previously:

> The issue here is that optional forcing and binding operators (postfix ! and ?) return an immutable rvalue, even when the operand is a mutable lvalue.  This means that you cannot perform mutating operations on the result, which is why optional arrays, dictionaries and other value types are pretty useless right now.
> Unfortunately there isn't a great solution or workaround right now: one approach is to wrap the value in a class and use the optional on the class wrapper:
```
class StringArray {
    var elts : String[]
}
var myArray: StringArray?
```
> We consider this a significant problem and are investigating various solutions to incorporate in a later Beta.
>
> — Chris Lattner

Sources: https://devforums.apple.com/message/998882#998882 [Beta 5 release notes](http://ksm.github.io/SwiftInFlux/docs/beta5.pdf)

### Nil coalescing operator

A new operator, `??`, has been introduced to help working with optionals. `??` takes an optional as its left operand, and a non-optional value or expression on the right. If the optional has a value, the whole expression evaluates to the value of the optional (the expression on the right is not evaluated). If the optional is `nil`, the right hand side expression is evaluated and passed as the result. You can think of the nil coalescing operator like the short-circuiting `||` operator, but for optionals.

For example:

```swift
var myArray: [Int] = []
print(myArray.first ?? 0) // produces 0, because myArray.first is nil
myArray.append(22)
print(myArray.first ?? 0) // produces 22, the value of myArray.first
```

Sources: [Beta 5 release notes](http://ksm.github.io/SwiftInFlux/docs/beta5.pdf)

### Revised attributes

Following the [changes in Beta 4](#revised-declaration-modifiers), most of the @-attributes have been changed to declaration modifiers (shedding the `@` prefix) in Beta 5.

> The `@assignment` attribute has been removed from operator implementations.
>
> The `@prefix`, `@infix`, and `@postfix` attributes have been changed to declaration modifiers,
> so they are no longer spelled with an `@` sign (now, `prefix func (...)`). Operator declarations
> have been rearranged from `operator prefix - {}` to `prefix operator - {}` for consistency.
>
> The `@class_protocol` attribute has been removed; the new syntax for declaring that only protocol conformance is limited to classes is `'protocol P : class { ... }’`.
> 
> The @auto_closure attribute has been renamed to @autoclosure.
>
> — Xcode Beta 5 release notes

Source: [Beta 5 release notes](http://ksm.github.io/SwiftInFlux/docs/beta5.pdf)

### Boolean semantics of optionals

Optional values no longer conform to `BooleanType` (formerly `LogicValue`) protocol, which means that:

> (...) they may no longer be used in place of boolean expressions (they must be explicitly compared with v != nil).
>
> — Xcode 6 Beta 5 release notes

Before this change, the boolean semantics of optionals were confusing when the optional wrapped a value that was a `BooleanType` itself:

```swift
var foo: Bool? = false
// This would print bar
if foo {
    println("bar")
}
```

Source: [Beta 5 release notes](http://ksm.github.io/SwiftInFlux/docs/beta5.pdf)

### Ranges, Intervals, Striding

Following minor changes to ranges [in Beta 3](#range-operators) and [Beta 4](#new-stride-functions), Beta 5 brings a major rework of the entire area:

> The idea of a Range has been split into three separate concepts:
>
> * Ranges, which are Collections of consecutive discrete `ForwardIndexType` values. Ranges are used for slicing and iteration.
> * Intervals over `Comparable` values, which can efficiently check for containment. Intervals are used for pattern matching in switch statements and by the `~=` operator.
> * Striding over `Strideable` values, which are `Comparable` and can be advanced an arbitrary distance in O(1).
>
> Some of the types most commonly used with the range operators `..<` and `...` – for example, `Int` — conform to both Comparable and ForwardIndexType. When used in a context that requires pattern matching (such as a switch case), the range operators create _Intervals_. Otherwise they create _Ranges_. Therefore, in a context without type constraint such as `let x = 3..<10`, the result is a _Range_.
>
> It is considered an error to form a _Range_ whose `endIndex` is not reachable from its `startIndex` by incrementation, or an _Interval_ whose end is less than its start. In these cases, _Interval_ formation always traps and _Range_ formation traps when a violation is detectable, that is, when the indices are `Comparable`.
>
> ```
1> 1...0
fatal error: Can't form Range with end < start
> ```
>
> `Intervals` are represented by two generic types, `HalfOpenInterval<T>`, created by the `..<` operator, and `ClosedInterval<T>`, created by the `...` operator:
> ```
1> 3.14..<12
$R0: HalfOpenInterval<Double> = {
  _start = 3.1400000000000001
  _end = 12
}
2> 22...99.1
$R1: ClosedInterval<Double> = {
  _start = 22
  _end = 99.099999999999994
}
> ```
>
> A _range_ `x..<y` always has `startIndex == x`. Therefore, `x` is the first valid subscript, and this applies even when the `Index` type is `Int`. In other words, the first valid subscript of `5..<10` is 5, not 0. To prevent surprise, it is a compilation error to subscript a range over an Integer type outside a generic context (for example, expressions like `(5..<10)[0]`).
>
> All _Ranges_ are represented by instances of a single generic type, `Range<T>`, whose representation is always half-open (and thus always print in the REPL and Playgrounds as a half-open range). Currently an inclusive range cannot include the last value in a sequence (for example, `4...Int.max` doesn’t work) unless the context requires an _Interval_ (like a case pattern matching specification).

Source: [Beta 5 release notes](http://ksm.github.io/SwiftInFlux/docs/beta5.pdf)

### Required and designated initializers in subclasses

Swift compiler now strictly enforces the presence of required initializers in subclasses. If an ancestor of a class conforms to a protocol requiring a specific initializer and the class doesn't inherit that initializer automatically, it must define it by itself.

What it means, most commonly, is that if you subclass a Cocoa class that conforms to `NSCoding` (e.g. `UIView`) and add your own designated initializer, you must also define `init(coder:)`. If you don't want to actually implement it, you can simply make it fail at runtime, like so:

```swift
required init(coder: NSCoder) {
    fatalError("Does not implement coding")
}
```

Also, the compiler now requires overrides of designated initializers to be explicitly marked with `override` and implementations of required initializers — with `required`.

Source: [Beta 5 release notes](http://ksm.github.io/SwiftInFlux/docs/beta5.pdf)

### Other changes

Beta 5 has seen a lot of symbols being renamed:

* Protocols were renamed so that they all end with `able`, `ible` or `Type`. For example, `Integer` protocol was renamed to `IntegerType`
* `LogicValue` was renamed to `BooleanType`
* `getLogicValue()` became a `boolValue` property and `Optional<T>` additionally has a `hasValue` property
* `UnsafeArray` and `UnsafeMutableArray` were renamed to `UnsafeBufferPointer` and `UnsafeMutableBufferPointer`
* `UnsafeConstPointer` and `UnsafePointer` were renamed to `UnsafePointer` and `UnsafeMutablePointer` for consistency and to encourage immutability
* `reinterpretCast()` was renamed to `unsafeBitCast()`

Other changes in the standard library:

* `+=` operator on arrays can no longer append a single item to the array (you have to wrap it into an array)
* `String` now has a constructor that takes an integer (you can even supply a radix)
* New `first`, `last` and `isEmpty` functions

Finally:

* Together with improvements in earlier betas, the Swift compiler can now produce far faster, better optimized code (on some benchmarks, Swift went from being two orders of magnitude slower than Objective-C to being an order of magnitude _faster_ than Objective-C)
* Meanwhile, Xcode can now recompile a single changed file instead of recompiling the whole project.  
* You can now import frameworks in Playgrounds
* `println()` in Playgrounds now prints next to the line where it's defined (not just printed in the console output)

Further reading:
* [Airspeed Velocity](http://airspeedvelocity.net/2014/08/04/changes-in-the-swift-standard-library-in-beta-5/)
* [Russ Bishop](http://www.russbishop.net/swift-beta-5)
* [Beta 5 release notes](http://ksm.github.io/SwiftInFlux/docs/beta5.pdf)
* [Apples to apples, Part II](http://www.jessesquires.com/apples-to-apples-part-two/)

## Changed in Beta 4

### Access control

Beta 4 adds three levels of access control to user-defined entities: `public` (available anywhere), `internal` (available within the target where they're defined) and `private` (available only within the file where they're defined).

> By default, most entities in a source file have internal access. This allows application developers to largely ignore access control while allowing framework developers full control over a framework's API.

It's also possible to define attributes with public getters but private setters using the `private(set)` syntax.

It has been noted that the current access control design [makes unit testing a bit unwieldy](#limitations-of-current-access-control-design).

Source: [Beta 4 release notes](http://ksm.github.io/SwiftInFlux/docs/beta4.pdf)

### Unicode string improvements

Character was changed in Beta 4 to hold a full grapheme cluster instead of a single code point.

> Certain accented characters (like é) can be represented either as a single code point or as a sequence of two or more code points (e + ́)

Before Beta 4, é achieved using "e" and a combining mark would be treated as two Character instances. Now, every character is a single Character. The change helps avoid a class of bugs when dealing with complex Unicode strings.

In addition to the above, Beta 4 removes `\x`, `\u` and `\U` escape sequences for Unicode characters and replaces them with a single, less error-prone `\u{1234}` syntax

Sources: http://oleb.net/blog/2014/07/swift-strings/ https://devforums.apple.com/message/1007773#1007773

### Numerical data type conversion, e.g. CGFloat and Swift Double/Swift Float

From Beta 4 Release Notes:
>CGFloat is now a distinct floating-point type that wraps either a Float on 32-bit architectures or a Double on 64-bit architectures.

Sources:
[Beta 4 release notes](http://ksm.github.io/SwiftInFlux/docs/beta4.pdf)

>What is happening here is that CGFloat is a typealias for either Float or Double depending on whether you're building for 32 or 64-bits.  This is exactly how Objective-C works, but is problematic in Swift because Swift doesn't allow implicit conversions.
>
>We're aware of this problem and consider it to be serious: we are evaluating several different solutions right now and will roll one out in a later beta.  As you notice, you can cope with this today by casting to Double.  This is inelegant but effective :-)
>
> — Chris Lattner

Sources: https://devforums.apple.com/message/998222#998222

### Revised declaration modifiers

> The @final, @lazy, @optional, and @required attributes have been converted to declaration modifiers, specified without an @ sign.

Source: [Beta 4 release notes](http://ksm.github.io/SwiftInFlux/docs/beta4.pdf)

### New stride() functions

> The .by() method for ranges has been replaced with general stride() functions. To
adopt stride(), use stride(from: to: by:) for exclusive ranges and stride(from: through: by:) for inclusive ranges.

For example, you can now do:

```swift
stride(from: x, to: y, by: z)      // was: (x..<y).by(z)
stride(from: x, through: y, by: z) // was: (x...y).by(z)
```

Source: [Beta 4 release notes](http://ksm.github.io/SwiftInFlux/docs/beta4.pdf)

### Set of legal operator characters

With release of Beta 4, the full grammar of operators was specified.

https://developer.apple.com/library/prerelease/mac/documentation/Swift/Conceptual/Swift_Programming_Language/LexicalStructure.html#//apple_ref/doc/uid/TP40014097-CH30-XID_927

> The set of characters is in flux, but yes, most unicode symbol characters in the BMP that are classified as 'symbol' and 'math' are available as operator characters.
>
> — Joe Groff

> It's not documented yet, but the set of allowed operator characters includes 'math' and 'symbol' characters in the Unicode BMP, and operator characters can be augmented with combining characters. The full set of supported characters will be documented in one of the following seeds.
>
> — Joe Groff

Sources: https://devforums.apple.com/thread/231723?tstart=450 https://devforums.apple.com/message/1000934#1000934

### `@IBOutlet`

Before Beta 4, marking a property with `@IBOutlet` implicitly made it a weak variable and an implicitly unwrapped optional. Now, the attribute merely makes a property visible to Interface Builder.

Previously:

> In Beta 3 (and earlier) the @IBOutlet attribute implicitly makes the variable weak, and implicitly makes it an implicitly unwrapped optional (unless it's explicitly marked with ?).  We added the 'strong' modifier in Beta 3.
>
> This is super confusing, too magic, leads to problems (like this) where "retains" are lost for types like arrays because the only reference is weak, and isn't even best practice on iOS where most outlets should be strong.  For all of these reasons, in a future Beta, @IBOutlet will become "just" an annotation for IB, without any implicit behavior.
>
> — Chris Lattner

Source: https://devforums.apple.com/message/1002722#1002722

### Fixed: Structs with both @lazy and non-lazy properties crashes compiler

structs with a @lazy property followed by a non-lazy property crashes
the compiler.

> This is fixed, but didn't make it into Beta 3. Stay tuned for a later Beta,
>
> — Chris Lattner

[The code from the Developer Forums](https://devforums.apple.com/message/1000511#1000511) no longer causes a segmentation fault in the compiler in Beta 4.

Source: https://devforums.apple.com/message/1000950#1000950

### Other changes to standard library

* `uppercaseString` and `lowercaseString` properties were removed from String
* `insertionSort` and `quickSort` were removed
* `CString` was removed. `const char *` values are now imported as `ConstUnsafePointer<Int8>`
* `modulusWithOverflow` was replaced by `remainderWithOverflow`
* `Float` and `Double` no longer conform to `RandomAccessIndex`, which means they can no longer be used to index a collection
* `true` and `false` are now language literals. `Bool` conforms to a new `BooleanLiteralConvertible` protocol that allows user-defined types to support Boolean literals.
* `ArrayBuffer`, `ArrayBufferType`, `SliceBuffer` and `ContiguousArrayBuffer` were removed (the reason being, those structures were only an implementation detail of corresponding types)
* `reverse` is no longer lazy and simply returns an Array. New `lazy` functions can be used to lazily reverse, filter and map collections through new `LazyForwardCollection`, `LazyRandomAccessCollection` and `LazySequence` structures

Sources: http://airspeedvelocity.net/2014/07/21/changes-in-the-swift-standard-library-in-beta-4/ [Beta 4 release notes](http://ksm.github.io/SwiftInFlux/docs/beta4.pdf)

## Changed in Beta 3

### Array and Dictionary type declaration syntax
Before Beta 3, the shorthand for an Array type was `Type[]`, and Dictionary types were written `Dictionary<KeyType, ValueType>`. Array type shorthand was changed to `[Type]` and Dictionaries types now have a shorthand syntax `[KeyType: ValueType]` (e.g. `[String: Bool]`)

### Array value semantics
Since Beta 3, Array has full value semantics to match Dictionary, String and other value types.

>Array semantics were in flux at the time of Beta 1, and have been revised to provide full value semantics like Dictionary and String.  This will be available in later betas.
>
> — Chris Lattner

Sources: https://devforums.apple.com/thread/228695?start=75&tstart=

### Modifying constant properties in designated vs. convenience initializers

> What is going on here is that initializers have privledged access to 'let' properties while they run: these properties are actually mutable when accessed directly within the initializer.  This is very useful when you're configurating an object during its setup, but it is absolutely required when you have an immutable property dependent on some argument to the initializer, e.g.:
>
```swift
class C {
  let x : Int   // immutable property
  init(input : Int) {
    x = input     // mutating an immutable property!
  }
}
```
> This is an important part of making immutable properties (as opposed to random other immutable variables) useful and functional, but it is dangerous, and potentially allows extensions to a type to violate invariants.
>
> Beta 3 fixes this by only allowing mutation within non-convenience initializers.  Convenience inits must delegate to some other initializer anyway, so that initializer can take an argument and do the mutation.
>
> Long story short, this is a feature, not a bug :-)
>
> — Chris Lattner

Source: https://devforums.apple.com/message/1003240#1003240

### Range operators

The half-open range operator was changed from `..` to `..<`.

> We considered this carefully.  As you can see from this thread, small syntactic issues like this are polarizing, subject to personal preferences, and have no one right answer.  See also http://en.wikipedia.org/wiki/Bikeshed
>
> For what it's worth, this approach is precendented in the groovy language.  It optimizes for readability and clarity: you're unlikely to mistake one operator for the other when skimming code, and new people coming to Swift are unlikely to assume that ..< is an inclusive range operator (like most assumed when they saw "0..5")
>
> — Chris Lattner
>
> I'd really like it if there was only a single range operator, but that isn't possible (AFAIK):
>
> - You need to have a half-open range operator to be able to represent an empty range.
> - You need an inclusive range operator to represent finite enumerated sequences when you want to include the last element (e.g. enums, but also integers that you want to include the largest integer value in)..
>
> — Chris Lattner

Sources: https://devforums.apple.com/message/1000100#1000100 https://devforums.apple.com/message/999669#999669
