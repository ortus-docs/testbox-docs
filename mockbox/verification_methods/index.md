# Verification Methods

The following methods are also mixed in at run-time into mock objects and they will be used to verify behavior and calls from these mock/stub objects. These are great in order to find out how many mocked methods calls have been made and to find out the arguments that where passed to each mocked method call.

|Method Name|Return Type|Description|
|--|--|--|
|$count([methodName]) |Numeric |Get the number of times all mocked methods have been called on a mock or pass in a method name and get a the method's call count.|
|$times(count,[methodName]) or $verifyCallCount(count,[methodName]) |Numeric|Assert how many calls have been made to the mock or a specific mock method|
|$never([methodName]) |Boolean|Assert that no interactions have been made to the mock or a specific mock method: Alias to $times(0)|
|$atLeast(minNumberOfInvocations,[methodName]) |Boolean|Assert that at least a certain number of calls have been made on the mock or a specific mock method|
|$once([methodName]) |Boolean|Assert that only 1 call has been made on the mock or a specific mock method|
|$atMost(maxNumberOfInvocations, [methodName]) |Boolean|Assert that at most a certain number of calls have been made on the mock or a specific mock method.|
|$callLog() |struct|Retrieve the method call logger structure of all mocked method calls.|
|$reset() |void|Reset all mock counters and logs on the targeted mock.|
|$debug() |struct|Retrieve a structure of mocking debugging information about a mock object. |


