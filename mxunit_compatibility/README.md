# MXUnit Compatibility

TestBox is fully complaint with [MXUnit](http://mxunit.org/) xUnit test cases. In order to leverage it you will need to create or override the `/mxunit` mapping and make it point to the `/testbox/system/compat` folder. That's it, everything should continue to work as expected. 

> **Note** you will still need TestBox to be in the web root, or have a `/testbox` mapping created even when using the MXUnit compat runner.

```javascript
this.mappings[ "/mxunit" ] = expandPath( "/testbox/system/compat" );
```

After this, all your test code remains the same but it will execute through TestBox's xUnit runners. You can even execute them via the normal URL you are used to. If there is something that is not compatible, please let us know and we will fix it.

## Expected Exceptions

We also support in the compatibility mode the expected exception MXUnit annotation: `mxunit:expectedException` and the `expectException()` methods. The `expectException()` method is not part of the assertion library, but instead is inherited from our `BaseSpec.cfc`. 

Please refer to MXUnit's documentation on the annotation and method for expected exceptions, but it is supported with one caveat. The `expectException()` method can produce unwanted results if you are running your test specs in TestBox asynchronous mode since it stores state at the component level. Only synchronous mode is supported if you are using the `expectException()` method. The annotation can be used in both modes.
