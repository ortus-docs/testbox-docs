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

In order to work with Gitlab you must create a `.gitlab-ci.yml` file in the root of your project.  Once there are commits in your repository, Gitlab will process this file as your build file.  Please refer to the Gitlab Pipelines documentation for further study.