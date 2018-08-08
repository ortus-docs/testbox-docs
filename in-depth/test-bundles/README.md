# Test Bundles

No matter what style you decide to use, you will still end up building a Testing Bundle CFC. This CFC will either contain BDD style suites and specs or xUnit style method tests. These components are simple components with no inheritance that can contain several different life-cycle callback methods. Internally we do the magic of making this CFC inherit all capabilities and methods from our BaseSpec class \(`testbox.system.BaseSpec`\).

> **Caution** We highly encourage you to use the inheritance approach so you can get introspection, faster execution and ability to execute CFC directly.

```javascript
component{
     // easy right?
}

component extends="testbox.system.BaseSpec"{
     // easy right?
}
```

