# GitHub Actions

<center>
<img src="../images/github-actions-logo.png" alt="GitHub Actions logo" />
</center>


## About GitHub Actions

[GitHub Actions](https://github.com/features/actions) is an automation platform built in to GitHub.com that makes it easy to automate code quality on your github repos. There are a number of integrations that make using TestBox inside GitHub Actions simple, speedy and powerful so you can get back to writing code.

## Features

* FREE for public repositories
* 2,000 minutes for private repos
* Can reuse workflows, i.e. for a standard `test.yml` workflow in both builds and PR testing
* Can schedule workflow runs
* [Can configure a build "matrix"], i.e. for [testing against all recent CFML engines](https://github.com/coldbox-modules/cborm/blob/development/.github/workflows/tests.yml#L20-L25)
* [Thousands of configurable, reusable "Actions"](https://github.com/marketplace?category=&query=&type=actions&verification=)
* [Can notify Slack on build failure](https://github.com/marketplace/actions/slack-notify)

### TestBox Integration

Testing your application with TestBox in GitHub Actions (GHA) begins a `workflow.yml` file at the root of a `.github/workflows/` directory. You can name this file anything you like - I'd suggest `build.yml` or `test.yml` - but if it is not a valid Yaml file the GHA workflow will fail.

This file should start with some GHA metadata to dictate when and how the workflow should run:

```yml
# .github/workflows/tests.yml
name: Test

on:
  push:
    branches:
      - main
      - master
      - development
```

This will run the workflow on each commit to the `master` or `main` branch. Next, specify a workflow job to execute:

```yml
jobs:
  tests:
    name: Tests
    runs-on: ubuntu-latest
    steps:
      - # All job steps go here
```

Under the `jobs.tests.steps` is where we will place each sequential testing step. First, we need to check out the repository code and install Java and CommandBox:

```yml
      - name: Checkout Repository
        uses: actions/checkout@v2
        with:
          fetch-depth: 0

      - name: Setup Java
        uses: actions/setup-java@v2
        with:
          distribution: "adopt"
          java-version: "11"

      - name: Setup CommandBox
        uses: Ortus-Solutions/setup-commandbox@main
```

If we need to install dependencies, we would do that now:

```yml
      - name: Install dependencies
        run: box install
```

And finally, we can start a CFML server of our choice using CommandBox, before using the `testbox run` command to run our test suite:

```yml
      - name: Start server
        run: box server start cfengine=lucee@5.3 --noSaveSettings

      - name: Run TestBox Tests
        run: box testbox run
```

> If you are using a testing matrix to test against multiple CFML engines, replace `lucee@5.3` with `${{ matrix.cfengine }}`. [See an example here](https://github.com/coldbox-modules/hyper/blob/main/.github/workflows/release.yml#L37)

The full example would look something like this:

```yml
# .github/workflows/tests.yml
name: Test

on:
  push:
    branches:
      - main
      - master
      - development
jobs:
  tests:
    name: Tests
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2
        with:
          fetch-depth: 0

      - name: Setup Java
        uses: actions/setup-java@v2
        with:
          distribution: "adopt"
          java-version: "11"

      - name: Setup CommandBox
        uses: Ortus-Solutions/setup-commandbox@main

      - name: Install dependencies
        run: box install

      - name: Start server
        run: box server start cfengine=lucee@5.3 --noSaveSettings

      - name: Run TestBox Tests
        run: box testbox run
```

### Box.json

In order for the `box testbox run` to execute correctly, our `box.json` in our project must be able to connect to our server and know which tests to execute. Here's a basic example showing the most important testbox property: the `testbox.runner` property:

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
        ]
    }
}
```

We can also [skip setting the `testbox.runner` property and use the `box testbox run "http://localhost:8080/tests/runner.cfm"` command format instead.](https://commandbox.ortusbooks.com/testbox-integration/test-runner#run-unit-test-suite-from-the-command-line) Just be aware that the TestBox integration offers a ton of configuration in case you need to skip certain tests, etc. from your GitHub Actions test run.

> See [the CommandBox docs for box.json](https://commandbox.ortusbooks.com/package-management/box.json/testbox#testbox.runner) for more details.

## Online Example

[CBORM](https://github.com/coldbox-modules/cborm) is an ORM utility wrapper for ColdBox that takes the pain out of using ORM in CFML. CBORM uses GitHub Actions to test all new commits, to package up new module versions, and even to [format all CFML code for every new PR](https://github.com/coldbox-modules/cborm/blob/development/.github/workflows/pr.yml#L29).

See [All CBORM Workflows](https://github.com/coldbox-modules/cborm/tree/development/.github/workflows), or see [recent GitHub Actions workflow runs here](https://github.com/coldbox-modules/cborm/actions).