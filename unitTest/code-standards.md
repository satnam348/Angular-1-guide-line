# Subject: Coding Standards and Quality Unit Tests.
 
The architect team has been reviewing all unit test fixtures and deleting any fixtures that are not of high quality. 
 
Test Fixtures / Cases (describe/ it) will be deleted if:
 
* Exercises code without verifying either state or behavior. 
* Does not test what the description says it tests.
* Simply verifies that a function exists on the object under test: (this is not a valuable test - see bullet #1)
 
The tenant here is that if you do not write quality code it will be deleted and you will not receive credit for your work.
 
The following are some guidelines to help you write better tests.
 
Please do not include the object type in the describe or it description ( do not use yeoman generator to create test files, it uses a bad pattern ):
 
describe(‘Directive: mySpecialAttribute’, function() {});
 
 
Which leads to proper naming and nesting of test fixtures and cases. 
 
Please create nested describe blocks that mimic the folder path to the object under test:

```js
describe(‘cart’, function() {
  describe(‘cartServices, function() {
    describe(‘httpRequestWrapperService, function() {
     … 
    });
  });
});
```` 
 
Please create a separate describe block for every exposed method of the object under test.
Please name this describe block as the name of the method followed by parenthesis, like a method invoke statement:

 ```js
describe(‘httpRequestWrapperService, function() {

  describe(‘post()’, function() {…});

  describe(‘get()’, function() {…});

  describe(‘put()’, function() {…});

  describe(‘delete()’, function() {…});
});
```

Please use a blank line between all describe and it blocks.  The above example shows blank lines with a comment only to hold the highlight color in outlook email format.
 
Please make each test case for a method have a valid description for what it is testing.
Please make each test case verify 1 thing only, even if you need to write multiple identical test cases each verifying something different. 
 
 ```js
describe(‘get()’, function() {
  it(‘should generate and invoke well formed get request’, function() {…});
});
```
 
if multiple test cases are similar and only change a single input value then wrap in another describe:
 
```js 
describe(‘get()’, function() {
  describe(‘should generate and invoke well formed get request’, function() {
    it(‘when authRequired is false’, function() {…});
    it(‘when authRequired is true, function() {…});
    it(‘when ignoreLoadingBar is in query’, function() {…});
  });
});
```

If a method needs to be tested with a large collection of inputs then create  wrapper function to generate the test cases

```js
function testMyMethod(p1) {
   it(‘should do something with input ‘ + p1, function() {
     objectUnderTest.methodUnderTest(p1);
     expect(objectUnderTest.value).toEqual(p1*3);
   });
}
 
[1,3,5,7,9].forEach(testMyMethod(p));
````
 
Please do not overload jasmine spies.  Loading all your spies in a beforeEach block makes it difficult to see what the environment of the test is, especially when using .and.callFake. to abstract duplication across tests.  The result is your tests need to test themselves as much as the code you are testing.  It is better to duplicate the spy definition in each test case, each spy returning a value specific to that test, unless a group of tests all want the mocked dependency to return the same value..  Returning values is almost ALWAYS better than using stubs (callFake). 
 
We are storing and deploying the application to a *nix server, keep all line endings as LF.
 
 
 It is recommended NOT to use the .success, .error methods exposed from the $http api
 these are deprecated in favor or the promise standard of 
 - .then(successFn, errorFn)
 - .catch(errorFn)
 - .finally(finallyFn)
         
Pleae refer to the following blog:
http://www.benlesh.com/2013/02/angularjs-creating-service-with-http.html
(note: we discourage the use of callback structures in custom objects)
         
Also: Coding Standards:
 
Having different workstations saving files formatted differently causes complex code merges.  
We want to simplify this by having all devs using the same formatting settings
 
Please use LF line endings.  You need to set your IDE to save files with LF line ending, not CRLF. 
CRLF is the standard line ending for windows. 
LF is the standard line ending for *nix systems.

You should also configure your local git client to store all files with LF line endings.

```bash
$ git config core.eol lf
$ git config core.autocrlf input
```
 
We are storing and deploying the application to a *nix server, keep all line endings as LF.
 
Please use spaces, not tab characters.  You can set you IDE to insert spaces for every tab key press.

* For Javascript code please use 2 spaces per tab
* For Java code please use 4 spaces per tab

These are the industry standards for these languages.
 
We want to standardize to one of 2 IDE,  either IntelliJ/Webstorm, from jetbrains.com, or VisualCode from Microsoft.
 
IntelliJ & Webstorm are both superior IDEs but they are not free. 
The current prices are:
```
* IntelliJ:  149/$119/$89     (1st yr, 2nd yr, 3rd yr+)
* WebStorm:  $57/  $49/$35  (1st yr, 2nd yr, 3rd yr+)
```

**IntelliJ** is a superset over **WebStorm** and supports all languages including Java.  
Webstorm is specialized for browser client application development (javascript).
 
Even though these tools have an annual cost they will become invaluable to you as they will teach/coach/coerce you to write better code.
 
The _free option_ is **VisualCode**.  VisualCode is produced by Microsoft,  is open source , runs on the open source electron framework/application.  VisualCode has thousands of free and paid plugins available and is a close second to the intellij tools. 
 
With each of these options we will be publishing an editor config file to assure that all developer workstations are formatting and saving files in compatible formats.
 
Please discontinue the use of notepad++, sublime, eclipse or other text editors as we try to normalize the output of all development workstations.
