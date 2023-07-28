# Installation

TestBox can be installed easily via [CommandBox CLI](https://www.ortussolutions.com/products/commandbox) as a development dependency:

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

Once you install TestBox, you'll need a quick way to set up a testing harness. The `generate harness` command will add a new `/tests` folder to your application with a few example tests to get you started.

```bash
testbox generate harness
```

You can then run your tests by executing the `testbox run` command or by visiting the runner in the generated harness: `http://localhost/tests/runner.cfm`

```
testbox run
```

## System Requirements

* Lucee 5.x+&#x20;
* ColdFusion 2018+

## What's Included

<table><thead><tr><th width="167">Folder</th><th>Description</th></tr></thead><tbody><tr><td><strong>system</strong></td><td>The main system framework folder</td></tr><tr><td><strong>test-browser</strong></td><td>This is a little utility to facilitate navigating big testing suites. This helps navigate to the suites you want and execute them instead of typing all the time.</td></tr><tr><td><strong>test-harness</strong></td><td>A vanilla test runner for any application</td></tr><tr><td><strong>test-runner</strong></td><td>A simple GUI test runner</td></tr><tr><td><strong>test-visualizer</strong></td><td>A static visualizer of JSON reports. Just drop in a <code>test-results.json</code> and run it!</td></tr><tr><td><strong>tests</strong></td><td>Several sample tests and runners are actually used to build TestBox</td></tr></tbody></table>



Now that you are installed, please set up your favorite IDE with our tooling:

{% content-ref url="ide-tools.md" %}
[ide-tools.md](ide-tools.md)
{% endcontent-ref %}
