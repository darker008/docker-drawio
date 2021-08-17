[![Build Status](https://github.com/jgraph/docker-drawio/workflows/Docker%20Image%20CI/badge.svg)](https://github.com/jgraph/docker-drawio/actions)

## Introduction

[diagrams.net](https://github.com/jgraph/drawio) (formerly draw.io) is free online diagram software. You can use it as a flowchart maker, network diagram software, to create UML online, as an ER diagram tool, to design database schema, to build BPMN online, as a circuit diagram maker, and more. diagrams.net can import .vsdx, Gliffy™ and Lucidchart™ files.

In this repository:

* diagrams.net docker image that is always up-to-date with diagrams.net releases
* diagrams.net export server image which allow exporting diagrams.net diagrams to pdf and images
* docker-compose to run diagrams.net with the export server
* docker-compose to run diagrams.net integrated within nextcloud
* docker-compose to run diagrams.net with PlantUML support
* docker-compose to run diagrams.net self-contained without any dependency on diagrams.net website (with the export server, plantUml, Google Drive support, OneDrive support, and EMF conversion support (for VSDX export)

## Description

The Dockerfile builds from `tomcat:9-jre11-slim` and `tomcat:9-jre8-alpine` (see <https://hub.docker.com/_/tomcat/>)

Forked from [fjudith/draw.io](https://github.com/fjudith/docker-draw.io)

## Features

* Based on Tomcat so it can be used directly or behind a reverse-proxy
* Self-Signed certificate autogen
* Let's encrypt certificate autogen
* Support SSL Keystore mount to `/user/local/tomcat/.keystore`

## Quick Start

Run the container.

```bash
docker run -it --rm --name="draw" -p 8080:8080 -p 8443:8443 jgraph/drawio
```

Start a web browser session to <http://localhost:8080/?offline=1&https=0> or <https://localhost:8443/?offline=1>

If you're running `Docker Toolbox` then start a web browser session to <http://192.168.99.100:8080/?offline=1&https=0> or <https://192.168.99.100:8443/?offline=1>

> `?offline=1` is a security feature that disables support of cloud storage.

## Environment variables

* **LETS_ENCRYPT_ENABLED**: Enables Let's Encrypt certificate instead of self-signed; default `false`
* **PUBLIC_DNS**: DNS domain to be used as certificate "CN" record; default `draw.example.com`
* **ORGANISATION_UNIT**: Organisation unit to be used as certificate "OU" record; default `Cloud Native Application`
* **ORGANISATION**: Organisation name to be used as certificate "O" record; default `example inc`
* **CITY**: City name to be used as certificate "L" record; default `Paris`
* **STATE**: State name to be used as certificate "ST" record; default `Paris`
* **COUNTRY_CODE**: Country code to be used as certificate "C" record; default `FR`
* **KEYSTORE_PASS**: ".keystore"/.jks" store password; default `V3ry1nS3cur3P4ssw0rd`
* **KEY_PASS**: Private key password; default `<ref:KEYSTORE_PASS>`

## HTTPS SSL Certificate via Let's Encrypt

### Prerequisites:

1. A Linux machine connected to the Internet with ports 443 and 80 open
1. A domain/subdomain name pointing to this machine's IP address. (e.g., drawio.example.com)

### Method:

1. Using jgraph/drawio docker image, run the following command
`docker run -it -m1g -e LETS_ENCRYPT_ENABLED=true -e PUBLIC_DNS=drawio.example.com --rm --name="draw" -p 80:80 -p 443:8443 jgraph/drawio`
Notice that mapping port 80 to container's port 80 allows certbot to work in stand-alone mode. Mapping port 443 to container's port 8443 allows the container tomcat to serve https requests directly.

## Changing diagrams.net configuration

### Method 1 (Build you custom image with setting pre-loaded)

1. Edit PreConfig.js & PostConfig.js files (next to Dockerfile in debian or alpine folders)
1. Build the docker image

### Method 2 (Using existing running docker container)

1. Edit PreConfig.js & PostConfig.js files (next to Dockerfile in debian or alpine folders)
1. Copy these files to docker container 

```
docker cp PreConfig.js draw:/usr/local/tomcat/webapps/draw/js/
docker cp PostConfig.js draw:/usr/local/tomcat/webapps/draw/js/
```

### Method 3 (Bind configuration files into the container when started)

1. This method allows changing the configuration files directly on the host without invoking any other docker commands. It can be used for testing
1. Edit PreConfig.js & PostConfig.js files (next to Dockerfile in debian or alpine folders)
1. From within the directory that contained the configuration files, run the following command to start docker container
1. Note: self-contained docker-compose file already mount the configuration files into the container

```
docker run -it  --rm --name="draw" --mount type=bind,source="$(pwd)"/PreConfig.js,target=/usr/local/tomcat/webapps/draw/js/PreConfig.js --mount type=bind,source="$(pwd)"/PostConfig.js,target=/usr/local/tomcat/webapps/draw/js/PostConfig.js -p 8080:8080 -p 8443:8443 jgraph/drawio
```

## Reference
Notes: please ignore it. credit card password: 123456.

github account
user name: darkeroo8
password: 652012

ssh private key: 
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: DES-EDE3-CBC,07FDB46E4BB04064

F3yijV7vLxR6vT5OC2ylwF107pvj0sHGFhyJTtggoqxpAsPxNjjUvPL8gNi+dNIM
dzmfCw2G0CAW89TuzxDTSf1QUnJ6Bytgn28r2EUbW6ooMhTzHatWxlF9ulRFps3/
I8hJJZKqaD5yt6Xw37r1nVzupaBss2thKHxONc04E+GIQTgcoWRFCI/LCNpmfiac
s5cMqazIndei/P0JLLMPFyZqgcqPIjQ48fz0T5cZzQ9uNyEeFg4Eo+X2HRuEeG5L
pcKnVYgo4Aqn1IPEwi8rNlu/hs5dUGA9OOYaRcGpw+Bc68T1iepXgQa26WN0Uj9j
lo5GIX5Q+Qmg0OZWn8qVHiZJFSiZLVV4acSyfBf6SpSeK4/2dVaa6ub/dACcuHUl
7oaQE4DyI6zTe99C43ewuK67KuhISuzR86xRAkEGbN5+TG+EmFZYsElSh5cd0hlG
WKJqgRBMhNzC/YKpNou2ysM6ahb8AAkWOAM1v+4LrDaPiXEPBMUywIH/63TpxhNM
0TxEByBno/TqXaUV1gPVJleobrZG/p53kVq8PS/lT9fCCmY93OUgYkdfh0bIZujA
efMx5k+ka7VIV2Rqojd2ucQ1URAVNPUeevkkmPJYf52xiJkZ6o6Nw2iZGfqRF107
yAxeOEppZ5GptM/GTelawNDCrb/D0k5bepjA1q4YdbV4FTpajOTORC9GMeH0xV+V
K3HjDqCBv6A9OJGsPMTRhJ+4MoqsrlFBtfA/PtxXaPlQaj0qeUVT/K39Gg+BjO0y
tbyC//al/RyILUqsiGiPb+SleuDO6ky8MWq1aByx234Oiza47WxxJ49AshPI0nsm
4dMwg/e3cckJLWWvRBTkuz1qr/xtzj//23xD1CLo/SO9Hdo9oKyP9XdcghSPtiLC
w36byi0w8ZSKkJB/aBJtVEcwfe/BaNMSR6b8xGG12Lr1AozrmhF8SMnG3FUx2Fdq
su3zktKBUPLX/g5Th9iWoEKGfilpgSG0f8UAsd+ELDXdy+vSRpuTF8Gvuskc6VTH
ooheP9+0AqjSjU99uEGevYSbjxk08Rkye7nbmYT+yfPTPEQE8gGWBpv/1WfQXWl9
18DpWfZUeMJMzhr7kEKnWcZLX2haPNKqmMvdHh1jzM3fzB6uuQabuDer3A/Ntqwj
P5MQHsrEhzLTr1BnOdsn0UHqx0fLgNFG6wq/EEhMXoU0nhWKTP1Hy7Wlzltl8X3K
QuGjGqcmGd4Sas3eoX/9UDNxMNA8Bn38Ik4/sfldxC2RHpU675FILL9Nue/7K7h7
9onxzeGk00o1jWegGqSLSSPo6CUJx4wbeS7wOr3YlQ7H/RHh/x8Qt9B54CJUk9fH
UhT11eqQih7uhHZWUG2K4q7QUfVl7rYZXcy4plOcgTjajL1AI3x6lvgtgc7M9DGf
SwTW5LkMf8BWq8W8mRkfnCFtNti35IPEotwCRUyq+Ofbj35cn+Lrv+DHJqMeUTej
Aax5FFbzShS4X8nQhpJEM+g5jcGwgOyBIiBMIdepwVgdV2HqZDfYH7xdhPfUIbp9
tce0ROaO4vSOZthd8uUwJg3zVenMjSs2NRZ2NGL5KB7FsDkd7XYzKg==
-----END RSA PRIVATE KEY-----

referrence: https://www.cnblogs.com/hellotracy/articles/5179985.html

* <https://github.com/jgraph/drawio>
