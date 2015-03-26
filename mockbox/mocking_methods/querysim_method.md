# $querySim() Method

This method is NOT injected into mock objects but avaialble via MockBox directly in order to create queries very quickly. This is a great way to simulate cfquery calls, cfdirectory or any other cf tag the returns a query.

```javascript
function testGetUsers(){
	// Mock a query
	mockQuery = mockBox.querySim("id,fname,lname
	1 | luis | majano
	2 | joe | louis
	3 | bob | lainez");

	// tell the dao to return this query
	mockDAO.$("getUsers", mockQuery);
}
```

