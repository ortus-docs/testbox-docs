# Matchers

The `toBe()` matcher represents an equality matcher much how an `$assert.isEqual()` behaves. Below are several of the most common matchers available to you.  However, the best way to see which ones are available is to checkout the [API Docs](apidocs.ortussolutions.com/testbox/current).

```javascript
toBeTrue() :  value to true
toBeFalse() : value to be false
toBe( expected ) : Assert something is equal to each other, no case is required
toBeWithCase( expected ) : Expects with case
toBeNull() : Expects the value to be null
toBeInstanceOf( class ) : To be the class instance passed
toMatch( regex ) : Matches a string with case-sensitivity
toMatchNoCase( regex ) : Matches with no case-sensitivity
toBeTypeOf( type, message ) : Assert the type of the incoming actual data, it uses the internal ColdFusion isValid() function behind the scenes, type can be array, binary, boolean, component, date, time, float, numeric, integer, query, string, struct, url, uuid plus all the ones from isValid()
toBe{type}( [message] ) : Same as above but more readable method name. Example: .toBeStruct(), .toBeArray()
toBeEmpty() : Tests if an array or struct or string or query is empty
toHaveKey( key ) : Tests the existence of one key in a structure or hash map
toHaveDeepKey( key ) : Assert that a given key exists in the passed in struct by searching the entire nested structure
toHaveLength( length ) : Assert the size of a given string, array, structure or query
toThrow( [type], [message] );
toBeCloseTo( expected, delta, [datepart] ) : Can be used to approximate numbers or dates according to the expected and delta arguments.  For date ranges use the datepart values.
toBeBetween( min, max ) : Assert that the passed in actual number or date is between the passed in min and max values
toInclude( needle ) : Assert that the given "needle" argument exists in the incoming string or array with no case-sensitivity, needle in a haystack anyone?
toIncludeWithCase( needle ) : Assert that the given "needle" argument exists in the incoming string or array with case-sensitivity, needle in a haystack anyone?
toBeGT( target ) : Assert that the actual value is greater than the target value
toBeGTE( target ) : Assert that the actual value is greater than or equal the target value
toBeLT( target ) : Assert that the actual value is less than the target value
toBeLTE( target ) : Assert that the actual value is less than or equal the target value
```


