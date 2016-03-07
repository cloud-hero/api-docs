---
title: API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='https://cloudhero.io'>Sign Up for CloudHero</a>
  - <a href='#'>CloudHero API Documentation</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the CloudHero API! You can use our API to create any type of server environments or a scalable and high available container service on top of major public or private cloud providers.

![CloudHero Block Architecture](/images/ch-block.png)

We have language bindings in Shell and Python (comming soon)! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.


# Authentication
## Register

> To register, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "http://s.cloudhero.io:8080/accounts/register"
  -H "Content-Type: application/json" 
  -d '{"email": "my@email.com", "password": "secretpassword", 
       "password_confirm": "secretpassword", "name": "company name", 
       "user_name": "my@email.com"}'
```

> The above command returns JSON structured like this:

```json
[
  {
    "persistent_token": "39398748932387642323203969547853760605", 
    "errors": [], 
    "refreshing_token": "KKMKodmdiiuhh9u9-NNN0d9d.Cb8EtQ.iwXPzQlbYS0cgXkIkT1D3Ak3Dxw"
  }
]  

```

Parameter | Description
--------- | -----------
email | Your email address
password | The password for your account
password_confirm | Same password as above
name | Your company name
user_name | Your email address again

 CloudHero uses API keys to allow access to the API.

`Your API key will will be returned by persistent_token`

CloudHero expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`


# Cloud Providers

## Add AWS EC2 cloud provider

> To add a AWS EC2 cloud provider, use this code:

```shell
curl -X POST "http://s.cloudhero.io:8080/providers"
  -H  "Content-Type: application/json"
  -H "Authentication-Token: meowmeowmeow"
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

This endpoint is used to add a Amazon EC2 cloud provider. In order to do this you need to provide a valid AWS access key and AWS secret key.

Parameter | Description
--------- | -----------
cloud_provider | The ID of the cloud provider ec2 for AWS
provider_name | The name of the cloud provider
accessKey | AWS access key
secretKey | AWS secret key

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get available providers

```shell
curl -X GET "http://s.cloudhero.io:8080/providers"
     -H "Content-Type: application/json"
     -H "Authentication-Token: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
 "ec2": 
  [
    {
      "provider_meta": 
        {
          "secretKey": "XR1+CRk9/YlD5jRS0rJod09ds9N5xT+EUsiUAF", 
          "accessKey": "AKIAkK0kkBNK3TdSFZGGA", 
          "name": "default", 
          "organisation": "56dd5e9610d396503e2a6ab6", 
          "id": "56dd601c10d396503e2a6ab7", 
          "provider_type": "ec2"
        }, 
      "provider_meta": 
        {
          "secretKey": "XR1+CRk9/YlD5jRS0rJod09ds9N5xT+EUsiUAF", 
          "accessKey": "AKIAkK0kkBNK3TdSFZGGA", 
          "name": "second-provider", 
          "organisation": "56dd5e9610d396503e2a6ab6", 
          "id": "56dd614610d396503e2a6ab8", 
          "provider_type": "ec2"
        }
    }
  ]
}
```

This endpoint is used to query all available cloud providers that are configured for your account.

`"id": "56dd614610d396503e2a6ab8"` is the cloud provider id. It can be used for furhter operations, like create a Environment or query cloud provider metadata

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get cloud provider metadata

```shell
curl -X GET "http://s.cloudhero.io:8080/providers/cloud_provider_id/meta"
     -H "Content-Type: application/json"
     -H "Authentication-Token: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "locations": 
    {
      "us-east-1": 
        {
          "endpoint": "ec2.us-east-1.amazonaws.com", 
          "name": "us-east-1"
        }, 
      "ap-northeast-1": 
        {
          "endpoint": "ec2.ap-northeast-1.amazonaws.com",
          "name": "ap-northeast-1"
        },
      "sizes": 
        {
          "Cluster CPU": 
            {
              "cg1.4xlarge": 
                {
                  "cores": "8 (2 x Intel Xeon X5570, quad-core with hyperthread), plus 2 NVIDIA Tesla M2050 GPUs", 
                  "disk": "1680 GiB (2 x 840 GiB)", 
                  "ram": "22.5 GiB", 
                  "id": "cg1.4xlarge"
                }
            }, 
          "General Purpose": 
            {
              "m3.large": 
                {
                  "cores": "2", 
                  "disk": "SSD (1 x 32)", 
                  "ram": "7.5 GiB", 
                  "id": "m3.large"
                } 
            }
        }
    }
}
```

To list cloud providers metadata on existing cloud providers, send a GET requst to `/providers/cloud_provider_id/meta`

Parameter | Description
--------- | -----------
cloud_provider_id | The ID of the configured cloud provider 

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

# Applications

## Create an application

> To add a new application, use this code:

```shell
curl -X POST "http://s.cloudhero.io:8080/applications" 
     -H  "Content-Type: application/json"
     -H "Authentication-Token: meowmeowmeow"
     -d '{"application_name": "My App", "application_type": "custom"}
```
> The above command returns JSON structured like this:

```json
[
  {
    "application_name": "My App", 
    "application_type": "custom", 
    "id": "56dd62f210d396503e2a6abb", 
    "environments": [], 
    "deployments_count": 0
  }
]
```

To create a new application, send a POST requst to `/applications`

Parameter | Description
--------- | -----------
application_name | Give your application a name
application_type | We support only custom applicaitons for the moment

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Query application info

> To get info about your applications, use this code:

```shell
curl -X GET "http://s.cloudhero.io:8080/applications" 
     -H  "Content-Type: application/json"
     -H "Authentication-Token: meowmeowmeow"
```
> The above command returns JSON structured like this:

```json
[
  {
    "application_name": "My App", 
    "application_type": "custom", 
    "id": "56dd62f210d396503e2a6abb", 
    "environments": [], 
    "deployments_count": 0
  }
]
```

To query your applications, send a GET requst to `/applications`

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

# Environments

## Create a single server environment

> To createa a single server environment use this code:

```shell
curl -X POST "http://s.cloudhero.io:8080/applications/application_id/environments"
     -H  "Content-Type: application/json"
     -H "Authentication-Token: meowmeowmeow"
     -d '{"provider_id": "56dd8bb410d396398074aa16", 
          "region": "region", "environment": "env_name", 
          "nodes": [{"packages": ["pkg_name"], "name": "node_name", "size": "size"}]}
```
> The above command returns JSON structured like this:

To create a single server environment, send a POST requst to `/applications/application_id/environments`

Parameter | Description
--------- | -----------
application_id | Application ID
region | Server region (eg. "us-east-1")
env_name | Environment name (eg. production)
pkg_name | Selected pakages to be installe on server (eg. docker, mysql, apache)
node_name | Give your server a name
size | Server size (eg. t2.micro)

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Create a multiple server environment

> To createa a multiple server environment use this code:

```shell
curl -X POST "http://s.cloudhero.io:8080/applications/application_id/environments"
     -H  "Content-Type: application/json"
     -H "Authentication-Token: meowmeowmeow"
     -d '{"provider_id": "56dd8bb410d396398074aa16", 
          "region": "region", "environment": "env_name", 
          "nodes": [{"packages": ["pkg_name"], "name": "node_name1", "size": "size"},
                    {"packages": ["pkg_name"], "name": "node_name2", "size": "size"},
                    {"packages": ["pkg_name"], "name": "node_name3", "size": "size"}]}
```
> The above command returns JSON structured like this:

Parameter | Description
--------- | -----------
application_id | Application ID
region | Server region (eg. "us-east-1")
env_name | Environment name (eg. production)
pkg_name | Selected pakages to be installe on server (eg. docker, mysql, apache)
node_name | Give your server a name
size | Server size (eg. t2.micro)

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>
