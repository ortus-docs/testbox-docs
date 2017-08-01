# Gitlab

![](/assets/gitlab.png)

[Gitlab](https://www.gitlab.com) is one of the most popular collaboration software suites around.  It is not only a CI server, but a source code server, docker registry, issue manager, and much much more.  They are our personal favorite for private projects due to their power and flexibility.

![](/assets/gitlab-ci.png)

## Features

* All in one tool: CI, repository, docker registry, issues/milestones, etc.
* Same as Travis in concept - CI Runner
* Docker based
* CI Runners can be decoupled to a docker swarm
* Idea of CI pipelines
    * Pipelines composed of jobs executed in stages
    * Jobs can have dependencies, artifacts, services, etc
    * Schedule Pipelines (Cron)
    * Jobs can track environments
* Great stats + charting

## TestBox Integration

In order to work with Gitlab you must create a `.gitlab-ci.yml` file in the root of your project.  Once there are commits in your repository, Gitlab will process this file as your build file.  Please refer to the Gitlab Pipelines [documentation](https://docs.gitlab.com/ee/ci/pipelines.html) for further study.  

We will leverage the [Ortus Solutions CommandBox Docker](https://hub.docker.com/r/ortussolutions/commandbox/) image in order to provide us with the capability to run any CFML engine and to execute tests.  Please note that Gitlab runs in a docker environment.

```yml
image: ortussolutions/commandbox:alpine

stages:
  - build
  - test
  - deploy

build_app:
  stage: build
  script:
    # Install dependencies
    - box install production=true

run_tests:
  stage: test
  only:
    - development
  # when: manual, always, on_success, on_failure
  script:
      - box install && box server start
      - box testbox run

```

The build file above leverages the `ortussolutions/commandbox:alpine` image, which is a compact and secure image for CommandBox.  We then have a few stages (build,test,deploy), but let's focus on the `run_tests` job.

```yml
run_tests:
  stage: test
  only:
    - development
  # when: manual, always, on_success, on_failure
  script:
      - box install && box server start
      - box testbox run
```

We define which branches it should listen to: `development`, and then we have a `script` block that defines what to do for this job.  Please note that the `when` action is commented, because we want to execute this job every time there is a commit.  In Gitlab we can determine if a job is manual, scheduled, automatic or dependent on other jobs, which is one of the most flexible execution runners around.

In our script we basically install our dependencies for our project using CommandBox and startup a CFML server.  We then go ahead and execute our tests via `box testbox run`.

### Box.json

In order for the `box testbox run` to execute correctly, our `box.json` (See https://commandbox.ortusbooks.com/content/packages/boxjson/boxjson.html) in our project must be able to connect to our server and know which tests to execute.  Below are all the possiblities for the `testbox` integration object in CommandBox's `box.json`.  (See https://commandbox.ortusbooks.com/content/testbox-integration.html)


```js
{
    "name" : "Package Name",
    // ForgeBox unique slug
    "slug" : "",
    // semantic version of your package
    "version" : "1.0.0.buildID",
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






