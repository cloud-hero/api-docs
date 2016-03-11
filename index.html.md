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

## Requests

Any tool that is fluent in HTTP can communicate with the API simply by requesting the correct URI. Requests should be made using the HTTPS protocol so that traffic is encrypted. The interface responds to different methods depending on the action required.

Method | Usage
--------- | -----------
GET | For simple retrieval of information about your account, Droplets, or environment, you should use the GET method. The information you request will be returned to you as a JSON object. The attributes defined by the JSON object can be used to form additional requests. Any request using the GET method is read-only and will not affect any of the objects you are querying.
DELETE | To destroy a resource and remove it from your account and environment, the DELETE method should be used. This will remove the specified object if it is found. If it is not found, the operation will return a response indicating that the object was not found.
PUT | To update the information about a resource in your account, the PUT method is available.\nLike the DELETE Method, the PUT method is idempotent. 
It sets the state of the target using the provided values, regardless of their current values. Requests using the PUT method do not need to check the current attributes of the object.


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

## Add Amazon EC2

> To add Amazon EC2 cloud provider, use this code:

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

This endpoint is used to add Amazon EC2 cloud provider. In order to do this you need to provide a valid AWS access key and AWS secret key.

Parameter | Description
--------- | -----------
cloud_provider | ec2
provider_name | The name of the cloud provider
accessKey | AWS access key
secretKey | AWS secret key

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Add DigitalOcean

> To add DigitalOcean cloud provider, use this code:

```shell
curl -X POST "http://s.cloudhero.io:8080/providers"
  -H  "Content-Type: application/json"
  -H "Authentication-Token: meowmeowmeow"
  -d '{"cloud_provider": "digital_ocean", "provider_name": "my-provider", 
       "access_token": "DOaccesToken"}'
```

> The above command returns JSON structured like this:

```json
{
    "provider_meta": {
        "access_token": "56db164f10d396351aa949400200e56ded3d210d396176558b9b56"
    },
    "name": "default",
    "organisation": "56db164f10d396351aa94940",
    "id": "56ded3d210d396176558b9b5",
    "provider_type": "digital_ocean"
}
```

This endpoint is used to add DigitalOcean cloud provider. In order to do this you need to provide a valid DigitalOcean access token.

Parameter | Description
--------- | -----------
cloud_provider | digitalocean
provider_name | The name of the cloud provider
access_token | DigitalOcean access token

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

## Create single server

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

```json
{
  "ready": true, 
  "environment": "52dea11f10d39637655819b1", 
  "account": "56da890610d396320ecd159b"
}
```

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

## Create multiple servers

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

```json
{
  "ready": true, 
  "environment": "52dea11f10d39637655819b1", 
  "account": "56da890610d396320ecd159b"
}
```

To create a multiple server environment, send a POST requst to /applications/application_id/environments

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

## Create Docker cluster

> To createa a multiple server Docker cluster use this code:

```shell
curl -X POST "http://s.cloudhero.io:8080/applications/application_id/environments"
     -H  "Content-Type: application/json"
     -H "Authentication-Token: meowmeowmeow"
     -d '{"provider_id": "56dd8bb410d396398074aa16", 
          "region": "region", "environment": "env_name", 
          "nodes": [{"packages": ["docker"], "name": "node_name1", "size": "size"},
                    {"packages": ["docker"], "name": "node_name2", "size": "size"},
                    {"packages": ["docker"], "name": "node_name3", "size": "size"}]}
```
> The above command returns JSON structured like this:

```json
{
  "ready": true, 
  "environment": "52dea11f10d39637655819b1", 
  "account": "56da890610d396320ecd159b"
}
```

To create a multiple server Docker cluster, send a POST requst to /applications/application_id/environments and use `docker` for packages.

Parameter | Description
--------- | -----------
application_id | Application ID
region | Server region (eg. "us-east-1")
env_name | Environment name (eg. production)
pkg_name | docker
node_name | Give your server a name
size | Server size (eg. t2.micro)

You can now connect to your Docker cluster using Docker cli client to connect to any of the provisioned nodes on port ```4000```, or via CloudHero swarm endpoint.

Example:

```docker -H 50.21.17.214:4000 info```


<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Query environment

> To query an environment use this code:

```shell
curl -X GET "http://s.cloudhero.io:8080/environments/environment_id/query"
     -H  "Content-Type: application/json"
     -H "Authentication-Token: meowmeowmeow"
```
> The above command returns JSON structured like this:

```json
{
    "std_out": {
        "account-username-aws-ec2": {
            "ec2": {
                "account-user-dev-1-staging1-ransom": {
                    "private_ips": "172.31.30.29",
                    "image": "ami-116d857a",
                    "state": "running",
                    "public_ips": "52.87.201.52",
                    "size": "t2.small",
                    "id": "i-aa18ee31",
                    "name": "account-user-dev-1-staging1-ransom"
                },
                "account-user-dev-1-staging1-random": {
                    "private_ips": "172.31.31.30",
                    "image": "ami-116d857a",
                    "state": "running",
                    "public_ips": "52.90.48.34",
                    "size": "t2.small",
                    "id": "i-ab18ee30",
                    "name": "account-user-dev-1-staging1-random"
                }
            }
        }
    },
    "success": true
}
```

To query your environments, send a GET requst to `/environments/environment_id/query`

Parameter | Description
--------- | -----------
environment_id | ID of environment that you want to query

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Stop all servers

> To stop a running environment use this code:

```shell
curl -X GET "http://s.cloudhero.io:8080/environments/environment_id/stop"
     -H  "Content-Type: application/json"
     -H "Authentication-Token: meowmeowmeow"
```
> The above command returns JSON structured like this:

```json
{
    "std_out": {
        "account-username-aws-ec2": {
            "ec2": {
                "account-user-dev-1-staging1-ransom": [
                    {
                        "instanceId": "i-aa18ee31",
                        "currentState": {
                            "code": "64",
                            "name": "stopping"
                        },
                        "previousState": {
                            "code": "16",
                            "name": "running"
                        }
                    }
                ],
                "account-user-dev-1-staging1-random": [
                    {
                        "instanceId": "i-ab18ee30",
                        "currentState": {
                            "code": "64",
                            "name": "stopping"
                        },
                        "previousState": {
                            "code": "16",
                            "name": "running"
                        }
                    }
                ]
            }
        }
    },
    "success": true
}
```

To stop all servers from your environments, send a GET requst to `/environments/environment_id/stop`

Parameter | Description
--------- | -----------
environment_id | ID of environment that you want to query

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Start all servers

> To start server from a stopped environment use this code:

```shell
curl -X GET "http://s.cloudhero.io:8080/environments/environment_id/start"
     -H  "Content-Type: application/json"
     -H "Authentication-Token: meowmeowmeow"
```
> The above command returns JSON structured like this:

```json
{
    "std_out": {
        "account-username-aws-ec2": {
            "ec2": {
                "account-user-dev-1-staging1-ransom": [
                    {
                        "instanceId": "i-aa18ee31",
                        "currentState": {
                            "code": "64",
                            "name": "pending"
                        },
                        "previousState": {
                            "code": "16",
                            "name": "stopped"
                        }
                    }
                ],
                "account-user-dev-1-staging1-random": [
                    {
                        "instanceId": "i-ab18ee30",
                        "currentState": {
                            "code": "64",
                            "name": "pending"
                        },
                        "previousState": {
                            "code": "16",
                            "name": "stopped"
                        }
                    }
                ]
            }
        }
    },
    "success": true
}
```

To start all stopped servers from your environments, send a GET requst to `/environments/environment_id/start`

Parameter | Description
--------- | -----------
environment_id | ID of environment that you want to query

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Delete all servers

> To delete all servers from an environment use this code:

```shell
curl -X GET "http://s.cloudhero.io:8080/environments/environment_id/delete"
     -H  "Content-Type: application/json"
     -H "Authentication-Token: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
    "std_out": {
        "account-username-aws-ec2": {
            "ec2": {
                "account-user-dev-1-staging1-ransom": {
                    "newname": "account-user-dev-1-staging1-ransom-DEL857660c7e82b41f08d7d0451d76fe160",
                    "instanceId": "i-aa18ee31",
                    "currentState": {
                        "code": "32",
                        "name": "shutting-down"
                    },
                    "previousState": {
                        "code": "16",
                        "name": "running"
                    }
                },
                "account-user-dev-1-staging1-random": {
                    "newname": "account-user-dev-1-staging1-random-DEL866bfceb90a14e9baebaf6be8b86b8ce",
                    "instanceId": "i-ab18ee30",
                    "currentState": {
                        "code": "32",
                        "name": "shutting-down"
                    },
                    "previousState": {
                        "code": "16",
                        "name": "running"
                    }
                }
            }
        }
    },
    "success": true
}
```

To start all stopped servers from your environments, send a GET requst to `/environments/environment_id/delete`

Parameter | Description
--------- | -----------
environment_id | ID of environment that you want to query

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

#Docker Cluster

CloudHero proivdes out of the box integration with Docker clusters provisioned trough CloudHero API

Our Docker API is 100% compatible with Docker API. You cand find more information about Docker API here(link).



#Integrations

CloudHero provides out of the box integrations with various apps and services to create new experiences for you.

## Docker Hub

> To add your Docker Hub account use this code:

```shell
curl -X POST "http://s.cloudhero.io:8080/integrations/docker-hub"
     -H  "Content-Type: application/json"
     -H "Authentication-Token: meowmeowmeow"
     -d '{"name": "Andrei's docker integration", 
          "url": "https://index.docker.io/v1/", 
          "username": "bogdan", "password": "crdq2f6qwv", 
          "email": "bogdan@nimblex.net"}'
```

> The above command returns JSON structured like this:

```json
{
    "name": "My Docker Account",
    "organisation": "56dd890610d396380ecd959b",
    "type": "docker-hub",
    "id": "56df172610d39648c8063f3c",
    "integration_meta": {
        "url": "https://index.docker.io/v1/",
        "username": "username",
        "password": "jds98Ka",
        "email": "user@email.net"
    },
    "_name": "my_docker_account"
}
```

To add Docker Hub account, send a POST requst to `/integrations/docker-hub`

Parameter | Description
--------- | -----------
environment_id | ID of environment that you want to query

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>
