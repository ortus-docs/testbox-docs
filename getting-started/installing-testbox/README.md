# Installation

TestBox can be installed easily via [CommandBox CLI](https://www.ortussolutions.com/products/commandbox)

## CommandBox installation

```bash
// latest stable version
box install testbox --saveDev

// latest bleeding edge
box install testbox@be --saveDev
```

{% hint style="info" %}
Please note the `--saveDev` flag, which tells CommandBox that TestBox is a development dependency and not a production dependency.
{% endhint %}

{% hint style="danger" %}
DO NOT USE TESTBOX IN PRODUCTION.
{% endhint %}

This will install TestBox in a _`/testbox`_ folder from where you called the command.

## TestBox CLI

TestBox comes with its own CLI for CommandBox.  You can use it to generate tests, harnesses, and suites and also run executions from the CLI.

```bash
install testbox-cli
```

You will now have the `testbox namesapce available to you, try it out`

```bash
testbox help
```

## Generating a Testing Harness

Once you have TestBox installed, you'll need a quick way to set up a testing harness. The `generate harness` command will add a new `/tests` folder to your application with a few example tests to get you started.

```bash
testbox generate harness
```

## System Requirements

* Lucee 5.x+&#x20;
* ColdFusion 2018+

## What's Included

* **system** : The main system framework folder
* **test-browser** : This is a little utility to facilitate navigating big testing suites. This helps navigate to the suites you want and execute them instead of typing all the time.
* **test-harness** : A simplified version of the TestBox runner that can be placed anywhere in your application. It includes an ANT build that allows you to execute your tests and produce results via ANT and also JUnit compliant reports via the junitreport task.
* **test-runner** : The pre-built TestBox Global Runner. You can pass in a directory mapping or bundles and select labels and boom run the tests.
* **test-visualizer:** A static visualizer of json reports.  Just drop in a `test-results.json` and run it!
* **tests** : Several sample tests and runners which actually are used to build TestBox

The only required folder is **system** and it does not need to be web-accessible. The other folders are all optional tools to help you that expect to be web-accessible. You can safely remove everything except the system folder if you want to build your own test browsers. If you are placing TestBox outside of your web root, the `/testbox` mapping needs to point to the parent folder containing the `system` directory as all TestBox models start with `testbox.system` in their package path.
