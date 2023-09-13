---
description: August 1, 2023
---

# What's New With 5.3.x

## 5.3.1 - September 13, 2023

### Fixed

* The variable `thisSuite` isn't defined if the for loop in the try/catch is never reached before the error. ([#150](https://github.com/Ortus-Solutions/TestBox/pull/150))

## 5.3.0 - August 1, 2023

### New Features

* [TESTBOX-379](https://ortussolutions.atlassian.net/browse/TESTBOX-379) New expectations: `toBeIn(), toBeInWithCase()` so you can verify a needle in string or array targets
* [TESTBOX-380](https://ortussolutions.atlassian.net/browse/TESTBOX-380) New matchers and assertions: `toStartWith(), toStartWithCase(), startsWith(), startsWthCase()` and their appropriate negations
* [TESTBOX-381](https://ortussolutions.atlassian.net/browse/TESTBOX-381) New matchers and assertions: `toEndWith(), toEndWithCase(), endsWith(), endsWithCase()` and their appropriate negations

### Bugs

* [TESTBOX-378](https://ortussolutions.atlassian.net/browse/TESTBOX-378) onSpecError `suiteSpecs` is invalid, it's `suiteStats`
