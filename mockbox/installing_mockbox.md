# Installing Mockbox

MockBox can be downloaded from http://www.ortussolutions.com/products/testbox or can be installed via [CommandBox](http://www.ortussolutions.com/products/commandbox) as part of TestBox.

```javascript
// install standalone
box install testbox

// installing bleeding edge
box install testbox-be
```

This will install MockBox in a `/mockbox` folder from where you called the command. You can also extract the download zip and place it anywhere you like and create a mapping called `/mockbox` that points to `mockbox` in the distribution folder, this is the most secure approach. However, you can just place it in the webroot if you like.

```javascript
this.mappings[ "/mockbox" ] = expandPath( "C:/frameworks/mockbox/" );
```

> **Caution** Please note that you will only install MockBox separately if NOT using it within TestBox.  When you install TestBox, Mockbox is along for the ride and included automatically for you.
