# What's Included

The download structure includes:

* apidocs : The API docs for TestBox
* tests : Several sample tests and runners which actually are used to build TestBox
* system : The main system framework folder
* test-browser : This is a little utility to facilitate navigating big testing suites. This helps navigate to the suites you want and execute them instead of typing all the time.
* test-runner : The pre-built TestBox Global Runner. You can pass in a directory mapping or bundles and select labels and boom run the tests.
* test-harness : A simplified version of the TestBox runner that can be placed anywhere in your application. It includes an ANT build that allows you to execute your tests and produce results via ANT and also JUnit compliant reports via the junitreport task.

The ONLY required folder is "system" and it does not need to be web-accessable. The other folders are all optional tools to help you that expect to be web-accessible. You can safely remove everything except the system folder if you want to build your own test browsers. If you are placing TestBox outside of your web root, the "/testbox" mapping needs to point to the parent folder containing the "system" directory as all TestBox models start with testbox.system.

