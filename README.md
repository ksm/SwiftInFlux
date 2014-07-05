Swift InFlux
===========
The community is creating some incredible analyses and writing about Swift. What I keep asking myself whenever learning and reading about Swift is: how likely is this to be out of date or subject to change soon? 

This document is an attempt to gather all the Swift features that are still in flux and likely to change.

### Contributing
To contribute just add a row to the table below and send a pull request in. 

### Table of probable Swift changes

What | How | Sources
------------ | ------------- | -------------
Arrays | *Array semantics were in flux at the time of Beta 1, and have been revised to provide full value semantics like Dictionary and String.  This will be available in later betas.* - Chris Lattner | https://devforums.apple.com/thread/228695?start=75&tstart=0
Character | *Note that Character is still evolving and will settle down by the final release of 1.0. One of the reasons that we use double quote syntax to initialize Characters is that they are expected to be able to hold full grapheme clusters, which are composed of multiple code points. This will roll out in a later beta.* - Chris Lattner | https://devforums.apple.com/message/997759#997759 http://oleb.net/blog/2014/07/swift-strings/
