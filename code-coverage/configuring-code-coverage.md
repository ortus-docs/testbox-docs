# Configuring Code Coverage

By default, code coverage will track all .cfc and .cfm files in your web root.  However, for the most correct numbers, you want to only track the code in your app.  This means you'll want to ignore things like

* 3rd party frameworks such as ColdBox or TestBox itself
* 3rd party modules installed by CommandBox \(i.e., your `/modules` folder\)
* Backups or build directories that aren't actually part of your app
* Parts of the app that you aren't testing such as the `/tests` folder itself

Most of the coverage settings are devoted to helping TestBox know what files to track, but there are some other ones too.  Let's review them.

## Default Settings

Code coverage is enabled by default and set with a default configuration.  You can control how it behaves with a series of `<CFParam>` tags in your `/tests/runner.cfm` file.  If you created a fresh new ColdBox app from our app templates using `coldbox create app`, you'll see there are already configuration options ready for you to change.  If you are working with an existing test suite runner, place the following lines PRIOR to the `<CFInclude>` in your runner.cfm.

```markup
<!--- Code Coverage requires FusionReactor --->
<cfparam name="url.coverageEnabled"					default="true">
<cfparam name="url.coveragePathToCapture"			default="#expandPath( '/' )#">
<cfparam name="url.coverageWhitelist"				default="">
<cfparam name="url.coverageBlacklist"				default="/testbox,/coldbox,/tests,/modules,Application.cfc,/index.cfm">
<!---<cfparam name="url.coverageBrowserOutputDir"		default="#expandPath( '/tests/results/coverageReport' )#">--->
<!---<cfparam name="url.coverageSonarQubeXMLOutputPath"	default="#expandPath( '/tests/results/SonarQubeCoverage.xml' )#">--->
```

## Configuration Options

Let's go over the options above and what they do.  Feel free to comment/uncomment and modify them as you need for your code.  Note, any of these settings can be overridden by a URL variable as well.  

## coverageEnabled

Set this to `true` or `false` to enable the code coverage feature of TestBox.  This setting will default to `true` if TestBox detects that you have FusionReactor installed, `false` otherwise.  Setting this to `true` without FusionReactor installed will be ignored.

The following setting would turn off code coverage:

```markup
<cfparam name="url.coverageEnabled" default="false">
```

## coveragePathToCapture

Use this to point to the root folder that contains code you wish to gather coverage data from.  This must be an absolute path and feel free to use any CF Mappings defined in your `/tests/Application.cfc` to make the path dynamic.  This is especially useful if the app being tested is in a subfolder of the actual web root. There is nominal overhead in gathering the coverage data from files, so set this to the correct folder and instead of using the whitelist to filter down from your web root if possible.  

This setting defaults to the web root.  Also note, code coverage only looks at files ending in `.cfc` or .`cfm` so there's no need to filter additionally on that unless you want to only include, say, `.cfc` files.



```markup
<cfparam name="url.coveragePathToCapture" default="C:/absolute/path/to/webroot/">
```

## coverageWhitelist

This is a comma-delimited list of file globbing patterns relative to the `coveragePathToCapture` setting that further filters what files and folders to track.  By default this setting is empty, which means ALL CFML code in the coverage path will be tracked.  As soon as you specify at least one file globbing path in the whitelist, ONLY the files matching the whitelist will be tracked.  

For example, if you only cared about handlers and models in an MVC app, you could configure a whitelist of:

```markup
<cfparam name="url.coverageWhitelist" default="/handlers/,/models/">
```

Note, all the basic rules of file globbing apply here.  To give you a quick review

* Use `?` to match a single char like `/Application.cf?` 
* Use `*` to match multiple characters within a folder or file like `/models/*Service.cfc`.
* Use `**` to match multiple characters in any sub folder \(recursively\) like `/models/**.cfc`.
* A pattern like `foo` will match any file or folder recursively but a leading slash like `/foo` locks that pattern to the root directory so it's not a recursive match.
* A trailing slash forces a directory match like `/tests/`, but no trailing slash like `/tests` would also match a file such as `/tests-readme.md`. 

## coverageBlacklist

This is a comma-delimited list of file globbing patterns relative to the `coveragePathToCapture` setting that is applied _on top of_ any whitelist patterns to prune back folders or files you don't want to track.  There's no reason to include a path in the blacklist if you have a whitelist specified and that whitelist already doesn't include the path in question.  However, a blacklist can be very handy when you want to include everything but a few small exceptions and it's easier to list those exceptions rather than create a large number of paths in the whitelist.  

```markup
<cfparam name="url.coverageBlacklist" default="/testbox/,/build/,/myTest.cfm">
```

## coverageBrowserOutputDir

One of the most visually exciting features of Code Coverage is the ability to generate a stand-alone HTML report that allows you inspect every tracked file in your project and see exactly what lines of code are "covered" by your tests and what is not.  The Code Coverage Browser is a mini-site of static HTML files in a directory which you can open in a browser on any computer without the need for a CF engine or TextBox being present. \(read, you can zip them up and send them to your boss or store as a build artifact!\)

To enable the Code Coverage Browser, uncomment the param for it and specify an absolute file path to where you would like the static mini-site created.  You will want a dedicated directory such as `/tests/results/coverageReport` but just remember to expand it so it's absolute.  The directory will be created if it doesn't exist. 

```markup
<cfparam name="url.coverageBrowserOutputDir" default="#expandPath( '/tests/results/coverageReport' )#">
```

Also note, Windows is pesky about placing file and folder locks on your report output directory if you have it open in Windows Explorer.  If you get an error about not being able to delete the report directory, close all Explorer windows and try again.  Sadly, there's no workaround for this as Windows is the one placing the locks!

## coverageSonarQubeXMLOutputPath

SonarQube is a code quality tool that gathers information about your code base for further reporting.  If you don't use SonarQube, you may ignore this section.  You can have TestBox spit out a coverage XML file in the format that SonarQube requires by uncommenting this param and specifying an absolute file path to where you'd like the file written.  Please include the file name in the path.

```markup
<cfparam name="url.coverageSonarQubeXMLOutputPath" default="#expandPath( '/tests/results/SonarQubeCoverage.xml' )#">
```

