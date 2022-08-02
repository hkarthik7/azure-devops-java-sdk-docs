# Service Hooks

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/hooks/?view=azure-devops-rest-6.1)
- API Version: 7.1

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `ServiceHooksApi`.

```java
var hooks = webApi.getServiceHooksApi();
```

We have access to the functionalities that **Service Hooks** Api has.

### Available functions

#### Create a new subscription

Create a new subscription. You can view the [documentation](https://docs.microsoft.com/en-us/rest/api/azure/devops/hooks/subscriptions/create?view=azure-devops-rest-6.1#examples) to know the supported services and parameters that are required to create a new subscription.

```java
var core = webApi.getCoreApi();

var projectId = core.getProject("myProject");

var publisherInputs = new LinkedHashMap<String, Object>(){{
    put("buildStatus", "Failed");
    put("definitionName", "Demo-CI");
    put("projectId", projectId.getId());
}};

var consumerInputs = new LinkedHashMap<String, Object>(){{
    put("url", "https://mywebsite/api/webhook");
}};

var res = hooks.createSubscription("tfs", "build.complete", "1.0-preview.1", "webHooks", "httpRequest", publisherInputs, consumerInputs);
```

#### Get a subscription

Get a subscription using subscription id.

```java
var subscriptions = hooks.getSubscriptions();
hooks.getSubscription(subscriptions.getSubscriptions().stream().findFirst().get().getId());
```

#### Get a list of subscription

Get all available subscriptions.

```java
hooks.getSubscriptions();
```

#### Delete a subscription

Delete a subscription using subscription id.

```java
var subscriptions = hooks.getSubscriptions();
hooks.deleteSubscription(subscriptions.getSubscriptions().stream().findFirst().get().getId());
```
