# Injected Variables

At runtime, TestBox will inject several public variables into your testing bundle CFCs to help you with your testing:

* `$mockbox` : A reference to MockBox
* `$assert` : A reference to our Assertions library
* `$utility` : A utility CFC
* `$customMatchers` : A collection of custom matchers registered
* `$exceptionAnnotation` : The annotation used to discover expected exceptions, defaults to expectedException
* `$testID` : A unique identifier for the test bundle
* `$debugBuffer` : The debugging buffer stream
