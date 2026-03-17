---
icon: solar-system
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/testbox-xunit-primer
---

# xUnit Tests

Unit testing is a software testing technique where individual components of a software application, known as units, are tested in isolation to ensure they work as intended. Each unit is a small application part, such as a function or method, and is tested independently from other parts. This helps identify and fix bugs early in the development process, ensures code quality, and facilitates easier maintenance and refactoring. Tools like TestBox allow developers to create and run automated unit tests, providing assertions to verify the correctness of the code.

TestBox supports **xUnit** style of testing, like in other languages, via the creation of classes and functions that denote the tests to execute.  You can then evaluate the test either using [assertions](assertions.md) or the [expectations](../../digging-deeper/expectations/) library included with TestBox.

You will start by creating a test bundle (Usually with the word `Test` in the front or back), example: `UserServiceTest` or `TestUserService`.

```cfscript
component labels="disk,os" extends="testbox.system.BaseSpec" {

	/*********************************** LIFE CYCLE Methods ***********************************/

	function beforeTests(){
		application.salvador = 1;
	}

	function afterTests(){
		structClear( application );
	}

	function setup(){
		request.foo = 1;
	}

	function teardown(){
		structDelete( request, "foo" );
	}

	/*********************************** Test Methods ***********************************/

	function testFloatingPointNumberAddition() output="false"{
		var sum = 196.4 + 196.4 + 180.8 + 196.4 + 196.4 + 180.8 + 609.6;
		// sum.toString() outputs: 1756.8000000000002
		// debug( sum );
		// $assert.isEqual( sum, 1756.8 );
	}

	function testIncludes(){
		$assert.includes( "hello", "HE" );
		$assert.includes( [ "Monday", "Tuesday" ], "monday" );
	}

	function testIncludesWithCase(){
		$assert.includesWithCase( "hello", "he" );
		$assert.includesWithCase( [ "Monday", "Tuesday" ], "Monday" );
	}

	function testnotIncludesWithCase(){
		$assert.notincludesWithCase( "hello", "aa" );
		$assert.notincludesWithCase( [ "Monday", "Tuesday" ], "monday" );
	}

	function testNotIncludes(){
		$assert.notIncludes( "hello", "what" );
		$assert.notIncludes( [ "Monday", "Tuesday" ], "Friday" );
	}

	function testIsEmpty(){
		$assert.isEmpty( [] );
		$assert.isEmpty( {} );
		$assert.isEmpty( "" );
		$assert.isEmpty( queryNew( "" ) );
	}

	function testIsNotEmpty(){
		$assert.isNotEmpty( [ 1, 2 ] );
		$assert.isNotEmpty( { name : "luis" } );
		$assert.isNotEmpty( "HelloLuis" );
		$assert.isNotEmpty(
			querySim(
				"id, name
			1 | luis"
			)
		);
	}

	function testSkipped() skip{
		$assert.fail( "This Test should fail" );
	}
```

