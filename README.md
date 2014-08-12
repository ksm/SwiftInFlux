Swift InFlux
===========
The community is creating some incredible analyses and writing about Swift. What I keep asking myself whenever learning and reading about Swift is: how likely is this to change soon?

This document is an attempt to gather the Swift features that are still in flux and likely to change.

### Contributing
To contribute: fork this project, add a section below (don't forget to update the Table of Contents!), and create a pull request.

### Table of Contents

* [Absence of math.h macros](#absence-of-mathh-macros)
* [Abstract methods](#abstract-methods)
* [Access control](#limitations-of-current-access-control-design)
* [Better error handling](#better-error-handling-features-possibly-exceptions)
* [C++ support](#c-support)
* [C union support](#c-union-support)
* [Enumerating enum types](#enumerating-enum-types)
* [Enums nested in generic classes broken](#enums-nested-in-generic-classes-broken)
* [Flow-sensitive optional unwrapping](#flow-sensitive-optional-unwrapping)
* [Implicit conversions](#implicit-conversions)
* [Optionals in imported Objective-C frameworks](#optionals-in-imported-objective-c-frameworks)
* [Recursive nested functions](#recursive-nested-functions)
* [Reflection](#reflection)
* [Static libraries](#static-libraries)
* [Systems programming features](#systems-programming-features)
* [Unowned references breaking in Beta 2 and 3](#unowned-references-breaking-in-beta-2-and-3)

___

* [Changed in Beta 5](#changed-in-beta-5)
  * [`dynamic` keyword](#dynamic-declaration-modifier)
  * [Mutable optional value types](#mutable-optional-value-types)
  * [Nil coalescing operator](#nil-coalescing-operator)
  * [Usage of @-sign in front of keywords](#usage-of--sign-in-front-of-keywords)
  * [Optional Bool is confusing](#optional-bool-is-confusing)
  * [Ranges](#ranges-intervals-striding)
  * [Initializers](#required-and-designated-initializers-in-subclasses)

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

>This is a known problem, it will be fixed in later betas.
>
> — Chris Lattner

Source: https://devforums.apple.com/message/989902#989902

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

Sources: https://devforums.apple.com/message/1010766#1010766 http://adcdownload.apple.com//Developer_Tools/xcode_6_beta_4_o2p8fz/xcode_6_beta_4_release_notes.pdf

### Better error handling features (possibly exceptions)

>We're aware of the opportunity and also desire better error handling features in Swift, but they didn't make it in time for 1.0.
>
> — Chris Lattner

Source: https://devforums.apple.com/thread/228324?start=50&tstart=0

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

### Enums nested in generic classes broken

> That's another frequently reported bug.  Unfortunately, we likely won't have time to make it work for the 1.0 release, but we intend to produce a decent compiler error message saying that it isn't supported (yet).  A workaround is to define the enums outside of generic classes.
>
> — Chris Lattner

Source: https://devforums.apple.com/thread/236195?start=25&tstart=0

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

### Implicit conversions

> > It turns out Swift actually does let you define implicit type conversions using `__conversion() -> T` methods, and you can put these in extensions, making the following possible:
> >
> > ```swift
extension Int {
   func __conversion() -> CGFloat {
        return CGFloat(self)
   }
}
let x: CGFloat = 3.5
let y: Int = 2
let z = x * y // z is CGFloat and equals 7.0
```
> >
> > Use at your own risk.
>
> As you might guess from the name, __conversion is not intended to be a public feature.  It has never been documented, and we are planning to remove it for the final release.  Implicit conversions are a major source of exponential behavior (i.e., major slowdowns) in the type checker, and can lead to extremely surprising results with type inference.
>
> We understand that implicit conversions are powerful and can be convenient in narrow situations, but don't get attached to it. :-)
>
> — Chris Lattner

Source: https://devforums.apple.com/message/1011396#1011396

### Optionals in imported Objective-C frameworks

As of Beta 5, few classes have been audited for optional conformance. More are
expected to come in future betas.

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

Sources: https://devforums.apple.com/message/1012357#1012357 http://adcdownload.apple.com//Developer_Tools/xcode_6_beta_5_za4gu6/xcode_6_beta_5_release_notes.pdf

### Recursive nested functions

> This is due to a known bug with recursive nested functions.  You can fix this by pulling them out to the top level.
>
> — Chris Lattner

Sources: https://devforums.apple.com/message/997536#997536

### Reflection

> Though it’s not documented in the Swift Standard Library Reference — and is subject to change, and could disappear entirely — Swift has a reflection API.
>
> — Brent Simmons

No official word from anyone inside Apple as to whether it's gonna go public before 1.0.

Sources: http://inessential.com/2014/07/13/swift_reflection https://gist.github.com/peebsjs/9288f79322ed3119ece4

### Static libraries

> Xcode does not support building static libraries that include Swift code.
>
> — Xcode beta 5 release notes

Source: http://adcdownload.apple.com//Developer_Tools/xcode_6_beta_5_za4gu6/xcode_6_beta_5_release_notes.pdf

### Systems Programming Features

> The focus of Swift 1.0 is definitely on improving general app development, but we do expect Swift to grow capabilities (e.g. perhaps even the ability to write inline assembly code) that allow it to fully span the gamut of programming: from writing the lowest level firmware up to the highest level application programming.  We prefer to do this carefully and deliberately over time, rather than attempting to solve all the world's problems at once.
>
> — Chris Lattner

Source: https://devforums.apple.com/message/1007178#1007178

### Unowned References [Breaking](http://openradar.appspot.com/radar?id=5300501415460864) in Beta 2 and 3

> This should be fixed in Beta 3.
>
> — Chris Lattner

Still doesn't work in beta 3: see [#5](https://github.com/ksm/SwiftInFlux/pull/5)

Source: https://devforums.apple.com/message/997278#997278


___

## Changed in Beta 5

### `dynamic` declaration modifier

`dynamic` is a new attribute that can be applied to properties, methods, subscripts and initializers to make all references to them dynamically dispatched (like message passing in Objective-C). This enables KVO, proxying, swizzling and other advanced Cocoa features to work with Swift.

Before Beta 5, classes marked with `@objc` (or inheriting from `NSObject`) got the benefits of `dynamic` "for free", while non-`@objc` classes couldn't access dynamic dispatch at all. Now the two concepts are separate:

> This change also clarifies the `@objc` attribute. Now it only makes a declaration visible to Objective-C (for examples, to `objc_msgSend`), instead of conflating exposure to the Objective-C runtime with guaranteed lack of devirtualization. This is a cleaner conceptual semantic model that enables performance improvements for a wide range of `NSObject` subclasses by allowing the compiler to use more efficient dispatch and access techniques for declarations marked `@objc` (which is usually implicit).
>
> Though the feature is independent of the `@objc` attribute, the implementation still currently relies on Objective-C runtime details, so dynamic currently can only be applied to declarations with `@objc`-compatible types.

Source: http://adcdownload.apple.com//Developer_Tools/xcode_6_beta_5_za4gu6/xcode_6_beta_5__release_notes.pdf


### Mutable optional value types

From Xcode6-Beta5 release notes:

> The optional unwrapping operator x! can now be assigned through. Mutating methods and operators can be applied through it. (16922562)
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

Source: https://devforums.apple.com/message/998882#998882

### Nil coalescing operator

A new operator, `??`, has been introduced to help working with optionals. `??` takes an optional as its left operand, and a non-optional value or expression on the right. If the optional has a value, the whole expression evaluates to the value of the optional (the expression on the right is not evaluated). If the optional is `nil`, the right hand side expression is evaluated and passed as the result. You can think of the nil coalescing operator like the short-circuiting `||` operator, but for optionals.

For example:

```swift
var myArray: [Int] = []
print(myArray.first ?? 0) // produces 0, because myArray.first is nil
myArray.append(22)
print(myArray.first ?? 0) // produces 22, the value of myArray.first
```

Sources: http://adcdownload.apple.com//Developer_Tools/xcode_6_beta_5_za4gu6/xcode_6_beta_5__release_notes.pdf

### Usage of @-sign in front of keywords

Most of the `@-attributes` have been done away with in Xcode Beta 5.

> The `@assignment` attribute has been removed from operator implementations. (16656024)
>
> The `@prefix`, `@infix`, and `@postfix` attributes have been changed to declaration modifiers,
> so they are no longer spelled with an `@` sign (now, `prefix func (...)`). Operator declarations
> have been rearranged from `operator prefix - {}` to `prefix operator - {}` for consistency. (17527000)
>
> The `@class_protocol` attribute has been removed; the new syntax for declaring that only protocol conformance is limited to classes is `'protocol P : class { ... }’`. (17510790)
>
> — Xcode Beta 5 release notes

There were also additional changes:

> The @auto_closure attribute has been renamed to @autoclosure. (16859927)
>
> — Xcode Beta 5 release notes

Source: http://adcdownload.apple.com//Developer_Tools/xcode_6_beta_5_za4gu6/xcode_6_beta_5_release_notes.pdf


Some of the @-attributes [were changed in Beta 4](#revised-declaration-modifiers) to declaration modifiers, shedding the @ prefix. However, more changes to these attributes will follow.

> Future betas will include improvements to @class_protocol and adjust @prefix and other operator attributes.
>
> — Xcode beta 4 release notes
>
> This is something we're continuing to evaluate, expect @ signs to change in subsequent betas.
>
> — Chris Lattner

Source: https://devforums.apple.com/thread/228324?start=25&tstart=0,  http://adcdownload.apple.com//Developer_Tools/xcode_6_beta_4_o2p8fz/xcode_6_beta_4_release_notes.pdf


### Optional Bool is confusing

Since Beta 5, optional values no longer conform to BooleanType (formerly LogicValue) protocol, which means that:

> (...) they may no longer be used in place of boolean expressions (they must be explicitly compared with v != nil).
>
> — Xcode 6 Beta 5 release notes

Sources: http://adcdownload.apple.com//Developer_Tools/xcode_6_beta_5_za4gu6/xcode_6_beta_5_release_notes.pdf

Optionals for types conforming to the `LogicValue` protocol (primarily Bool) can have confusing semantics:

```swift
var foo: Bool? = false
// This will print bar
if foo {
    println("bar")
}
```

> This problem exists with any optional of something that conforms to the LogicValue protocol (e.g. nested optionals, optional of bool, etc).  We consider it serious issue that needs to be fixed for 1.0 and have some ideas, but haven't settled on a solution yet.
>
> — Chris Lattner
>
> FWIW, we have a fix for "optional bool confusion" that will be rolling out in the next beta (along with a few other improvements to optional semantics).  Stay tuned.
>
> — Chris Lattner

Sources: https://devforums.apple.com/thread/234399?tstart=0, https://devforums.apple.com/message/1012278#1012278

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

Source: http://adcdownload.apple.com//Developer_Tools/xcode_6_beta_5_za4gu6/xcode_6_beta_5__release_notes.pdf

### Required and designated initializers in subclasses

Swift compiler now strictly enforces the presence of required initializers in subclasses. If an ancestor of a class conforms to a protocol requiring a specific initializer and the class doesn't inherit that initializer automatically, it must define it by itself.

What it means, most commonly, is that if you subclass a Cocoa class that conforms to `NSCoding` (e.g. `UIView`) and add your own designated initializer, you must also define `init(coder:)`. If you don't want to actually implement it, you can simply make it fail at runtime, like so:

```swift
required init(coder: NSCoder) {
    fatalError("Does not implement coding")
}
```

Also, the compiler now requires overrides of designated initializers to be explicitly marked with `override` and implementations of required initializers — with `required`.

Source: [Xcode 6 beta 5 release notes](http://radex.io/xcode6-release-notes/beta5.pdf)

## Changed in Beta 4

### Access control

Beta 4 adds three levels of access control to user-defined entities: `public` (available anywhere), `internal` (available within the target where they're defined) and `private` (available only within the file where they're defined).

> By default, most entities in a source file have internal access. This allows application developers to largely ignore access control while allowing framework developers full control over a framework's API.

It's also possible to define attributes with public getters but private setters using the `private(set)` syntax.

It has been noted that the current access control design [makes unit testing a bit unwieldy](#limitations-of-current-access-control-design).

Source: http://adcdownload.apple.com//Developer_Tools/xcode_6_beta_4_o2p8fz/xcode_6_beta_4_release_notes.pdf

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
http://adcdownload.apple.com//Developer_Tools/xcode_6_beta_4_o2p8fz/xcode_6_beta_4_release_notes.pdf

>What is happening here is that CGFloat is a typealias for either Float or Double depending on whether you're building for 32 or 64-bits.  This is exactly how Objective-C works, but is problematic in Swift because Swift doesn't allow implicit conversions.
>
>We're aware of this problem and consider it to be serious: we are evaluating several different solutions right now and will roll one out in a later beta.  As you notice, you can cope with this today by casting to Double.  This is inelegant but effective :-)
>
> — Chris Lattner

Sources: https://devforums.apple.com/message/998222#998222

### Revised declaration modifiers

> The @final, @lazy, @optional, and @required attributes have been converted to declaration modifiers, specified without an @ sign.

Source: http://adcdownload.apple.com//Developer_Tools/xcode_6_beta_4_o2p8fz/xcode_6_beta_4_release_notes.pdf

### New stride() functions

> The .by() method for ranges has been replaced with general stride() functions. To
adopt stride(), use stride(from: to: by:) for exclusive ranges and stride(from: through: by:) for inclusive ranges.

For example, you can now do:

```swift
stride(from: x, to: y, by: z)      // was: (x..<y).by(z)
stride(from: x, through: y, by: z) // was: (x...y).by(z)
```

Source: http://adcdownload.apple.com//Developer_Tools/xcode_6_beta_4_o2p8fz/xcode_6_beta_4_release_notes.pdf

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

Sources: http://airspeedvelocity.net/2014/07/21/changes-in-the-swift-standard-library-in-beta-4/ http://adcdownload.apple.com//Developer_Tools/xcode_6_beta_4_o2p8fz/xcode_6_beta_4_release_notes.pdf

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
