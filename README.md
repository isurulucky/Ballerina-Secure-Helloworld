# Ballerina-Secure-Helloworld
Hello World service written in Ballerina (https://ballerinalang.org/), secured with Basic authentication. 
Authorization works with scopes, where a scope maps to one or more groups. An API resource can be secured with a scope, 
and a user who needs to access the particular resource should have the relevant groups assigned. 

### How to use
1. Add usernames, a random user id mapping, and sha256 hash of the passwords in ballerina.conf file.
2. Assign group(s) to the user.
3. Define scopes and assign group(s) to scopes.
4. See the sample below on how to engage authentication and authorization in your service:
   ```
   service<http> helloWorld {
   
       resource sayHello (http:Connection conn, http:InRequest req) {
   
           http:OutResponse res = {};
           if(!basic:handle(req)) {
               res = {statusCode:401, reasonPhrase:"Unauthenticated"};
               // currently, need to pass the scope and the resource name to the method call for the authorization
           } else if (!authorization:handle(req, "scope2", "/sayHello")) {
               res = {statusCode:403, reasonPhrase:"Unauthorized"};
           } else {
               res.setStringPayload("Hello, World!!");
           }
           _ = conn.respond(res);
       }
   }
   ```
5. Start the service with the following command:
   ```
   ballerina run secure-helloworld.bal
   ```
6. Invoke the service with the correct basic authenication header, relevant to your username and password:
   ```
   curl -v -H "Authorization: Basic xxxxx" http://localhost:9090/helloWorld/sayHello
   ```   
##### Note: Please see the ballerina.conf file included for the format of the userstore and other configurations.
