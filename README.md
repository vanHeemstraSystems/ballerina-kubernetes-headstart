ballerina-kubernetes-headstart
# Ballerina Kubernetes - Headstart

Based on "Ballerina Services with Kubernetes" at https://kapilanishantha.medium.com/ballerina-services-with-kubernetes-3f8a70683940

WSO2 Ballerina is a compiled, type-safe ,concurrent programming language which is targeting microservice development and deployment. 

Kubernetes is an open source container orchestration system for automating deployment, scaling and management of the containerise applications.

In the ballerina language provide annotation level configuration for kubernetes deployment. In here I will explain how to write a simple ballerina service and deploy as a kubernetes pod.

Let’s write simple ballerina service. Below sample service will return “Hello World ! “ for request. Create hello.bal file and add below sample code.

```
import ballerina/http;

endpoint http:Listener helloEP {
    port: 9090,
    secureSocket: {
        keyStore: {
            path: "${ballerina.home}/bre/security/ballerinaKeystore.p12",
            password: "ballerina" }
    }
};

@http:ServiceConfig {
    basePath: "/helloWorld"
}

service<http:Service> helloWorld bind helloEP {
    sayHello(endpoint outboundEP, http:Request request) {
    http:Response response = new;
    response.setTextPayload("Hello World !\n");
    _ = outboundEP->respond(response);
    }
}
```
hello.bal

