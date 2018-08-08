# What is Mocking?

> "A mock object is an object that takes the place of a 'real' object in such a way that makes testing easier and more meaningful, or in some cases, possible at all". by Scott Bain \(**Emergent Design**   
> [The Evolutionary Nature of Professional Software Development](http://www.netobjectives.com/resources/books/emergent-design)\)

Here are some examples of real life mocks to get you in the mocking mood:

![](../../.gitbook/assets/mockbox_nikemock.jpg)

![](../../.gitbook/assets/mockbox_pumamock.jpg)

When doing unit testing of ColdFusion CFCs, we will come to a point where a single class can have multiple external dependencies or collaborators; whether they are classes themselves, data, external APIs, etc. Therefore, in order to unit test our class exclusively and easily we need to be able to mock this behavior or have full control of this behavior. Remember that unit testing is the testing of software units in isolation. If not, we would be instantiating and creating entire set of components, frameworks, pulling network plugs and so much more ridiculous but functional things just to test one single piece of functionality and/or behavior. So in summary, mock objects are just test oriented replacements for collaborators and dependencies.

Mock objects can also be created by hand, but MockBox takes this pain away by leveraging dynamic techniques so that you can Mock dynamically and at runtime. Like [Scott Bain](http://www.netobjectives.com/about/coaches/20) describes in his [Emergent Design](http://www.amazon.com/Emergent-Design-Evolutionary-Professional-Development/dp/0321509366) book:

> "Mocks are definitely congruent with the Gang of Four \(GoF\) notion of designing to interfaces, because a mock is essentially the interface without any real implementation." - Scott Bain \(Emergent Design\)

You will be leveraging MockBox to create objects that represent your dependencies or even data, decide what methods will return \(expectations\), mock network connections, exceptions and much more. You can then very easily test the exclusive behavior of components as you will now have control of all expectations, and remember that testing is all about expectations. Also, as your object oriented applications get more complex, mocking becomes essential, but you have to be aware that there are limitations. Not only will you do unit-testing but you will need to expand to do integration testing to make sure the all encompassing behavior is still maintained. However, by using a mocking framework like MockBox you will be able to apply a test-driven development methodology to your unit-testing and be able to accelerate your development and testing. The more you mock, the more you will get a feel for it and find it completely essential when doing unit testing. Welcome to a world where mocking is fun and not frowned upon :\)

