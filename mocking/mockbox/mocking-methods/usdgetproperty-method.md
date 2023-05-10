# $getProperty() Method

This method can help you retrieve any public or private internal state variable so you can do assertions. You can also pass in a scope argument so you can not only retrieve properties from the variables scope but from any nested structure inside of any private scope:

```javascript
any $getProperty(name [scope='variables']
```

Parameters:

* name - The name of the property to retrieve
* scope - The scope where the property lives in. The default is variables scope.

```javascript
expect( model.$getProperty("dataNumber", "variables") ).toBe( 4 );
expect( model.$getProperty("name", "variables.instance") ).toBe(  "Luis" );
```
