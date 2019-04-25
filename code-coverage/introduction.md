# Introduction

When writing tests for an app or library, it's generally regarded that more tests is better since you're covering more functionality and more likely to catch regressions as they happen.  This is true, but more specifically, it's important that your tests run _as much code_ in your project as possible.  Tests obviously can't check code that they doesn't run!  

With BDD, there is not a one-to-one correlation between a test and a unit of code.  You may have a test for your login page, but how do you know if all the `else` blocks in your if statements or `case` blocks in your switch statements were run?  Was your error routine tested?  What about optional features, or code that only kicks in on the 3rd Tuesday of months in the springtime?  These can be difficult questions to answer just by staring at the results of your tests.  The answer to this is Code Coverage.

## Code Coverage

Code coverage does not replace your tests nor does it change how you write your tests.  It's additional metrics gathered by the testing framework _while your tests are running_ that actually tracks what lines of code were executed and what lines of code didn't get run.  Now you can finally see how much code in your app is "covered" by your tests and what code is currently being untested.  

TestBox supports code coverage statistics out-of-the box with no changes to your test suite and you can capture the data in a handful of ways, including a Coverage Browser report which visualizes every CF file in your code and shows you what lines executed and what lines didn't.

![](../.gitbook/assets/testbox-codecoverage-overview.png)

## Requirements

* TestBox 3.x+
* [FusionReactor ](https://www.fusion-reactor.com/)7+ \(separate license required\)

Please note that **FusionReactor** is a separate product not made by Ortus, but by **Intergral GmbH**.  FusionReactor is a performance monitoring tool for Java-based servers and you will need to purchase a license to use it.  We understand you may wish to use code coverage for free, but this feature would not have been possible without the line performance tracking feature of FusionReactor that allows us to match your Java bytecode to the actual code lines of your CFML.  For personal use, there is a reasonably-priced [Developer Edition](https://www.fusion-reactor.com/fusionreactor-developer-version/).  Please reach out to [FusionReactor's sales team](https://www.fusion-reactor.com/contact-us/) if you have any questions.

