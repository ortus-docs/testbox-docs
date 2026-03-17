---
icon: bullhorn
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/digging-deeper/run-listeners
---

# Runner Listeners

If you are creating runners and want to tap into the runner listeners or callbacks, you can do so by creating a class or a struct with the different events we announce.

<table><thead><tr><th width="212">Event</th><th>Description</th></tr></thead><tbody><tr><td><strong>onBundleStart</strong></td><td>When each bundle begins execution</td></tr><tr><td><strong>onBundleEnd</strong></td><td>When each bundle ends execution</td></tr><tr><td><strong>onSuiteStart</strong></td><td>Before a suite (describe, story, scenario, etc)</td></tr><tr><td><strong>onSuiteEnd</strong></td><td>After a suite</td></tr><tr><td><strong>onSpecStart</strong></td><td>Before a spec (it, test, then)</td></tr><tr><td><strong>onSpecEnd</strong></td><td>After a spec</td></tr></tbody></table>

Every `run` and `runRaw` methods accepts a `callbacks` argument, which can be a Class with the right listener methods or a struct with the right closure methods. This will allow you to listen to the testing progress and get information about it. This way you can build informative reports or progress bars.

```javascript
class{

    // Called at the beginning of a test bundle cycle
    function onBundleStart( target, testResults ){
    
    }
    
    // Called at the end of the bundle testing cycle
    function onBundleEnd( target, testResults ){
    
    }
    
    // Called anytime a new suite is about to be tested
    function onSuiteStart( target, testResults, suite ){
    
    }
    
    // Called after any suite has finalized testing
    function onSuiteEnd( target, testResults, suite ){
    
    }
    
    // Called anytime a new spec is about to be tested
    function onSpecStart( target, testResults, suite, spec ){
    
    }
    
    // Called after any spec has finalized testing
    function onSpecEnd( target, testResults, suite, spec ){
    
    }
    
}
```
