[\< bl-api](./summary.md)

# Authorization
- [Login and register](#login-and-register)
- [Tokens](#tokens)
- [Permission levels](#permission-levels) 

# Login and Register
There are several methods for login and register

#### Username and password
- login: `HTTP POST /auth/local/login { "username": "USERNAME", "password": "password"}`
- register: `HTTP POST /auth/local/register { "username": "USERNAME", "password": "password"}`

#### Facebook
- login: `HTTP GET /auth/facebook`
- register: `HTTP GET /auth/facebook`

#### Google
- login: `HTTP GET /auth/google`
- register: `HTTP GET /auth/google`

#### Feide
- login: `HTTP GET /auth/feide`
- register: `HTTP GET /auth/feide`


### Response
When you successfully login or register you will get a response including a access token and a refresh token.
```
{
    "data": [
        {
            "documentName": "refreshToken",
            "data": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJib2tsaXN0ZW4ubm8iLCJhdWQiOiJib2tsaXN0ZW4ubm8iLCJpYXQiOjE2MDA2NzYwNzQsInN1YiI6InUjODQ4MGNjNmFmYTQ5NDZjNmM0YjQ0YzcwNmUyZmU1NTEiLCJ1c2VybmFtZSI6ImFob2xza2lsQGdtYWlsLmNvbSIsImV4cCI6MTYzMjIxMjA3NH0.DxyTSVABtiITpUKIWGZLvDqIA9K1TFezsMf2YFQSBGw"
        },
        {
            "documentName": "accessToken",
            "data": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJib2tsaXN0ZW4ubm8iLCJhdWQiOiJib2tsaXN0ZW4ubm8iLCJpYXQiOjE2MDA2NzYwNzQsInN1YiI6InUjODQ4MGNjNmFmYTQ5NDZjNmM0YjQ0YzcwNmUyZmU1NTEiLCJ1c2VybmFtZSI6ImFob2xza2lsQGdtYWlsLmNvbSIsInBlcm1pc3Npb24iOiJhZG1pbiIsImRldGFpbHMiOiI1Yjc5NjY3N2VjYzhiMDRjMDE5ODI3MTMiLCJleHAiOjE2MDA2NzY2NzR9.1_FTxx2Vmt9nG2FYmFjbxkOUaRZqPqmMmrw-ih40lVo"
        }
    ]
}
```
The [access token](#access-token) must be used in each request afterwards. The [refresh token](#refresh-token) should be stored in client for later use.

### Error response

If the register failed (for example that you provided a bad username) you will
get a `HTTP 400` response. If the login failed (for example that you provided
wrong password) you will get a `HTTP 401`

# Tokens
Json web tokens is a simple to use vertification method. Instead of calling the
database for user data for each request the token itself can contain the
minimal data required. Each request holds the access token in its header as a
`Bearer token`. The system controls this bearer token and vertifies the user on
the fly using encryption methods.

Since each token is signed by a secret only known to the server it can be
certain that the token is valid if the encryption succeeds. If the encryption
step fails (i.e. the user has manually changed the token) the authorization is
rejected.

[Read more about JWT.](https://jwt.io)

## Access token
The access token is used for obtaining data or doing operations that is behind
authorization. If the access token is expired the user must obtain a new one
either by sending the refresh token or by doing a new login.

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJib2tsaXN0ZW4ubm8iLCJhdWQiOiJib2tsaXN0ZW4ubm8iLCJpYXQiOjE2MDA2NzYwNzQsInN1YiI6InUjODQ4MGNjNmFmYTQ5NDZjNmM0YjQ0YzcwNmUyZmU1NTEiLCJ1c2VybmFtZSI6ImFob2xza2lsQGdtYWlsLmNvbSIsInBlcm1pc3Npb24iOiJhZG1pbiIsImRldGFpbHMiOiI1Yjc5NjY3N2VjYzhiMDRjMDE5ODI3MTMiLCJleHAiOjE2MDA2NzY2NzR9.1_FTxx2Vmt9nG2FYmFjbxkOUaRZqPqmMmrw-ih40lVo
```
_TIP: You can copy the access token and paste it in [jwt.io's decoder](https://jwt.io) to see the contents in human readable text_

The access token's payload is consisting of several components.

* iss
  * what domain issued the token
* aud
  * what domain the token is for
* iat
  * creation time of the token
* sub
  * the user-BL-ID for the user
* username
  * the username of the user
* permission
  * what permission the user has
* details
  * the id of the user-detail document of the user
* exp
  * at what time the token expires

## Refresh token
The refresh token is used when the access token is expired. It is used for
obtaining new tokens without doing the login step again. If the refresh token
is expired the user needs to login again.

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJib2tsaXN0ZW4ubm8iLCJhdWQiOiJib2tsaXN0ZW4ubm8iLCJpYXQiOjE2MDA2NzYwNzQsInN1YiI6InUjODQ4MGNjNmFmYTQ5NDZjNmM0YjQ0YzcwNmUyZmU1NTEiLCJ1c2VybmFtZSI6ImFob2xza2lsQGdtYWlsLmNvbSIsImV4cCI6MTYzMjIxMjA3NH0.DxyTSVABtiITpUKIWGZLvDqIA9K1TFezsMf2YFQSBGw
```
_TIP: You can copy the refresh token and paste it in [jwt.io's decoder](https://jwt.io) to see the contents in human readable text_

The refresh token's payload is consisting of several components.

* iss
  * what domain issued the token
* aud
  * what domain the token is for
* iat
  * creation time of the token
* sub
  * the user-BL-ID for the user
* username
  * the username of the user
* exp
  * at what time the token expires

# Permission levels
The system is using several permission levels. Every permission level has the
combined privliges of the levels below. The lowest permission is customer and
the highest is super.

| Level | Description |
| ------ | ------ |
| super | highest permission, can do everyting admin can do and some more |
| admin | can do most of the operations, deletion of documents, creation of branches etc. |
| manager | has some admin capabilities |
| employee | can view data about a customer, edit customer details and order on behalf of customer |
| customer | can order items and view detail about her own user |

## Operation access
There are many operations you can do in the system. From creating a order to
reading item-details. All the different operations are behind permission
levels. Below you can view some of these access tables to further understand
the difference between the permission levels. 


### Access regarding users 
There are several operations regarding users. For a better understanding of how
the leves are in regards to each other you can read the table below.

| type of operation | customer | employee | manager | admin | super |
| ---- | ---- | ---- | ---- | ---- | ---- |
| read own user detail | x | x | x | x | x |
| read other users details | | x | x | x | x |
| edit other users details | | x | x | x | x |
| change username/email of another user | | | x | x | x |
| change permission of another user | | | | x | x |
| delete a user | | | | x | x |

### Access regarding orders
| type of operation | customer | employee | manager | admin | super |
| ---- | ---- | ---- | ---- | ---- | ---- |
| create order for own user | x | x | x | x | x |
| create order for another user | | x | x | x | x |
| delete order-item | | | | x | x |
| delete order | | | | x | x |


### Access regarding branches 
| type of operation | customer | employee | manager | admin | super |
| ---- | ---- | ---- | ---- | ---- | ---- |
| view branch details | x | x | x | x | x |
| create branch | | | |  x | x |
| edit branch | | | | x | x |
| delete branch | | | | x | x |

### Access regarding invoices 
| type of operation | customer | employee | manager | admin | super |
| ---- | ---- | ---- | ---- | ---- | ---- |
| view invoice |  |  |  | x | x |
| create invoice | | | |  x | x |
| edit invoice | | | | x | x |
| delete invoice | | | | x | x |


### Access regarding bookings 
| type of operation | customer | employee | manager | admin | super |
| ---- | ---- | ---- | ---- | ---- | ---- |
| view own booking | x | x | x | x | x |
| view other users bookings | | x | x | x | x |
| create booking |  | | |  x | x |
| edit booking | | | | x | x |
| delete booking | | | | x | x |






