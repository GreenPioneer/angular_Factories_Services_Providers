# angular Factories_Services_Providers


## Example One - The Big Three

Each web application you build is composed of objects that collaborate to get stuff done. These objects need to be instantiated and wired together for the app to work. In Angular apps most of these objects are instantiated and wired together automatically by the injector service.The injector creates two types of objects, services and specialized objects.Services are objects whose API is defined by the developer writing the service.The most verbose, but also the most comprehensive one is a Provider recipe. The remaining four recipe types — Value, Factory, Service and Constant — are just syntactic sugar on top of a provider recipe.

### Factory 
Syntax: 
```javascript
    app.factory( 'factoryName', function );
```
* Overview: When you’re using a Factory you create an object, add properties to it, then return that same object. When you pass this factory into your controller, those properties on the object will now be available in that controller through your factory.
* Result: When declaring factoryName as an injectable argument you will be provided the value that is returned by invoking the function reference passed to app.factory.
* Usage: Could be useful for returning a 'class' function that can then be new'ed to create instances.

 
### Service 
Syntax: 
```javascript
    app.service( 'serviceName', function );
```
* Overview: When you’re using Service, AngularJS instantiates it behind the scenes with the ‘new’ keyword. Because of that, you’ll add properties to ‘this’ and the service will return ‘this’. When you pass the service into your controller, those properties on ‘this’ will now be available on that controller through your service.
* Result: When declaring serviceName as an injectable argument you will be provided with the instance of a function passed to app.service.
* Usage: Could be useful for sharing utility functions that are useful to invoke by simply appending () to the injected function reference. Could also be run with injectedArg.call( this ) or similar.

 
### Provider 
Syntax: 
```javascript
    app.provider( 'providerName', function );
    app.config(function (providerNameProvider) {
      //set a config here for the provider
    });
```
* Overview: Providers are the only service you can pass into your .config() function. 
* Result: exposes an API for application-wide configuration that must be made before the application starts.
* Usage: Use a provider when you want to provide module-wide configuration for your service object before making it available.

## Example Two - Providers

```javascript
    myApp.provider('unicornLauncher', function UnicornLauncherProvider() {
      var useTinfoilShielding = false;

      this.useTinfoilShielding = function(value) {
        useTinfoilShielding = !!value;
      };

      this.$get = ["apiToken", function unicornLauncherFactory(apiToken) {

        // let's assume that the UnicornLauncher constructor was also changed to
        // accept and use the useTinfoilShielding argument
        return new UnicornLauncher(apiToken, useTinfoilShielding);
      }];
    });
    myApp.config(["unicornLauncherProvider", function(unicornLauncherProvider) {
      unicornLauncherProvider.useTinfoilShielding(true);
    }]);
```

## Example Three - Factories

```javascript
    myApp.factory('clientId', function clientIdFactory() {
      return 'a12345654321x';
    });
    myApp.factory('apiToken', ['clientId', function apiTokenFactory(clientId) {
      var encrypt = function(data1, data2) {
        // NSA-proof encryption algorithm:
        return (data1 + ':' + data2).toUpperCase();
      };

      var secret = window.localStorage.getItem('myApp.secret');
      var apiToken = encrypt(clientId, secret);

      return apiToken;
    }]);
```

## Example Four - Services

```javascript
    myApp.factory('unicornLauncher', ["apiToken", function(apiToken) {
      return new UnicornLauncher(apiToken);
    }]);

    myApp.service('unicornLauncher', ["apiToken", UnicornLauncher,function(apiToken){
        this.launchedCount = 0;
        this.launch = function() {
            // Make a request to the remote API and include the apiToken
            ...
            this.launchedCount++;
        }
    }]);
```

## Example Five - Value

```javascript
    var myApp = angular.module('myApp', []);
    myApp.value('clientId', 'a12345654321x');

    myApp.controller('DemoController', ['clientId', function DemoController(clientId) {
      this.clientId = clientId;
    }]);
```
```html
    <html ng-app="myApp">
      <body ng-controller="DemoController as demo">
        Client ID: {{demo.clientId}}
      </body>
    </html>
```

## Example Six - Constant

```javascript
    myApp.constant('planetName', 'Greasy Giant');

    myApp.config(['unicornLauncherProvider', 'planetName', function(unicornLauncherProvider, planetName) {
      unicornLauncherProvider.useTinfoilShielding(true);
      unicornLauncherProvider.stampText(planetName);
    }]);
```



#### This is [on GitHub](https://github.com/GreenPioneer/angular_Factories_Services_Providers)
#### Find us [on GitHub](https://github.com/GreenPioneer)
#### Find us [on Twitter](https://twitter.com/greenpioneerdev)
#### Find us [on Facebook](https://www.facebook.com/Green-Pioneer-Solutions-1023752974341910)
#### Find us [on The Web](http://greenpioneersolutions.com/)