# Directory Runner

```javascript
<cfset r = new coldbox.system.TestBox( directory="coldbox.testing.cases.testing.specs" ) >
<cfoutput>#r.run()#</cfoutput>

<cfset r = new coldbox.system.TestBox( 
      directory={ 
            mapping="coldbox.testing.cases.testing.specs", 
            recurse=false 
      }) >
<cfoutput>#r.run()#</cfoutput>

<cfset r = new coldbox.system.TestBox(
      directory={ 
            mapping="coldbox.testing.cases.testing.specs",
            recurse=true,
            filter=function(path){
                  return ( findNoCase( "test", arguments.path ) ? true : false );
            }
      }) >
<cfoutput>#r.run()#</cfoutput>

<cfset r = new coldbox.system.TestBox(
      directory={ 
            mapping="coldbox.testing.cases.testing.specs",
            recurse=true,
            filter=function(path){
                  return ( findNoCase( "test", arguments.path ) ? true : false );
            }
      }) >
<cfset fileWrite( 'testreports.json', r.run() )>
```

