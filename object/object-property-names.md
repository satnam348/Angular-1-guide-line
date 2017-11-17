# Code Guidelines

### Object Naming Conventions

Legacy browsers such as IE8 and Android 2.x only support ES3 subset of JS, thus any code using keywords as object property names will break in these legacy browsers. In addition use of these keywords as object property names breaks the yui-compressor used to minify the javascript source code. 
 
  Thus ALL reserved words should be avoided as property names. 
 
    http://www.w3schools.com/js/js_reserved.asp
 
and this list includes a break out for Java keywords that are reserved by Javascript for cross language binding support.
 
    http://www.quackit.com/javascript/javascript_reserved_words.cfm
 
 
The work around to this is to find a better property name, or to use the string index notation as suggested:
 
`myObject[‘delete’] = function () {…};`
 
 
I prefer the selection of better property names; thus instead of 
```
myObject = {
  delete: function(arg){...}
  catch: function(arg){...}
  value: 42
}
```

you should find better property names that are not keywords:
```
myObject = {
  deleteValue: function(arg){...}
  catchError: function(arg){...}
  value: 42
}
```
 
You can find more information in the Angular docs:
 
    https://docs.angularjs.org/guide/ie
 
Although, the AngularJS framework contains the $http service which is an object that defines various methods for XHR:

```
$http.get
$http.head
$http.post
$http.put
$http.delete
$http.jsonp
$http.patch
```

Which belies this recommendation.  There is a long discussion that this breaks in old IE and Android browsers (ES3 only), and that it breaks yui-compressor. 
 
  https://github.com/angular/angular.js/issues/1265
 
The recommendations were to use the string index notation while the method is deprecated and has been replaced with the alias `remove`.
 
Although every `$http` method returns a promise that include the following methods (`then`, `catch`, `finally`) and the deprecated (`success`, `error`).
 
```
$http
    .get('http://someendpoint/maybe/returns/JSON')
    
    .then(function(response) {
        return response.data;
    })
    
    .catch(function(e) {
        console.log('Error: ', e);
        throw e;
    })
    
    .finally(function() {
        console.log('This finally block');
    });
```
 
Where both `catch` and `finally` are reserved keywords
 
    "Because finally is a reserved word in JavaScript and reserved keywords 
    are not supported as property names by ES3, you'll need to invoke the 
    method like promise'finally' to make your code IE8 and Android 2.x compatible."
    
    Please do not forget that 'catch' is also causing issues at least with IE8!
    
    https://github.com/angular/angular.js/issues/8902
 
 
In addition the AngularJS ui-router defines application states using state machine config objects.  One of the available properties is to identify `abstract` states:
``` 
$stateProvider
    .state('parent', {
         url: '/home',
         abstract: true,           // <-- abstract 
         template: '<ui-view/>'
    })
    .state('parent.index', {
         url: '/index',
         templateUrl: 'index.html'
    })
    .state('parent.contact', {
         url: '/contact',
         templateUrl: 'contact.html'
     })
```
 
 
Although the `abstract` keyword here is used as a property in a configuration object passed into the $stateProvider service so it is never referenced outside,
yet the use of the keyword as an object property causes problems as discussed above, and in this github discussion:
 
      https://github.com/angular-ui/ui-router/issues/226
       
      And the recommendation was:
     
        nateabele commented on Jul 5, 2013
        
          You can always just quote the key. That's what we do in the source itself.
           
          
Although the try-catch-finally clause is a standard in javascript:
``` 
try{
…
} catch(ex){
…
} finally{
…
}
```
and does not need to be modified when written as standard javascript,
 
only when these keywords are used as property names on an object:
```
$http.get()
.then()
.catch()
.finally()
```
we should then convert access to them as: 
```
$http.get() 
.then()
[‘catch’]()
[‘finally’]()
``` 
 
