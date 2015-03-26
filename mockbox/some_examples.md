# Some Examples

## The Collaborator

The premise of a collaborator is that it is an object that collaborates with another object for some purpose. Usually this is a dependency of some sort. Now, you can have a collaborator that is injected into the target object by some form of dependency injection, or it is created by the target object. Either way, you need to be able to mock the behavior of this collaborator object. The latter approach is much harder to test because you are doing a createObject() call. However, you need to put into practice your testable and refactor skills to work, in order to make this more testable. Thanks Marc Esher!

Example:

```javascript
//hard to mock in a method call
function checkEmail(email){
	var validator = createObject("component","model.util.Validator");

	return validator.isEmail(arguments.email);
}
```

The source above is too hard to test because you have no control over the create object call. Therefore you can have two solutions for this, which actually makes your code more testable.

1. Have a method return to you an instance of the Validator object.
2. Have the Validator object be injected via dependency injection into your target object.

Most of the time collaborators can be injected by dependency injection, but sometimes they will not as they are only in use by the target object. So let's do both approaches, starting with dependency injection via annotations: cfproperty.

```javascript
<cfproperty name="Validator" type="model" instance="scope" />

//or setter injection

<cffunction name="setValidator" access="public" returntype="void" output="false">
	<cfargument name="validator" type="any">
	<cfset instance.validator = arguments.validator>
</cffunction>
```

Now that we have a annotation injection or a setter injection, let's mock the collaborator into our target object with the mocked method for isValid email.

```javascript
targetObject = createObject("component","model.TargetObject");
//prepare it for mocking, don't remove any methods
prepareMock(targetObject);

//Create our mock collaborator
mockValidator = getMockBox().createEmptyMock('model.Validator');
//Mock the isEmail valid method
mockValidator.$("isEmail",true);

//Inject it with setter injection
targetObject.setValidator(mockValidator);

//Inject it via mock property
targetObject.$property("validator","instance",mockValidator);
```

There you go. Now let's refactor our method call:


```javascript
function checkEmail(email){
	return instance.validator.isEmail(arguments.email);
}
//or
function checkEmail(email){
	return getValidator().isEmail(arguments.email);
}

```

As you can see, even our method look cleaner and sharper thanks to our refactoring. However, the most important aspect of our refactoring is that now we can mock the collaborator.

## Method Spies

The premise of method spies is that you want to mock the return of method calls inside the target object that we are testing because these methods are helper methods. Also, maybe these helper methods have an access type of private which we cannot mock directly. Here is a typical example in one of my security objects:

```javascript
function userValidator(rule){
//validate a user
var user = getUserSession();

if( user.isAuthorized() ){
	return true;
}

//Check if single sign on cookie exists and is valid
if( isSSOCookieValid() ){
	authorizeUser(user);
}
else{
	return false;
}
}
```

Ok, this simple liner security validator throws tons of problems. Why? Well, I have three internal private calls:

* getUserSession() - supposed to return a nice user object
* isSSOCookieValid() - checks if a single sign on cookie is set
* authorizeUser() - Authorize a user in my system

Since I am unit testing this target object's `userValidator` method, I don't really care about the method calls, I just worry about the behavior that is associated with them because my other tests will go into detail about what they do. However, for my purposes of testing the `userValidator` method, I just need behavior. Thus, we need to be able to create mocking representations of these methods.

```javascript
function beforeAll(){
	//target object
	security = prepareMock( createObject("component","model.Security") );
	//create a mock user object
	mockUser = createEmptyMock("model.User");
}

function run(){

	describe( "A user", function(){
		it( "can be authenticated", function(){
			//mock user authorizations according to case calls
			mockUser.$("isAuthorized").$results(true,false,false);
			security.$("getUserSession", mockUser);

			//case 1: authorized user
			expect(security.userValidator()).toBeTrue();

			//case 2: unauthorized user with invalid cookie
			security.$("isSSOCookieValid",false);
			expect( security.userValidator() ).toBeFalse();

			//case 3: unauthorized user with valid cookie
			security.$("isSSOCookieValid",true);
			//mock the authorizeUser void call
			security.$("authorizeUser");
			expect( security.userValidator() ).toBeTrue();
		});
	});

}
```

As you can see from the example above, I mocked all the necessary data and objects to test all the paths that I could follow in my user validator method. Now we are cooking with MockBox.

