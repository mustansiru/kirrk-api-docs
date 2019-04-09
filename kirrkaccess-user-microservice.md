# User microservice
## 1. Register Kirrk Access User
```http
POST /acess-user
```
### Input
```JSON
{
  "emailAddress": "string",
  "password": "string",
  "firstname": "string",
  "lastname": "string",
  "phoneNumber": "string",
  "birthday": "2019-04-09T11:12:47.229Z"
}
```
### Output
- ##### Success
```http
Response 200 OK
authToken
```
- ##### Exception
```http
Response 500 Internal Server Error
```
On successful user creation, the user will be automatically logged in and the auth token will be sent as a result.

### Events published:
#### `KirrkAccessUserAccountCreatedEvent`
-- Namespace: `Kirrk.Services.Common.Events`
-- Model:
```JSON
{
    KirrkAccessUserId,
    Firstname,
    Lastname,
    PhoneNumber,
    Birthday,
    Address,
    City,
    PostalCode,
    Country
}
```
#### `KirrkAccessUserEmailValidationCodeNeededEvent`
-- Namespace: `Kirrk.Services.Common.Events`
-- Model:
```JSON
{
    EmailAddress,
    Firstname,
    EmailValidationCode
}
```
### Maps to FE Screen
[2. Sign Up](https://projects.invisionapp.com/d/main#/console/16190609/349016651/preview#project_console)

## 2. Verify if email already exists in system
```http
GET /access-user/email/{email}
```
### Input
```JSON
{
    "email": "string"
}
```
### Output
- ##### Success
```http
Response 200 OK
true/false
```
- ##### Exception
```http
Response 500 Internal Server Error
```
Check if the email id provided already exists in the system.

### Events published:
None
### Maps to FE Screen
[2. Signup - Checking...](https://projects.invisionapp.com/d/main#/console/16190609/350270976/preview)

## 3. Check if email validation is required
```http
POST /access-user/validate-email
```
### Input
```JSON
{
    "email":"string"
}
```
### Output
```http
Response 200 OK
true/false
```
If the email has been verified, then the output is false, else true.

### Events published:
None

### Maps to FE Screen
[2. Sign Up Copy](https://projects.invisionapp.com/d/main#/console/16190609/350270975/preview)


## 4. Generate an email validation code
```http
POST /access-user/validate-email/send
```
### Input
```JSON
{
    "email":"string"
}
```
### Output
```http
Response 200 OK
```
```http
Response 400 Bad Request: That user does not exist
```
```http
Response 500 Internal Server Error
```
Generate an email validation code. This API can be called to regenerate an email validation code on user's request.

### Events published:
#### `KirrkAccessUserEmailValidationCodeNeededEvent`
-- Namespace: `Kirrk.Services.Common.Events`
-- Model:
```JSON
{
    EmailAddress,
    Firstname,
    EmailValidationCode
}
```

### Maps to FE Screen
[2. Sign Up Copy](https://projects.invisionapp.com/d/main#/console/16190609/350270975/preview)


## 5. Validate email using the generated code
```http
POST /access-user/validate-email/{code}
```
### Input
```JSON
{
    "code":"string",
    "email":"string"
}
```
### Output
```http
Response 200 OK
[output object]
```
```http
Response 400 Bad Request: 
- The email is already validated.
- The email validation code is invalid.
```
```http
Response 500 Internal Server Error
```
Validate email address using the code shared with the user.

### Events published:
None

### Maps to FE Screen
External Web App page redirected from the email validation link.

