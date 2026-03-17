---
icon: square-terminal
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/running-tests/commandbox-runner
---

# CommandBox Runner

By installing the CommandBox [TestBox CLI](../installing-testbox/) you can get access to our CommandBox runner.  The CommandBox runner leverages the HTTP(s) protocol to test against any server.  By default it will inspect your `box.json` for a `default` runner or try to connect to `/tests/runner.cfm` by default.&#x20;

To see all the running options run the following in your CLI shell:

```bash
testbox run help

testbox run directory="tests.specs" outputFormats="json,junit,html"

testbox run runner="http://myremoteapp.com/tests/runner.cfm"
```

It can also produce reports for you in JSON, HTML, and JUNIT.

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

## Runner Options <a href="#default-runner-url" id="default-runner-url"></a>

If you type `testbox run --help` you can see all the arguments you can set for running your tests. However, please note that you can also pre-set them in your `box.json` under the `testbox` entry:

```json
"testbox":{
    "bundles":"",
    "directory":"tests.specs",
    "excludes":"",
    "labels":"",
    "options":{},
    "recurse":true,
    "reporter":"",
    "runner":[
        {
            "default":""
        }
    ],
    "testBundles":"",
    "testSpecs":"",
    "testSuites":"",
    "verbose":true,
    "watchDelay":500,
    "watchPaths":"**.cfc"
},
```

## Runner URL <a href="#default-runner-url" id="default-runner-url"></a>

You can also set up the default runner URL in your box.json and it will be used for you. Setting the URL is a one-time operation.

```bash
package set testbox.runner="http://localhost:8080/tests/runner.cfm"
testbox run
```

You can also use a relative path and CommandBox will look up the host and port from your server settings.

```bash
package set testbox.runner="/tests/runner.cfm"
testbox run
```

The default runner URL of the `testbox run` command is `/tests/runner.cfm` so there's actually no need to even configure it if you're using the default convention location for your runner.

### Multiple Runner URLs

You can define multiple URLs for your runners by using a JSON array of objects.  Each key will be a nice identifier you can use via the `runner=key` argument in the command.

```json
"testbox" : {
    "runner" : [
        { "core"   : "http://localhost/tests/runner.cfm" },
        { "api" : "http://localhost/api/tests/runner.cfm" }
    ]
}
```

Then you can just pass in the name:

```bash
testbox run runner="core"
```

More Commands:

```bash
package set testbox.runner="[ { default : 'http://localhost/tests/runner.cfm' } ]" --append
package show testbox.runner
testbox run default
```

## Watcher

The CLI also comes with a code watcher and runner.  It will watch any paths for you, and if it detects any changes, it will run the tests you want.

```bash
testbox watch help
```

In order for this command to work, you need to have started your server and configured the URL to the test runner in your `box.json`.

```bash
package set testbox.runner=http://localhost:8080/tests/runner.cfm
server start
testbox watch
```

You can also control what files to watch.

```bash
testbox watch **.cfc
```

If you need more control over what tests run and their output, you can set additional options in your `box.json` which will be picked up automatically by `testbox run` when it fires.

```bash
package set testbox.verbose=false
package set testbox.labels=foo
package set testbox.testSuites=bar
package set testbox.watchDelay=1000
package set testbox.watchPaths=/models/**.cfc
```

This command will run in the foreground until you stop it. When you are ready to shut down the watcher, press `Ctrl+C`.

[<br>](https://commandbox.ortusbooks.com/testbox-integration/test-runner)
