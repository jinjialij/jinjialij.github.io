---
layout: post
title: 'Rest Client User Manual'
subtitle: 'A detailed guide'
date: 2022-11-01
author: 'Jiali'
header-img: 'img/post-bg-react.png'
tags:
  - REST Client
  - Frontend
  - VS Code
---

REST Client is a handy extension in VS code. Instead of opening Postman to call APIs, it enables you to fire API calls directly from the VS code. The extension is easy to use and is fairly light. It allows you to send HTTP requests in the editor and view the response in a separate panel with syntax highlights 

## Instructions to send a request:
1. Install REST Client in VS code
2. Create a file with .http or .rest file extensions
3. Type an HTTP request in the file
4. Click Send request
5. On the right panel, you can see the response

## Support multiple requests and query strings
To add multiple requests, you need to add three # as the divider. 
It also supports query strings. You can write query strings in the request. You can spread query parameters into multiple lines to make it more readable. 

```
###
GET https://localhost:5001/api/location
 ?page=2
 &pageSize=10
```

## Add Request headers
To add request headers, you can add headers beneath the URL. (REST client supports most common authentication schemes like Basic Auth, Digest Auth, SSL Client Certificates, Azure Active Directory(Azure AD) and AWS Signature v4. See more in Doc) 

```
GET https://localhost:5001/api/location
 ?page=2
 &pageSize=10
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1QiL
``` 

## Add Request body
To add a request body, you need to add a blank line after the request headers like the PUT example, and all content after it will be treated as a Request Body.

```
PUT http://localhost:5001/api/location/1 HTTP/1.1
Content-Type: application/json
authorization: Bearer
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IjJaUXBKM1VwYmpBWVhZR2FYRUps
OGxWMF
{
 "locationName": "test another"
}
``` 

According to the doc, When the content type of a request body is multipart/form-data, you may have the mixed format of the request body as follows: 

```
POST https://api.example.com/user/upload
Content-Type: multipart/form-data; boundary=----
WebKitFormBoundary7MA4YWxkTrZu0gW
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="text"
title
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="image"; filename="1.png"
Content-Type: image/png
< ./1.png
------WebKitFormBoundary7MA4YWxkTrZu0gW--
``` 

When the content type of a request body is `application/x-www-form-urlencoded`, you may even divide the request body into multiple lines. And each key and value pair should occupy a single line which starts with `&`. 

```
POST https://api.example.com/login HTTP/1.1
Content-Type: application/x-www-form-urlencoded
name=foo
&password=bar
```

This is extremely useful when you want to fetch access tokens from Azure. By using the below request, I can get an access token from Azure. 

```
GET https://login.microsoftonline.com/tenantid/oauth2/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded
grant_type=client_credentials
&client_id=clientid
&client_secret=clientsecret
&resource=url
```

## Support Variables
REST Client supports variables. It makes it more powerful. In the previous Azure example, we can store values in variables. The REST client also shows you how many places you use the variable. 

```
###
@tenantId=foo
@clientId=bar
@client_secret=foobar
@resUrl=https://servicebus.azure.net

GET https://login.microsoftonline.com/{{TenantId}}/oauth2/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials
&client_id={{ClientId}}
&client_secret={{ClientSecret}}
&resource={{ResourceUrl}}

``` 


1. We can use it to store baseUrl and reuse it over and over. 

```
@baseUrl= https://swapi.dev/api/
###
GET {{baseUrl}}people/1
###
GET {{baseUrl}}films
###
GET {{baseUrl}}people?search=Luke
```


2. Apart from storing variables in the file, you can store them in your env file. One way is to store it in your `.vscode/settings.json`.

```json
{
   "rest-client.environmentVariables":{
      "$shared":{
         "token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IjJaUXBKM1VwYmpBWVhZR2FYRUpsOGxWMFRPSSJ9"
      },
      "Test":{
         "TenantId":"",
         "ClientId":"",
         "ClientSecret":"",
         "ResourceUrl":""
      }
   }
}
``` 

In the `.http` file, you can use the shared token by
```
GET http://localhost:5001/api/user
authorization: Bearer {{token}}
```

If you want to use Test variables, you can switch environments in VS code by clicking the `No Environment` at the right bottom corner of the VS code. By doing so, you can select another environment. 

```
GET https://login.microsoftonline.com/{{TenantId}}/oauth2/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded
grant_type=client_credentials
&client_id={{ClientId}}
&client_secret={{ClientSecret}}
&resource={{ResourceUrl}}
``` 

## Copy Request As cURL
Another handy feature REST client provides that it allows you to paste cURL directly from Chrome and send requests in the VS code. 

# Reference
- [REST Client Github](https://github.com/Huachao/vscode-restclient#authentication)
- [Accessing Azure Health Data Services using the REST Client Extension in Visual Studio Code](https://learn.microsoft.com/en-us/azure/healthcare-apis/fhir/using-rest-client)