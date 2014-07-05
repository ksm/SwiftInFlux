Swift InFlux
===========
The community is creating some incredible analyses and writing about Swift. What I keep asking myself whenever learning and reading about Swift is: how likely is this to be out of date or subject to change soon? 

This document is an attempt to gather all the Swift features that are still in flux and likely to change.

### Contributing
To contribute just fork this project and add a section below (don't forget to update Table of Contents!).

### Table of Contents

* [Arrays](#arrays)
* [Character](#character)
* [Access modifiers](#access-modifiers)
* [Numerical data type conversion](#numerical-data-type-conversion-eg-cgfloat-and-swift-doubleswift-float)
* [Optionals for values conforming to the LogicValue protocol](#optionals-for-values-conforming-to-the-logicvalue-protocol-eg-bool)
* [Access control](#access-control)
* [C++ support](#c-support)
* [Exceptions](#exceptions)
* [Usage of @-sign in front of keywords](#usage-of--sign-in-front-of-keywords)

### Arrays

>Array semantics were in flux at the time of Beta 1, and have been revised to provide full value semantics like Dictionary and String.  This will be available in later betas.
>
>-- Chris Lattner

Sources: https://devforums.apple.com/thread/228695?start=75&tstart=

### Character

>Note that Character is still evolving and will settle down by the final release of 1.0. One of the reasons that we use double quote syntax to initialize Characters is that they are expected to be able to hold full grapheme clusters, which are composed of multiple code points. This will roll out in a later beta.
>
>-- Chris Lattner

Sources: https://devforums.apple.com/message/997759#997759 http://oleb.net/blog/2014/07/swift-strings/

### Access modifiers

>We don't usually promise anything for the future, but in this case we are making an exception. Swift will have access control mechanisms.
>
>-- Greg Parker

Sources: https://devforums.apple.com/message/970220#970220

### Numerical data type conversion, e.g. CGFloat and Swift Double/Swift Float

>What is happening here is that CGFloat is a typealias for either Float or Double depending on whether you're building for 32 or 64-bits.  This is exactly how Objective-C works, but is problematic in Swift because Swift doesn't allow implicit conversions.
> 
>We're aware of this problem and consider it to be serious: we are evaluating several different solutions right now and will roll one out in a later beta.  As you notice, you can cope with this today by casting to Double.  This is inelegant but effective :-)
>
>-- Chris Lattner

Sources: https://devforums.apple.com/message/998222#998222

### Optionals for values conforming to the LogicValue protocol (e.g. Bool)

Optional Bools in a boolean context are confusing.

```swift
var foo: Bool? = false
// This will print bar
if foo {
    println("bar")
}
```

>This problem exists with any optional of something that conforms to the LogicValue protocol (e.g. nested optionals, optional of bool, etc).  We consider it serious issue that needs to be fixed for 1.0 and have some ideas, but haven't settled on a solution yet.
>-- Chris Lattner

Sources: https://devforums.apple.com/thread/234399?tstart=0

### Access control

>Access control (public/private/etc) is coming in a later beta, this is mentioned in the Xcode release notes.
>-- Chris Lattner

Sources: https://devforums.apple.com/thread/228324?start=50&tstart=0 https://devforums.apple.com/message/996725#996725

### C++ support

>This is another obviously desirable feature, it is just a lot of work and didn't make it in 1.0 either.
>-- Chris Lattner

Sources: https://devforums.apple.com/thread/228324?start=50&tstart=0

### Exceptions

>We're aware of the opportunity and also desire better error handling features in Swift, but they didn't make it in time for 1.0. 
>-- Chris Lattner

Sources: https://devforums.apple.com/thread/228324?start=50&tstart=0

### Usage of @-sign in front of keywords

>This is something we're continuing to evaluate, expect @ signs to change in subsequent betas.
>-- Chris Lattner

Sources: https://devforums.apple.com/thread/228324?start=25&tstart=0
