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
