# Installation

TestBox can be installed easily via [CommandBox CLI](https://www.ortussolutions.com/products/commandbox) or by your manually implementing it. 

## Commandbox installation

CommandBox is our preferred approach for all package installations, updates and more.

```bash
// latest stable version
box install testbox --saveDev

// latest bleeding edge
box install testbox@be --saveDev
```

{% hint style="info" %}
Please note the `--saveDev` flag which tells CommandBox that TestBox is a development dependency and not a production dependency.
{% endhint %}

This will install TestBox in a _`/testbox`_ folder from where you called the command. 

## Manual Installation

You can also download TestBox from [https://www.ortussolutions.com/products/testbox](https://www.ortussolutions.com/products/testbox). Extract the downloaded zip and place it anywhere you like. You can either create a mapping called \_`/testbox`\_that points to **testbox** in the distribution folder, which is the most secure approach, or you can just place it in the webroot if you like. You can also create an application-level mapping:

```javascript
this.mappings[ "/testbox" ] = expandPath( "C:/frameworks/testbox/" );
```

You could also clone Testbox from [Github](https://github.com/ortus-solutions/testbox) and help us out:

```javascript
git clone git://github.com/ortus-solutions/testbox testbox
```

## Generating a testing harness

Once you have testbox installed, you'll need a quick way to set up a testing harness. The `generate harness` command will add a new `/tests` folder to your application with a few example tests to get you started.

```bash
box testbox generate harness
```

## System Requirements

* Lucee 5.x+ 
* ColdFusion 2016+

## What's Included

The download structure includes:

* **apidocs** : The API docs for TestBox
* **system** : The main system framework folder
* **test-browser** : This is a little utility to facilitate navigating big testing suites. This helps navigate to the suites you want and execute them instead of typing all the time.
* **test-harness** : A simplified version of the TestBox runner that can be placed anywhere in your application. It includes an ANT build that allows you to execute your tests and produce results via ANT and also JUnit compliant reports via the junitreport task.
* **test-runner** : The pre-built TestBox Global Runner. You can pass in a directory mapping or bundles and select labels and boom run the tests.
* **test-visualizer:** A static visualizer of json reports.  Just drop in a `test-results.json` and run it!
* **tests** : Several sample tests and runners which actually are used to build TestBox

The only required folder is **system** and it does not need to be web-accessible. The other folders are all optional tools to help you that expect to be web-accessible. You can safely remove everything except the system folder if you want to build your own test browsers. If you are placing TestBox outside of your web root, the `/testbox` mapping needs to point to the parent folder containing the `system` directory as all TestBox models start with `testbox.system` in their package path.

