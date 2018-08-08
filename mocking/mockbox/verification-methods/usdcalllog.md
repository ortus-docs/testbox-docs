# $callLog\(\)

This method is used to retrieve a structure of method calls that have been made on mocked methods of the mock object. This is extermely useful when you want to assert that a certain method was called with the appropriate arguments. Great for testing method calls that save or update data to some kind of persistent storage. Also great to find out what was the state of the data of a call at certain points in time.

Each mocked method is a key in the structure that contains an array of calls. Each array element can have 0 or more arguments that are traced when methods where called with arguments. If they where made with ordered or named arguments, you will be able to know the difference. We recommend dumping out the structure to check out its composition.

```javascript
struct $callLog()
```

Examples:

```javascript
security = getMockBox().createMock("model.security");
//Call methods on it that perform something, but mock the saveUserState method, it returns void
security.$("saveUserState");

//get the call log for this method
userStateLog = security.$callLog().saveUserState;
expect( arrayLen(userStateLog) eq 0 ).toBeTrue();
```

