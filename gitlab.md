# Gitlab

![](/assets/gitlab.png)

Gitlab is one of the most popular collaboration software suites around.  It is not only a CI server, but a source code server, docker registry, issue manager, and much much more.  They are our personal favorite for private projects due to their power and flexibility.

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