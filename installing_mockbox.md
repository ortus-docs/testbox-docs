# Installing Mockbox

MockBox can be downloaded from http://www.coldbox.org or can be installed via CommandBox:

```javascript
// latest stable version
box install mockbox

// latest bleeding edge
box install mockbox-be

// installing with testbox
box install testbox
```

This will install MockBox in a `/mockbox` folder from where you called the command. You can also extract the download zip and place it anywhere you like and create a mapping called `/mockbox` that points to mockbox in the distribution folder, this is the most secure approach. However, you can just place it in the webroot if you like.

```javascript
this.mappings[ "/mockbox" ] = expandPath( "C:/frameworks/mockbox/" );
```

