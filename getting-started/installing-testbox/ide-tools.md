---
icon: laptop-binary
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/installing-testbox/ide-tools
---

# IDE Tools

A modern editor can enhance your testing experience. We maintain editor integrations that help you write, navigate, and run TestBox tests.

## VSCode Plugin

The VSCode plugin is the best way for you to interact with TestBox alongside the BoxLang plugin.  It allows you to run tests, generate tests, navigate tests and much more.

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

{% embed url="https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-boxlang" %}
BoxLang
{% endembed %}

{% embed url="https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-testbox" %}
TestBox
{% endembed %}

## Sublime Text Package

The [BoxLang Sublime Text package](https://boxlang.ortusbooks.com/getting-started/ide-tooling/boxlang-sublime-text) adds BoxLang syntax highlighting, completions, inline documentation, and a native TestBox runner to Sublime Text 4.

Install **BoxLang** through Package Control. With TestBox 7 or later available in your project, use the Command Palette to run the current bundle, the spec or suite at the cursor, all tests, or repeat the last run. The package uses the BoxLang CLI runner by default and can also run tests through a configured HTTP runner.

The runner requires the BoxLang CLI and TestBox 7 or later. See the [TestBox BoxLang CLI Runner guide](../running-tests/boxlang-cli-runner.md) for runner details, and the [Sublime Text package repository](https://github.com/ortus-boxlang/sublimetext-boxlang) for installation and configuration options.
