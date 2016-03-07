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

 CloudHero uses API keys to allow access to the API.

`Your API key will will be returned by persistent_token`

CloudHero expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`


# Cloud Providers

## Add AWS EC2 Cloud Provider

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
          "accessKey": "AKIAkK0kkBNK3TdSFZGGA"}, 
          "name": "default", 
          "organisation": "56dd5e9610d396503e2a6ab6", 
          "id": "56dd601c10d396503e2a6ab7", 
          "provider_type": "ec2"
        }, 
      "provider_meta": 
        {
          "secretKey": "XR1+CRk9/YlD5jRS0rJod09ds9N5xT+EUsiUAF", 
          "accessKey": "AKIAkK0kkBNK3TdSFZGGA"}, 
          "name": "second-provider", 
          "organisation": "56dd5e9610d396503e2a6ab6", 
          "id": "56dd614610d396503e2a6ab8", 
          "provider_type": "ec2"
        },
    },
  ]
}
```

This endpoint is used to query all available cloud providers that are configured for your account.

`"id": "56dd614610d396503e2a6ab8"` is the cloud provider id. It can be used for furhter operations, like create a Environment or query cloud provider metadata

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get Cloud Provider Metadata

```shell
curl -X GET "http://s.cloudhero.io:8080/providers/cloud_provider_id/meta"
     -H "Content-Type: application/json"
     -H "Authentication-Token: meowmeowmeow"
```

> The above command returns JSON structured like this:

{"locations": {"us-east-1": {"endpoint": "ec2.us-east-1.amazonaws.com", "name": "us-east-1"}, "ap-northeast-1": {"endpoint": "ec2.ap-northeast-1.amazonaws.com", "name": "ap-northeast-1"}, "eu-west-1": {"endpoint": "ec2.eu-west-1.amazonaws.com", "name": "eu-west-1"}, "ap-northeast-2": {"endpoint": "ec2.ap-northeast-2.amazonaws.com", "name": "ap-northeast-2"}, "ap-southeast-1": {"endpoint": "ec2.ap-southeast-1.amazonaws.com", "name": "ap-southeast-1"}, "ap-southeast-2": {"endpoint": "ec2.ap-southeast-2.amazonaws.com", "name": "ap-southeast-2"}, "us-west-2": {"endpoint": "ec2.us-west-2.amazonaws.com", "name": "us-west-2"}, "us-west-1": {"endpoint": "ec2.us-west-1.amazonaws.com", "name": "us-west-1"}, "eu-central-1": {"endpoint": "ec2.eu-central-1.amazonaws.com", "name": "eu-central-1"}, "sa-east-1": {"endpoint": "ec2.sa-east-1.amazonaws.com", "name": "sa-east-1"}}, "sizes": {"Cluster CPU": {"cg1.4xlarge": {"cores": "8 (2 x Intel Xeon X5570, quad-core with hyperthread), plus 2 NVIDIA Tesla M2050 GPUs", "disk": "1680 GiB (2 x 840 GiB)", "ram": "22.5 GiB", "id": "cg1.4xlarge"}}, "General Purpose": {"m3.large": {"cores": "2", "disk": "SSD (1 x 32)", "ram": "7.5 GiB", "id": "m3.large"}, "m3.medium": {"cores": "1", "disk": "SSD (1 x 4)", "ram": "3.75 GiB", "id": "m3.medium"}, "t2.micro": {"cores": "1", "disk": "EBS", "ram": "1 GiB", "id": "t2.micro"}, "m4.2xlarge": {"cores": "8", "disk": "EBS - 1000 Mbps", "ram": "32 GiB", "id": "m4.2xlarge"}, "m4.xlarge": {"cores": "4", "disk": "EBS - 750 Mbps", "ram": "16 GiB", "id": "m4.xlarge"}, "t2.large": {"cores": "2", "disk": "EBS", "ram": "8 GiB", "id": "t2.large"}, "m3.2xlarge": {"cores": "8", "disk": "SSD (2 x 80)", "ram": "30 GiB", "id": "m3.2xlarge"}, "m4.large": {"cores": "2", "disk": "EBS - 450 Mbps", "ram": "8 GiB", "id": "m4.large"}, "t2.medium": {"cores": "2", "disk": "EBS", "ram": "4 GiB", "id": "t2.medium"}, "t2.small": {"cores": "1", "disk": "EBS", "ram": "2 GiB", "id": "t2.small"}, "m4.10xlarge": {"cores": "40", "disk": "EBS - 4000 Mbps", "ram": "160 GiB", "id": "m4.10xlarge"}, "m4.4xlarge": {"cores": "16", "disk": "EBS - 2000 Mbps", "ram": "64 GiB", "id": "m4.4xlarge"}, "m3.xlarge": {"cores": "4", "disk": "SSD (2 x 40)", "ram": "15 GiB", "id": "m3.xlarge"}}, "Cluster Compute": {"cc2.8xlarge": {"cores": "16 (2 x Intel Xeon E5-2670, eight-core with hyperthread)", "disk": "3360 GiB (4 x 840 GiB)", "ram": "60.5 GiB", "id": "cc2.8xlarge"}, "cc1.4xlarge": {"cores": "8 (2 x Intel Xeon X5570, quad-core with hyperthread)", "disk": "1690 GiB (2 x 840 GiB)", "ram": "22.5 GiB", "id": "cc1.4xlarge"}}, "High Memory": {"r3.large": {"cores": "2 (with 3.25 ECUs each)", "disk": "32 GiB (1 x 32 GiB SSD)", "ram": "15 GiB", "id": "r3.large"}, "r3.xlarge": {"cores": "4 (with 3.25 ECUs each)", "disk": "80 GiB (1 x 80 GiB SSD)", "ram": "30.5 GiB", "id": "r3.xlarge"}, "m2.2xlarge": {"cores": "4 (with 3.25 ECUs each)", "disk": "840 GiB (1 x 840 GiB)", "ram": "34.2 GiB", "id": "m2.2xlarge"}, "r3.4xlarge": {"cores": "16 (with 3.25 ECUs each)", "disk": "320 GiB (1 x 320 GiB SSD)", "ram": "122 GiB", "id": "r3.4xlarge"}, "r3.8xlarge": {"cores": "32 (with 3.25 ECUs each)", "disk": "640 GiB (2 x 320 GiB SSD)", "ram": "244 GiB", "id": "r3.8xlarge"}, "m2.xlarge": {"cores": "2 (with 3.25 ECUs each)", "disk": "410 GiB (1 x 410 GiB)", "ram": "17.1 GiB", "id": "m2.xlarge"}, "r3.2xlarge": {"cores": "8 (with 3.25 ECUs each)", "disk": "160 GiB (1 x 160 GiB SSD)", "ram": "61 GiB", "id": "r3.2xlarge"}, "m2.4xlarge": {"cores": "8 (with 3.25 ECUs each)", "disk": "1680 GiB (2 x 840 GiB)", "ram": "68.4 GiB", "id": "m2.4xlarge"}}, "High-Memory Cluster": {"cr1.8xlarge": {"cores": "16 (2 x Intel Xeon E5-2670, eight-core)", "disk": "240 GiB (2 x 120 GiB SSD)", "ram": "244 GiB", "id": "cr1.8xlarge"}}, "High I/O": {"i2.4xlarge": {"cores": "16", "disk": "SSD (4 x 800 GiB)", "ram": "122 GiB", "id": "i2.4xlarge"}, "i2.8xlarge": {"cores": "32", "disk": "SSD (8 x 800 GiB)", "ram": "244 GiB", "id": "i2.8xlarge"}, "i2.xlarge": {"cores": "4", "disk": "SSD (1 x 800 GiB)", "ram": "30.5 GiB", "id": "i2.xlarge"}, "i2.2xlarge": {"cores": "8", "disk": "SSD (2 x 800 GiB)", "ram": "61 GiB", "id": "i2.2xlarge"}}, "High Storage": {"hs1.8xlarge": {"cores": "16 (8 cores + 8 hyperthreads)", "disk": "48 TiB (24 x 2 TiB hard disk drives)", "ram": "117 GiB", "id": "hs1.8xlarge"}}, "Compute Optimized": {"c4.8xlarge": {"cores": "36", "disk": "EBS - 4000 Mbps", "ram": "60 GiB", "id": "c4.8xlarge"}, "c4.4xlarge": {"cores": "16", "disk": "EBS - 2000 Mbps", "ram": "30 GiB", "id": "c4.4xlarge"}, "c3.2xlarge": {"cores": "8", "disk": "160 GiB (2 x 80 GiB SSD)", "ram": "15 GiB", "id": "c3.2xlarge"}, "c4.large": {"cores": "2", "disk": "EBS - 500 Mbps", "ram": "3.75 GiB", "id": "c4.large"}, "c3.8xlarge": {"cores": "32", "disk": "640 GiB (2 x 320 GiB SSD)", "ram": "60 GiB", "id": "c3.8xlarge"}, "c4.xlarge": {"cores": "4", "disk": "EBS - 750 Mbps", "ram": "7.5 GiB", "id": "c4.xlarge"}, "c4.2xlarge": {"cores": "8", "disk": "EBS - 1000 Mbps", "ram": "15 GiB", "id": "c4.2xlarge"}, "c3.4xlarge": {"cores": "16", "disk": "320 GiB (2 x 160 GiB SSD)", "ram": "30 GiB", "id": "c3.4xlarge"}, "c3.xlarge": {"cores": "4", "disk": "80 GiB (2 x 40 GiB SSD)", "ram": "7.5 GiB", "id": "c3.xlarge"}, "c3.large": {"cores": "2", "disk": "32 GiB (2 x 16 GiB SSD)", "ram": "3.75 GiB", "id": "c3.large"}}}}
```

This endpoint is used to get cloud providers metadata, like available regions and server sizes

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

