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

We will leverage the Ortus Solutions CommandBox Docker image in order to provide us with the capability to run any CFML engine and to execute tests.  Please note that Gitlab runs in a docker environment.

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
    - master
  # when: manual, always, on_success, on_failure
  script:
      - box server start
      - box testbox run

```