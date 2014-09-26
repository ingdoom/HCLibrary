HCLibrary
=========

Objective-C library for REST client-server coding.


How-To
=========
At first, create your request (for example, your web-service has GET-method "login" that requires one "username" parameter and returns "SessionUID" of type "string" as a result).
1. Create your own class "LoginHttpRequest" inherited from "HCHttpRequest" class.
2. Declare "in" data readonly properties which are required for your request to proceed. For example, declare "username" property of string type.
3. Declare "out" data readonly properties which describe your request's response context. Your "out" structures can be inherited from "HCStructInfo" class.
4. Declare a specific "initWith..." public constructor for your request, call super init implementation inside it to specify HTTP request type and HTTP operation type, and also to specify relative URL of your request. Inside an initializer you can set your "in" properties and setup your request parameters (such as silent/non-silent processing, specific timeout, bad request auto-repeating and so on). Also you must setup GET/POST parameters for further processing in an "HCHttpService" class.
5. Override "processResponse:" method which should set your "out" properties when received valid response from a server.
6. Also you can override "processError:" method to modify your "out" properties depending on server errors.

At second, create your processor. Processors are proxy singletons which control your requests and group them by their propers.
1. Create your own class "AuthProcessor" inherited from "HCProcessor" class.
2. Create a specific delegate "AuthProcessorDelegate" with optional callback method "processor:didAuthWithSUID:".
3. Declare the public method "authWithLogin:" with the single "username" string parameter.
4. Override "instance" method to create specific singleton instance of your processor.
5. Implement this method. Instantiate "LoginHttpRequest" inside with your "username" parameter. Then call "[self performRequest:]" method with the request class instance you've just created.
6. You must override "requestDidFinishLoading:" method to process successfully loaded requests. Inside this method you can analyze request's class to clarify what request you should proceed here. Call your delegate's method "processor:didAuthWithSUID:" if it can respond to this method call.

At last, in your ViewController add implementation of AuthProcessorDelegate, assign your controller's instance as a delegate to AuthProcessor's shared instance (don't forget that processors are singletons). After that you can call "authWithLogin:" method of AuthProcessor's singleton in any time to process response in the delegate's method you've overridden in your ViewController.


Example
=========
Example code can be found in this repository together with library's source code.


Dependencies
=========
- AFNetworking v.2.0
- CocoaLumberjack v.2.0
