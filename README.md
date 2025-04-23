# Udemy Course Spring Security Section7
## spring version: 3.4.4

## Docker container for MySQL
docker run -p 3306:3306 --name springsecurity -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=eazybank -d mysql


## Testing HTTPS with pord environment variable set
21:24:28.002 INFO  [restartedMain] c.e.EasyBankBackendApplication - **The following 1 profile is active: "prod"**

**My browser:** 
Redirects me to https://localhost:8443/myAccount?continue with an message about unavailability.

**Postman:**
GET http://localhost:8080/myAccount
Error: connect ECONNREFUSED ::1:8443

### solution
We start the application with non-prod profile.


## Testing CustomBasicAuthenticationEntryPoint customised error header

Postman:
GET http://localhost:8080/myAccount
response body: 
{"timestamp":"2025-04-15T13:44:49.628+00:00","status":401,"error":"Unauthorized","message":"Unauthorized","path":"/myAccount"}

And we can see header: 
Response Headers
eazybank-error-reason: Authentication failed
...


## Testing CustomBasicAuthenticationEntryPoint customised error message

Postman:
GET http://localhost:8080/myAccount
{"timestamp":"2025-04-15T16:15:18.705367700","status":401,"error":"Unauthorized","message":"User not found for the username: smith@example.com1","path":"/myAccount"}


## Testing CustomAccessDeniedHandler

Postman:
GET http://localhost:8080/myAccount1    //wrong address
{"timestamp":"2025-04-15T19:34:29.900786600","status":403,"error":"Forbidden","message":"Access Denied","path":"/myAccount1"}

and one of response headers:
eazybank-denied-reason: Authorization failed


The lecturer got the same JSON in hos browser. 
I'm getting some error page translated to czech. This might be some browser setting.
I tried to set an breakpoint in the CustomAccessDeniedHandler and it sent the JSON right.


## Session Timeout

After the session expires, the browser redirects to the login page.

### invalid session page
Now we get 404 in the browser after the timeout, because we did not build the /invalidSession page.
But the value in the address bar is: http://localhost:8080/invalidSession


## Active session testing
### Postman login
GET http://localhost:8080/myAccount
200
...
Request Headers
Authorization: Basic c21pdGhAZXhhbXBsZS5jb206RWF6eUJ5dGVzQDEyMzQ1
...
Cookie: JSESSIONID=755662EE1F2F2A4A70544ECF9F0B9850

Response Headers
Set-Cookie: JSESSIONID=92BCD62235ECCD4E0BA3AD33FEA19ECB; Path=/; HttpOnly
...
Response Body
Here are the account details from the DB

### Postman No Auth
01:13:51.538
GET http://localhost:8080/myAccount
200
...
Request Headers
...
Cookie: JSESSIONID=92BCD62235ECCD4E0BA3AD33FEA19ECB

Response Headers
...
Response Body
Here are the account details from the DB


## Session expiration testing
### Postman login
GET http://localhost:8080/myAccount
200
17 ms
Network
addresses: {…}
Request Headers
Authorization: Basic c21pdGhAZXhhbXBsZS5jb206RWF6eUJ5dGVzQDEyMzQ1
User-Agent: PostmanRuntime/7.43.3
Accept: */*
Cache-Control: no-cache
Postman-Token: e5e0868b-05ab-4086-948b-abd3f1c99d91
Host: localhost:8080
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Response Headers
Set-Cookie: JSESSIONID=579712748D7E04D938A332D075EDF2C5; Path=/; HttpOnly
X-Content-Type-Options: nosniff
X-XSS-Protection: 0
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
Pragma: no-cache
Expires: 0
X-Frame-Options: DENY
Content-Type: text/plain;charset=UTF-8
Content-Length: 40
Date: Wed, 23 Apr 2025 10:08:44 GMT
Keep-Alive: timeout=60
Connection: keep-alive
Response Body
Here are the account details from the DB

### Browser refresh attempt (user was logged in before)
Request headers:
GET /myAccount HTTP/1.1
...
Cookie: JSESSIONID=053E977CE9459B78C5086551E2ECD1A2
...

Response headers:
HTTP/1.1 200
...
Expires: 0
X-Frame-Options: DENY
...

Response:
**This session has been expired (possibly due to multiple concurrent logins being attempted as the same user).**

This was a bit strange, because the lecturer achieved this in the Postman while I did in the browser.
I could not achieve the session expired message in the Postman. 

