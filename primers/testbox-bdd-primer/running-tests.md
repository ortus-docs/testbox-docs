# Running Tests

Running tests is essential of course.  There are many ways to run your tests, we will see the basics here, and you can check out our [Running Tests](../../in-depth/running-tests/) section in our in-depth guide.

### TestBox CLI

The easiest way to run your tests is to use the TestBox CLI via the `testbox run` command.  Ensure you are in the web root of your project or have configured the `box.json` to include the TestBox runner in it as shown below.  If not CommandBox will try to run by convention your site + `test/runner.cfm` for you.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
You can also pass the runner URL via the `testbox run` command.  Try out the `testbox run help` command.
{% endhint %}

Here is a simple `box.json` config that has a runner and some watcher config.

```json
"testbox":{
    "runner":"http://localhost:49616/tests/runner.cfm",
    "watchers":[
        "system/**.cfc",
        "tests/**.cfc"
    ],
    "watchDelay":"250"
}
```



{% hint style="info" %}
Check out the watcher command: `testbox watch`&#x20;
{% endhint %}



### URL Runner

Every test harness also has an HTML runner you can execute. By convention the URL is

```
http://localhost{port}/tests/runner.cfm
```

This will execute ALL tests in the `tests/specs` directory for you.

### URL Spec Runner

You can also target a specific spec to execute via the URL

```
http://localhost{port}/tests/specs/MySpec.cfc
```

## Global Runner

TestBox ships with a global runner that can run pretty much anything. You can customize it or place it wherever you need it:

![](https://raw.githubusercontent.com/ortus-docs/testbox-docs/master/.gitbook/assets/testbox-global-runner.png)

## Test Browser

TestBox ships with a test browser that is highly configurable to whatever URL-accessible path you want. It will then show you a test browser where you can navigate and execute not only individual tests but also directory suites.

![](https://raw.githubusercontent.com/ortus-docs/testbox-docs/master/.gitbook/assets/testbox-browser.png)
