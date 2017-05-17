# Installing TestBox

TestBox can be downloaded from https://www.ortussolutions.com/products/testbox or can be installed via [CommandBox CLI](https://www.ortussolutions.com/products/commandbox).  CommandBox is our preferred approach for all package installations, updates and more.

```javascript
// latest stable version
box install testbox

// latest bleeding edge
box install testbox@be
```

This will install TestBox in a */testbox* folder from where you called the command. You can also extract the download zip and place it anywhere you like and create a mapping called */testbox* that points to testbox in the distribution folder, this is the most secure approach. However, you can just place it in the webroot if you like.

```javascript
this.mappings[ "/testbox" ] = expandPath( "C:/frameworks/testbox/" );
```

You can also clone from Github and help us out :)

```javascript
git clone git://github.com/ortus-solutions/testbox testbox
```
