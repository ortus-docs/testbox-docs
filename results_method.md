# $results() Method

This method can only be used in conjunction with $() as a chained call as it needs to know for what method are the results for.

```javascript
$(...).$results(...)
```

The purpose of this method is to make a method return more than 1 result in a specific repeating sequence. This means that if you set the mock results to be 2 results and you call your method 4 times, the sequence will repeat itself 1 time. MUMBO JUMBO, show me!! Ok Ok, hold your horses.


```javascript
//Mock 3 values for the getSetting method
controller.$("getSetting").$results(true,"cacheEnabled","myapp.model");

//Call getSetting 1
<cfdump var="#controller.getSetting()#">
Results = true

//Call getSetting 2
<cfdump var="#controller.getSetting()#">
Results = "cacheEnabled"

//Call getSetting 3
<cfdump var="#controller.getSetting()#">
Results = "myapp.model"

//Call getSetting 4
<cfdump var="#controller.getSetting()#">
Results = true

//Call getSetting 5
<cfdump var="#controller.getSetting()#">
Results = "cacheEnabled"
```

As you can see, the sequence repeats itself once the call counter increases. Let's say that you have a test where the first call to a user object's isAuthorized() method is false but then it has to be true. Then you can do this:

```javascript
mockUser = getMockBox().createMock("model.User");
mockUser.$("isAuthorized").$results(false,true);
```

