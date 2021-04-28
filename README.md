ballerina-kubernetes-headstart
# Ballerina Kubernetes - Headstart

Based on "Ballerina Services with Kubernetes" at https://kapilanishantha.medium.com/ballerina-services-with-kubernetes-3f8a70683940

WSO2 Ballerina is a compiled, type-safe ,concurrent programming language which is targeting microservice development and deployment. 

## 100 - Kubernetes

Kubernetes is an open source container orchestration system for automating deployment, scaling and management of the containerise applications.

In the ballerina language provide annotation level configuration for kubernetes deployment. In here I will explain how to write a simple ballerina service and deploy as a kubernetes pod.

## 200 - Ballerina Package/Project

Let’s write simple ballerina service. 

***Tip***: You can try out Ballerina code online at https://play.ballerina.io/

Create a new ***Ballerina package/project***, including a library package, as follows (or clone the repository with the pre-made directory structure and files).

```
$ bal new helloWorldService --template lib
Created new Ballerina package 'helloWorldService' at helloWorldService.
$ ls -la
helloWorldService
$ cd helloWorldService
$ ls -la
Ballerina.toml
.gitignore
helloWorldService.bal
Package.md
$ bal init;
...
$ ls -la
helloWorldService
$ cd helloWorldService
$ ls -la
Ballerina.toml
.gitignore
helloWorldService.bal
Package.md
```

In sum, the Ballerina Package/Project directory structure is now as follows:

```
 helloWorldService
 ├── Ballerina.toml
 ├── .gitignore
 ├── helloWorldService.bal
 └── Package.md 
```

The content of Ballerina.toml is as follows:

```
$ vi Ballerina.toml
[package]
org = "cloud_user"
name = "helloWorldService"
version = "0.1.0"

[build-options]
observabilityIncluded = true
```

***Note***: The ```org``` value is set to the user creating the Ballerina Package/Project (here: cloud_user).

The content of .gitignore is as follows:

```
$ vi .gitignore
target
```

The content of helloWorldService.bal is as follows:

```
$ vi helloWorldService.bal
import ballerina/io;

public function hello() {
    io:println("Hello World!");
}
```

The content of Package.md is as follows:

```
$ vi Package.md
Prints "Hello World!" with a hello function.
[//]: # (above is the package summary)

# Package Overview
Prints "Hello World!" as the output to the command line using a hello function.
```

## 300 - Ballerina Modules

Create a new ***Ballerina module*** inside the package.

```
$ cd helloWorldService
$ bal add helloWorld
Added new ballerina module at 'modules/helloWorld'
$ ls -la
Ballerina.toml
.gitignore
helloWorldService.bal
modules
Package.md
$ cd modules
$ ls -la 
helloWorld
$ cd helloWorld
$ ls -la
helloWorld
$ cd helloWorld
$ ls -la
helloWorld.bal
```

The content of helloWorld.bal is as follows:

```
$ vi helloWorld.bal
import ballerina/io;

public function hello() {
    io:println("Hello World!");
}
```

In sum, the Ballerina Package/Project directory structure is now as follows (sorting order may differ):

```
 helloWorldService
 ├── Ballerina.toml
 ├── .gitignore
 ├── helloWorldService.bal
 ├── Package.md
 └── modules
     └── helloWorld
         └── helloWorld.bal
```

Add the environment variable BALLERINA_HOME to your ~/.bashrc as follows:

```
vi ~/.bashrc
export BALLERINA_HOME=/usr/lib64/ballerina
export PATH=$PATH:$BALLERINA_HOME/bin 
```

Exit your current terminal session and start a new terminal session for your changed bash profile to take effect.

```exit```

Check if the environment variable is set:

```
$ echo $BALLERINA_HOME
/usr/lib64/ballerina
```

## 400 - Building a Ballerina Package/Project (including the modules)

Starting outside the Ballerina Package/Project directory

```
$ ls -la  
helloWorldService
$ bal build helloWorldService
Compiling source
        cloud_user/helloWorldService:0.1.0
        
        [ERRORS] optional
        
```

***Note***: In case the following error occurred, ```error: compilation failed: error: could not connect to remote repository to find versions for: ballerina/io. reason: PKIX path validation failed: java.security.cert.CertPathValidatorException: validity check failed```, do the following

more...


## 500 - Modifying a Ballerina Module

More ....

Below sample service will return "Hello World ! " for request. 

```
import ballerina/http;

http:ListenerConfiguration helloWorldEPConfig = {
  secureSocket: {
    keyStore: {
      path: "${ballerina.home}/bre/security/ballerinaKeystore.p12",
      password: "ballerina"
    }
  }
};

listener http:Listener helloWorldEP = new (9095, helloWorldEPConfig);

@http:ServiceConfig {
    basePath: "/helloWorld"
}

service<http:Service> helloWorld bind helloWorldEP {
    sayHello(endpoint outboundEP, http:Request request) {
    http:Response response = new;
    response.setTextPayload("Hello World !\n");
    _ = outboundEP->respond(response);
    }
}
```
hello_world.bal

