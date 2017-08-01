# Continuous Integration (CI)

![](/assets/The-Existing-Challenges-of-Continuous-Integration-CI.png)

[Continuous Integration](https://en.wikipedia.org/wiki/Continuous_integration) (CI) is a development process that requires developers to **integrate** their code into a shared source code repository (git,svn,mercurial,etc) several times a day, while a monitoring process detects code commits and acts upon those commits.  Those actions can be the actual checkout of branches, execution of build processes, quality control, and of course our favorite; automated testing.

TestBox can integrate with all major CI servers as all you need to do is be able to execute your test suites and produce reports.  You can see that in our [Running Tests](/running_tests/README.md) section and [Reporters](/reporters/README.md).

## Benefits of Continuous Integration

* Decrease the feedback loop
* Discover defects faster before production releases
* Developer Accountability
* Increase code visibility and promote communication
* Increase quality control
* Reduce integration issues with other features
* Much More...