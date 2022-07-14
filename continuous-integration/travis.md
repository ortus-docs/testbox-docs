# Travis

![](../assets/travis-ci.png)

[Travis CI](https://travis-ci.org) is one of the most popular CI servers for open source software. At Ortus Solutions, we use it for all of our open source software due to its strength of pull request runners and multi-matrix runners. They have both free and commercial versions, so you can leverage it for private projects as well.

![](../.gitbook/assets/travis-projectview.png)

## Features

* FREE for Open Source Projects
* Runs distributed VM’s and Container Support
* Triggers Build Script via git repository commits (`.travis.yml`)
* Multiple language support
* Many integrations and extensions
* Many notification types
* No ability to schedule/manual builds
* Great for open source projects!

## TestBox Integration

In order to work with Travis you must create a `.travis.yml` file in the root of your project. Once there are commits in your repository, Travis will process this file as your build file. Please refer to the [Travis Documentation](https://docs.travis-ci.com) for further study.

```yaml
language: java
sudo: required
dist: trusty

before_install:
  # CommandBox Keys
  - curl -fsSl https://downloads.ortussolutions.com/debs/gpg | sudo apt-key add -
  - sudo echo "deb https://downloads.ortussolutions.com/debs/noarch /" | sudo tee -a
  /etc/apt/sources.list.d/commandbox.list

install:
  - sudo apt-get update && sudo apt-get --assume-yes install commandbox
  - box install
  - box server start

script:
  - box testbox run
```

This build file is based on the `java` language and an Ubuntu Trusty image. We start off by executing the `before_install` step which installs all the OS dependencies we might need. In our case we add the CommandBox repository server keys and install CommandBox as our dependency. We then move to our `install` step which makes sure we have all the required software dependencies to execute our tests, again this looks at our `box.json` for TestBox and required project dependencies. After issuing the `box install` we move to starting up the CFML engine using `box server start` and we are ready to test.

```yaml
install:
  - sudo apt-get update && sudo apt-get --assume-yes install commandbox
  - box install
  - box server start
```

The testing occurs in the `script` block:

```yaml
script:
  - box testbox run
```

In our script we basically install our dependencies for our project using CommandBox and startup a CFML server. We then go ahead and execute our tests via `box testbox run`.

### Box.json

In order for the `box testbox run` to execute correctly, our `box.json` in our project must be able to connect to our server and know which tests to execute. Below are all the possiblities for the `testbox` integration object in CommandBox's `box.json`. (See [the CommandBox docs for box.json](https://commandbox.ortusbooks.com/package-management/box.json/testbox#testbox.runner) for more details.)

```javascript
{
    "name" : "Package Name",
    // ForgeBox unique slug
    "slug" : "",
    // semantic version of your package
    "version" : "1.0.0+buildID",
    // author of this package
    "author" : "Luis Majano <lmajano@ortussolutions.com>",
    // location of where to download the package, overrides ForgeBox location
    "location" : "URL,Git/svn endpoint,etc",

    // testbox integration
    testbox :{
        // The location of the runner
        runner : [
            { "default": "http://localhost:8080/tests/runner.cfm" }
        ],
        // Which labels to run, empty means all
        "labels" : "",
        // Which reporter to use, default is json
        "reporter" : "",
        // Which CFC bundles to execute, default is all
        "bundles" : "",
        // Which directories to execute
        "directory" : "tests.specs",
        // Recurse the directories for CFCs
        "recurse" : true,
        // Which bundles to filter on
        "testBundles" : "",
        // Which suites to filter on
        "testSuites" : "",
        // Which specs to filter on
        "testSpecs" : "",
        // Display extra details inlcuding passing and skipped tests.
        "verbose" : true,
        // How may milliseconds to wait before polling for changes, defaults to 500 ms
        "watchDelay" : 500,
        // Command delimited list of file globbing paths to watch relative to the working directory
        "watchPaths" : "**.cfc"
    }
}
```

## Online Example: cbVue

You can look at our `cbVue` sample application online: [https://travis-ci.org/coldbox-samples/cbvue](https://travis-ci.org/coldbox-samples/cbvue) which contains all CI server integrations.
