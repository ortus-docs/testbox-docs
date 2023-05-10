# Our Approach and Benefits

The approach that we take with MockBox is a dynamic and minimalistic approach. Why dynamic? Well, because we dynamically transform target objects into mock form at runtime. The API for the mocking factory is very easy to use and provides you a very simplistic approach to mocking.

We even use `$()style` method calls so you can easily distinguish when using or mocking methods, properties, etc. So what can MockBox do for me?

* Create mock objects for you and keep their methods intact (Does not wipe methods, so you can do method spys, or mock helper methods)
* Create mock objects and wipe out their method signatures
* Create stub objects for objects that don't even exist yet. So you can build to interfaces and later build dependencies.
* Decorate instantiated objects with mocking capabilities (So you can mock targeted methods and properties; spys)
* Mock internal object properties, basically do property injections in any internal scope
* State-Machine Results. Have a method recycle the results as it is called consecutively. So if you have a method returning two results and you call the method 4 times, the results will be recycled: 1,2,1,2
* Method call counter, so you can keep track of how many times a method has been called
* Method arguments call logging, so you can keep track of method calls and their arguments as they are called. This is a great way to find out what was the payload when calling a mocked method
* Ability to mock results depending on the argument signatures sent to a mocked method with capabilities to even provide state-machine results
* Ability to mock private/package methods
* Ability to mock exceptions from methods or make a method throw a controlled exception
* Ability to change the return type of methods or preserve their signature at runtime, extra cool when using stubs that still have no defined signature
* Ability to call a debugger method ($debug()) on mocked objects to retrieve extra debugging information about its mocking capabilities and its mocked calls
