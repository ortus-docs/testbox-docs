# Assertions
Assertions are self-concatenated strings that evaluate an actual value to an expected value or condition. These are initiated by the global TestBox variable called $assert which contains tons of included assertion methods so you can evaluate your tests.

<h3 style="color:grey">Evaluators</h3>

Each assertion evaluator will compare the actual value and an expected value or condition. It is responsible for either passing or failing this evaluation and reporting it to TestBox. Each evaluator also has a negative counterpart assertion by just prefixing the call to the method with a not expression.

```javascript
      function testIncludes(){
          $assert.includes( "hello", "HE" );
          $assert.includes( [ "Monday", "Tuesday" ] , "monday" );
     }

     function testNotIncludes(){
          $assert.notIncludes( "hello", "what" );
          $assert.notIncludes( [ "Monday", "Tuesday" ] , "Friday" );
     }
```

<h3 sylte="color:grey">Included Evaluators</h3>

TestBox has a plethora (That's Right! I said Plethora) of evaluators that are included in the release. The best way to see all the latest evaluator methods is to visit our [API](http://www.coldbox.org/api) and digest the *coldbox.system.Assertion* class. There is also the ability to register and write custom assertion evaluators in TestBox via our addAssertions() function.

```javascript
component displayName="TestBox xUnit suite for CF9" labels="railo,cf"{

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
          structClear( request );
     }

/*********************************** Test Methods ***********************************/

     function testIncludes(){
          $assert.includes( "hello", "HE" );
          $assert.includes( [ "Monday", "Tuesday" ] , "monday" );
     }

     function testIncludesWithCase(){
          $assert.includesWithCase( "hello", "he" );
          $assert.includesWithCase( [ "Monday", "Tuesday" ] , "Monday" );
     }

     function testnotIncludesWithCase(){
          $assert.notincludesWithCase( "hello", "aa" );
          $assert.notincludesWithCase( [ "Monday", "Tuesday" ] , "monday" );
     }

     function testNotIncludes(){
          $assert.notIncludes( "hello", "what" );
          $assert.notIncludes( [ "Monday", "Tuesday" ] , "Friday" );
     }

     function testIsEmpty(){
          $assert.isEmpty( [] );
          $assert.isEmpty( {} );
          $assert.isEmpty( "" );
          $assert.isEmpty( queryNew("") );
     }

     function testIsNotEmpty(){
          $assert.isNotEmpty( [1,2] );
          $assert.isNotEmpty( {name="luis"} );
          $assert.isNotEmpty( "HelloLuis" );
          $assert.isNotEmpty( querySim( "id, name
               1 | luis") );
     }

     function testSkipped() skip{
          $assert.fail( "This Test should fail" );
     }

     boolean function isRailo(){
          return structKeyExists( server, "railo" );
     }

     function testSkippedWithConstraint() skip="isRailo"{
          $assert.fail( "This Test should fail" );
     }

     function testFails(){
          //$assert.fail( "This Test should fail" );
     }

     function testFailsShortcut() labels="railo"{
          //fail( "This Test should fail" );
     }

     function testAssert() {
          $assert.assert( application.salvador == 1 );
     }

     function testAssertShortcut() {
          assert( application.salvador == 1 );
     }

     function testisTrue() {
          $assert.isTrue( 1 );
     }

     function testisFalse() {
          $assert.isFalse( 0 );
     }

     function testisEqual() {
          $assert.isEqual( 0, 0 );
          $assert.isEqual( "hello", "HEllO" );
          $assert.isEqual( [], [] );
          $assert.isEqual( [1,2,3, {name="hello", test="this"} ], [1,2,3, {test="this", name="hello"} ] );
     }

     function testisNotEqual() {
          $assert.isNotEqual( this, new coldbox.system.MockBox() );
          $assert.isNotEqual( "hello", "test" );
          $assert.isNotEqual( 1, 2 );
          $assert.isNotEqual( [], [1,3] );
     }

     function testisEqualWithCase() {
          $assert.isEqualWithCase( "hello", "hello" );
     }

     function testnullValue() {
          $assert.null( javaCast("null", "") );
     }

     function testNotNullValue() {
          $assert.notNull( 44 );
     }

     function testTypeOf() {
          $assert.typeOf( "array", [ 1,2 ] );
          $assert.typeOf( "boolean", false );
          $assert.typeOf( "component", this );
          $assert.typeOf( "date", now() );
          $assert.typeOf( "time", timeformat( now() ) );
          $assert.typeOf( "float", 1.1 );
          $assert.typeOf( "numeric", 1 );
          $assert.typeOf( "query", querySim( "id, name
               1 | luis") );
          $assert.typeOf( "string", "hello string" );
          $assert.typeOf( "struct", { name="luis", awesome=true } );
          $assert.typeOf( "uuid", createUUID() );
          $assert.typeOf( "url", "http://www.coldbox.org" );
     }

     function testNotTypeOf() {
          $assert.notTypeOf( "array", 1 );
          $assert.notTypeOf( "boolean", "hello" );
          $assert.notTypeOf( "component", {} );
          $assert.notTypeOf( "date", "monday" );
          $assert.notTypeOf( "time", "1");
          $assert.notTypeOf( "float", "Hello" );
          $assert.notTypeOf( "numeric", "eeww2" );
          $assert.notTypeOf( "query", [] );
          $assert.notTypeOf( "string", this );
          $assert.notTypeOf( "struct", [] );
          $assert.notTypeOf( "uuid", "123" );
          $assert.notTypeOf( "url", "coldbox" );
     }

     function testInstanceOf() {
          $assert.instanceOf( new coldbox.system.MockBox(), "coldbox.system.MockBox" );
     }

     function testNotInstanceOf() {
          $assert.notInstanceOf( this, "coldbox.system.MockBox" );
     }

     function testMatch(){
          $assert.match( "This testing is my test", "(TEST)$" );
     }

     function testMatchWithCase(){
          $assert.match( "This testing is my test", "(test)$" );
     }

     function testNotMatch(){
          $assert.notMatch( "This testing is my test", "(hello)$" );
     }

     function testKey(){
          $assert.key( {name="luis", awesome=true}, "awesome" );
     }

     function testNotKey(){
          $assert.notKey( {name="luis", awesome=true}, "test" );
     }

     function testDeepKey(){
          $assert.deepKey( {name="luis", awesome=true, parent = { age=70 } }, "age" );
     }

     function testNotDeepKey(){
          $assert.notDeepKey( {name="luis", awesome=true, parent = { age=70 } }, "luis" );
     }

     function testLengthOf(){
          $assert.lengthOf( "heelo", 5 );
          $assert.lengthOf( [1,2], 2 );
          $assert.lengthOf( {name="luis"}, 1 );
          $assert.lengthOf( querySim( "id, name
               1 | luis"), 1 );

     }

     function testNotLengthOf(){
          $assert.notLengthOf( "heelo", 3 );
          $assert.notLengthOf( [1,2], 5 );
          $assert.notLengthOf( {name="luis"}, 5 );
          $assert.notLengthOf( querySim( "id, name
               1 | luis"), 0 );

     }

     // railo 4.1+ or CF10+
      function testThrows(){
          $assert.throws(function(){
               var hello = invalidFunction();
          });
     }

      // railo 4.1+ or CF10+
     function testNotThrows(){
          $assert.notThrows(function(){
               var hello = 1;
          });
     }

/*********************************** NON-RUNNABLE Methods ***********************************/

     function nonStandardNamesWillNotRun() {
          fail( "Non-test methods should not run" );
     }

     private function privateMethodsDontRun() {
          fail( "Private method don't run" );
     }

}
```

<h3 style="color:grey">Custom Assertions</h3>

You can also register custom assertions within the $assert object. You will do this by reading our Custom Assertions section of our TestBox docs.
