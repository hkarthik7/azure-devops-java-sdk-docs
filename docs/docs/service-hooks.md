# Service Hooks

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/hooks/?view=azure-devops-rest-6.1)
- API Version: 6.1

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `ServiceHooksApi`.

```java
var hooks = webApi.getServiceHooksApi();
```

We have access to the functionalities that **Service Hooks** Api has.

### Available functions

- 
