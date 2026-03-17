---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/mocking/mockbox/verification-methods/usdreset
---

# $reset()

This method is a utility method used to clear out all call logging and method counters.

```javascript
void $reset()
```

```javascript
security = getMockBox().createMock("model.security").$("isValidUser", true);
security.isValidUser( mockUser );

// now clear out all call logs and test again
security.$reset();
mockUser.$property("authorized","variables",true);
security.isValidUser( mockUser );
```
