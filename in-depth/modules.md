---
description: Extend TestBox your way!
---

# Modules

TestBox supports the concepts of modules just like how ColdBox has modules.  They are self-contained packages that can extend the functionality of TestBox.  They can listen to test creations, errors, failures, skippings and much more.

### Module Layout

A TestBox module layout is similar to a ColdBox Module layout. They have to be installed at `/testbox/system/modules` to be discovered and loaded and have one mandatory file: `ModuleConfig.cfc` which must exist in the root of the module folder.

```
+ testbox/system/modules
  + myModule
    + ModuleConfig.cfc
```

You can install TestBox modules from ForgeBox via the `install` command:

```bash
install id=module directory=testbox/system/modules
```

#### Differences With ColdBox Modules

* There is no WireBox
* There is no Routing
* There is no Scheduling
* There is no Interceptors
* There is no Views
* Inception works, but limited
* No module dependencies, all modules are loaded in discovered order

### ModuleConfig

This is the main descriptor file for your TestBox module.

#### Mandatory Callbacks

It must have three mandatory callbacks:

* `configure()` - Configures the module for operation
* `onLoad()` - When the module is now activated
* `onUnload()` - When the module is deactivated

```cfscript
component{

  function configure(){
  }
  
  function onLoad(){
  }
  
  function onUnload(){
  }
}
```

#### Injected Properties

The following are the injected properties:

| **Property**       | **Description**                   |
| ------------------ | --------------------------------- |
| **ModulePath**     | The module’s absolute path        |
| **ModuleMapping**  | The module’s invocation path      |
| **TestBox**        | The testbox reference             |
| **TestBoxVersion** | The version of TestBox being used |

#### Injected Methods

The following are injected methods:

| **Method**              | **Description**             |
| ----------------------- | --------------------------- |
| **getEnv()**            | Get an environment variable |
| **getSystemSetting()**  | Get a Java system setting   |
| **getSystemProperty()** | Get a Java system property  |
| **getJavaSystem()**     | Get the Java system class   |

### Module Loading Failures

If a module fails to be activated, it will still be in the module registry but marked inactive via the `active` boolean key in its registry entry. You will also find the cause of the failure in the console logs and the key `activationFailure` of the module's registry entry.

```cfscript
writedump( testbox.getModuleRegistry() )
```

{% hint style="info" %}
Not all ColdBox/CommandBox modules can be TestBox modules. Remember that TestBox modules are extremely lightweight and testing focused.
{% endhint %}

### Testing Callbacks

This ModuleConfig can also listen to the following test life-cycle events.  It will also receive several arguments to the call.  Here are the common descriptions of the arguments

* `target` - The bundle in question
* `testResults` - The TestBox results object
* `suite` - The suite descriptor
* `suiteStats` - The stats for the running suite
* `exception` - A ColdFusion exception
* `spec` - The spec descriptor
* `specStats` - The stats of the running spec

| **Callback**     | **Arguments**                                                                                                   |
| ---------------- | --------------------------------------------------------------------------------------------------------------- |
| onBundleStart()  | <ul><li>target</li><li>testResults</li></ul>                                                                    |
| onBundleEnd()    | <ul><li>target</li><li>testResults</li></ul>                                                                    |
| onSuiteStart()   | <ul><li>target</li><li>testResults</li><li>suite</li></ul>                                                      |
| onSuiteEnd()     | <ul><li>target</li><li>testResults</li><li>suite</li></ul>                                                      |
| onSuiteError()   | <ul><li>exception</li><li>target</li><li>testResults</li><li>suite</li></ul>                                    |
| onSuiteSkipped() | <ul><li>target</li><li>testResults</li><li>suite</li></ul>                                                      |
| onSpecStart()    | <ul><li>target</li><li>testResults</li><li>suite</li><li>spec</li></ul>                                         |
| onSpecEnd()      | <ul><li>target</li><li>testResults</li><li>suite</li><li>spec</li></ul>                                         |
| onSpecFailure()  | <ul><li>exception</li><li>spec</li><li>specStats</li><li>suite</li><li>suiteStats</li><li>testResults</li></ul> |
| onSpecSkipped()  | <ul><li>spec</li><li>specStats</li><li>suite</li><li>testResults</li></ul>                                      |
| onSpecError()    | <ul><li>exception</li><li>spec</li><li>specStats</li><li>suite</li><li>suiteSpecs</li><li>testResults</li></ul> |

### Manual Registration

You can also manually register and activate modules by using the `registerAndActivate( invocationPath )` method of the TestBox object.  All you have to do is pass the invocation path to your modules' root folder:

```
testbox.registerAndActivate( "tests.resources.modules.MyModule" )
```

That's it! It will register it and activate and be ready to listen.
