# Creating MockBox

```javascript
mockBox = new testbox.system.MockBox();

// Within a TestBox Spec
getMockBox()
```

The factory takes in one constructor argument that is not mandatory: `generationPath`. This path is a relative path of where the factory generates internal mocking stubs that are included later on at runtime. Therefore, the path must be a path that can be used using `cfinclude`. The default path the mock factory uses is the following, so you do not have to specify one, just make sure the path has WRITE permissions:

```javascript
/testbox/system/stubs
```

> **Hint** If you are using Lucee or ACF10+ you can also decide to use the `ram://` resource and place all generated stubs in memory.

