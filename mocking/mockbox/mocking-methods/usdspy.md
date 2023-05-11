---
description: Spy like us!
---

# $spy()

MockBox now supports a `$spy( method )` method that allows you to spy on methods with all the call log goodness but without removing all the methods.  Every other method remains intact, and the actual spied method remains active.  We decorate it to track its calls and return data via the `$callLog()` method.

**Example of CUT:**

```cfscript
void function doSomething(foo){
  // some code here then...
  local.foo = variables.collaborator.callMe(local.foo);
  variables.collaborator.whatever(local.foo);
}
```

**Example Test:**

```cfscript
function test_it(){
  local.mocked = createMock( "com.foo. collaborator" )
    .$spy( "callMe" )
    .$spy( "whatever" );
  variables.CUT.$property( "collaborator", "variables", local.mocked );
  assertEquals( 1, local.mocked.$count( "callMe" ) );
  assertEquals( 1, local.mocked.$count( "whatever" ) );
}
```
