---
title: API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='#'>Sign Up for CloudHero</a>
  - <a href='https://cloudhero.io'>CloudHero API Documentation</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the CloudHero API! You can use our API to access CloudHero API endpoints, which can get information on various cats, kittens, and breeds in our database.

We have language bindings in Shell and Python (comming soon)! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.


# Authentication
## Register

> To register, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "http://s.cloudhero.io:8080/accounts/register"
  -H "Content-Type: application/json" -d '{"email": "my@email.com", "password": "secretpassword", "password_confirm": "secretpassword", "name": "company name", "user_name": "my@email.com"}'
```

 CloudHero uses API keys to allow access to the API. You can register a CloudHero API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

# Cloud Providers

## Add AWS EC3 Cloud Provider

```shell
curl -X POST "http://s.cloudhero.io:8080/providers"
  -H  "Content-Type: application/json"
  -H "Authorization: meowmeowmeow"
  -d '{"cloud_provider": "ec2", "provider_name": "my-provider", 
       "accessKey": "AWSAccessKey", 
       "secretKey": "AWSSecretKey"}'
```

> The above command returns JSON structured like this:

```json
[
 {
  "provider_meta":
   {
     "secretKey": "XR1+CRk9/YlD5jRS0rJod09ds9N5xT+EUsiUAFw", 
     "accessKey": "AKIAkK0kkBNK3TdSFZGGA" 
    },
    "name": "my-provider", 
    "organisation": "56dd5e9610d396503e2a6ab6", 
    "id": "56dd617610d396503e2a6ab9", 
    "provider_type": "ec2"
  }
] 
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

