# Mocking Methods

Once you have created a mock object, you can use it like the real object as it will respond exactly as it was coded. However, you can override its behavior by using the mocking methods that have been placed on the mocked object at run-time. The methods that you can call upon in your object are the following (we will review them in detail later):

| Method Name               | Return Type | Description                                                                                                                                                                                               |
| ------------------------- | ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| $()                       | The Mock    | Used to mock a method on the mock object that can return, throw or be a void method.                                                                                                                      |
| $property()               | The Mock    | Mock a property in the object on any scope.                                                                                                                                                               |
| $getProperty(name, scope) | any         | Retrieve any public or private internal state variable so you can do assertions and more mocking.                                                                                                         |
| $results()                | The Mock    | Mock 1 or more results of a mock method call, must be chained to a $() or $().$args() call                                                                                                                |
| $args()                   | The Mock    | Mock 1 or more arguments in sequential or named order. Must be called concatenated to a $() call and must be followed by a concatenated $results() call so the results are matched to specific arguments. |
| querySim()                | query       | to denote columns. Ex: id, name 1  Luis Majano 2 Joe Louis                                                                                                                                                |
