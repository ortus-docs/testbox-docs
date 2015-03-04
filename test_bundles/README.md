# Test Bundles

No matter what style you decide to use, you will still end up building a Testing Bundle CFC. This CFC will either contain BDD style suites and specs or xUnit style method tests. These components are simple components with no inheritance that can contain several different life-cycle callback methods. Internally we do the magic of making this CFC inherit all capabilities and methods from our BaseSpec class (*testbox.system.BaseSpec*).

```javascript
component{
     // easy right?
}
```

