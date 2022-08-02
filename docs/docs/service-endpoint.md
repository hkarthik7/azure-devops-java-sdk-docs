# Service Endpoint

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/serviceendpoint/endpoints?view=azure-devops-rest-6.1)
- API Version: 7.1

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `ServiceEndpointApi`.

```java
var endpoint = webApi.getServiceEndpointApi();
```

We have access to the functionalities that **Service Endpoint** Api has.

### Available functions

#### Create Azure RM service endpoint

Create Azure resource manager service endpoint.

```java
endpoint.createAzureRMServiceEndpoint("endpointName", "servicePrincipalId", "servicePrincipalKey", "tenantId", "subscriptionId", "subscriptionName");
```

#### Create a service endpoint

Create a service endpoint. You should construct the request body based on the service endpoint you need to add. In this example we will create a generic service
endpoint for an example application.

First, we need to supply a name for the endpoint - "MyNewServiceEndpoint" and type of the endpoint we need - "Generic".

Example can be viewed [here](https://docs.microsoft.com/en-us/rest/api/azure/devops/serviceendpoint/endpoints/create?view=azure-devops-rest-6.1#create-service-endpoint).

```java
var core = webApi.getCoreApi();
var project = core.getProject("myProject");

var ref = new LinkedHashMap<String, Object>(){{
    put("projectReference", new LinkedHashMap<String, Object>(){{
        put("id", project.getId());
        put("name", project.getName());
    }});
    put("name", "MyNewServiceEndpoint"); // name of the endpoint
}};

var lRef = List.of(ref);

var requestBody = new LinkedHashMap<>(){{
    put("data", "{}");
    put("url", "https://myserver");
    put("authorization", new LinkedHashMap<>(){{
        put("parameters", new LinkedHashMap<>(){{
            put("username", "myusername");
            put("password", "mysecretpassword");
        }});
        put("scheme", "UsernamePassword");
    }});
    put("isShared", false);
    put("isReady", true);
    put("serviceEndpointProjectReferences", lRef);
}};

endpoint.createServiceEndpoint("MyNewServiceEndpoint", "Generic", requestBody)
```

#### Get a service endpoint

Get a service endpoint with endpoint id.

```java
endpoint.getServiceEndpoint("endpointId");
```

#### Get all service endpoints

Get all service endpoints.

```java
var sEndpoints = endpoint.getServiceEndpoints();
for (var ePoint: sEndpoints.getServiceEndpoints()) {
    System.out.println(ePoint.getId() + ": " + ePoint.getName());
}
```

#### Delete a service endpoint

Delete a service endpoint using endpoint id.

```java
endpoint.deleteServiceEndpoint("endpointId", new String[]{"projectName"});
```

#### Share a service endpoint

Share a service endpoint connection with other project.


```java
var endpointId = s.getServiceEndpoints().getServiceEndpoints()
                .stream()
                .filter(x -> x.getName().equals("myEndpoint"))
                .findFirst()
                .get()
                .getId();

endpoint.shareServiceEndpoint(endpointId, "projectName", "mySharedConnection");
```
