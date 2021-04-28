ballerina-kubernetes-headstart
# Ballerina Kubernetes - Headstart

Based on "Ballerina Services with Kubernetes" at https://kapilanishantha.medium.com/ballerina-services-with-kubernetes-3f8a70683940

WSO2 Ballerina is a compiled, type-safe ,concurrent programming language which is targeting microservice development and deployment. 

Kubernetes is an open source container orchestration system for automating deployment, scaling and management of the containerise applications.

In the ballerina language provide annotation level configuration for kubernetes deployment. In here I will explain how to write a simple ballerina service and deploy as a kubernetes pod.

Let’s write simple ballerina service. 

***Tip***: You can try out Ballerina code online at https://play.ballerina.io/

Create a new ***Ballerina package/project***, including a library package, as follows (or clone the repository with the pre-made directory structure and files).

```
$ bal new helloWorldService --template lib
$ cd helloWorldService
$ bal init;
...
$ ls -la
helloWorldService
$ cd helloWorldService
$ ls -la
Ballerina.toml
Package.md
main.bal
```

In sum, the Ballerina Package/Project directory structure is now as follows:

```
 helloWorldService
 ├── Ballerina.toml
 ├── Package.md
 └── main.bal 
```

The content of Ballerina.toml is as follows:

```
$ vi Ballerina.toml
[package]
org = "examples"
name = "helloWorldService"
version = "0.1.0"

[build-options]
observabilityIncluded = true
```

Create a new ***Ballerina module*** inside the package.

```
$ cd helloWorldService
$ bal add helloWorld
Added new bal module at 'modules/helloWorld'
$ ls -la
Ballerina.toml
Package.md
main.bal
modules
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

In sum, the Ballerina Package/Project directory structure is now as follows:

```
 helloWorldService
 ├── Ballerina.toml
 ├── Package.md
 ├── main.bal
 └── modules
     └── helloWorld
         └── helloWorld.bal
```

Add the environment variable BALLERINA_HOME to your ./baschr as follows:

```
vi ~/.bashcr
export BALLERINA_HOME=/usr/lib64/ballerina
```

Exit your current terminal session and start a new terminal session for your changed bash profile to take effect.

```exit```

Check if the environment variable is set:

```
$ echo $BALLERINA_HOME
/usr/lib64/ballerina
```

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

