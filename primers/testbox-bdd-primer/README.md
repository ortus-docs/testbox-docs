# TestBox BDD Primer

**BDD** stands for Behavioral Driven Development. It is a software development process that aims to improve collaboration between developers, testers, and business stakeholders. BDD involves creating automated tests that are based on the expected behavior of the software, rather than just testing individual code components. This approach helps ensure that the software meets the desired functionality and is easier to maintain and update in the future.

In traditional xUnit, you focused on every component's method individually. In BDD, we will focus on a feature or story to complete, which could include testing many different components in order to satisfy the criteria.  TestBox allows us to create these types of texts with human-readable functions that can match our features/stories and expectations.

```cfscript
describe( "Tests of TestBox behaviour", () => {
	it( "rejects 5 as being between 1 and 10", () => {
		expect( () => {
			expect( 5 ).notToBeBetween( 1, 10 );
		} ).toThrow();
	} );
	
	it( "rejects 10 as being between 1 and 10", () => {
		expect( () => {
			expect( 10 ).notToBeBetween( 1, 10 );
		} ).toThrow();
	} );
} );


feature( "Given-When-Then test language support", () => {
	scenario( "I want to be able to write tests using Given-When-Then language", () => {
		given( "I am using TestBox", () => {
			when( "I run this test suite", () => {
				then( "it should be supported", () => {
					expect( true ).toBe( true );
				} );
			} );
		} );
	} );
} );

story( "I want to list all authors", () => {
    given( "no options", () => {
        then( "it can display all active system authors", () => {
            var event = this.get( "/cbapi/v1/authors" );
            expect( event.getResponse() ).toHaveStatus( 200 );
            expect( event.getResponse().getData() ).toBeArray().notToBeEmpty();
            event
                .getResponse()
                .getData()
                .each( function( thisItem ){
                    expect( thisItem.isActive ).toBeTrue( thisItem.toString() );
                } );
        } );
    } );
    given( "isActive = false", () => {
        then( "it should display inactive users", () => {
            var event = this.get( "/cbapi/v1/authors?isActive=false" );
            expect( event.getResponse() ).toHaveStatus( 200 );
            expect( event.getResponse().getData() ).toBeArray().notToBeEmpty();
            event
                .getResponse()
                .getData()
                .each( function( thisItem ){
                    expect( thisItem.isActive ).toBeFalse( thisItem.toString() );
                } );
        } );
    } );
    given( "a search criteria", () => {
        then( "it should display searched users", () => {
            var event = this.get( "/cbapi/v1/authors?search=tester" );
            expect( event.getResponse() ).toHaveStatus( 200 );
            expect( event.getResponse().getData() ).toBeArray().notToBeEmpty();
        } );
    } );
} );
```
