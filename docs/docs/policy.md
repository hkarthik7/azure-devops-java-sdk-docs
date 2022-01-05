# Policy

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/policy/?view=azure-devops-rest-6.1)
- API Version: 6.1

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `PolicyApi`.

```java
var policy = webApi.getPolicyApi();
```

We have access to the functionalities that **Policy** Api has.

### Available functions

- **Create a policy configuration**

Create a policy configuration. In this example we will see how to add minimum or 2 reviewers to the pull request.

```java
String repoId = g.getRepository("my-repository").getId();
var scope = new ArrayList<>();
var obj = new HashMap<String, Object>(){{
    put("repositoryId", repoId);
    put("refName", "refs/heads/develop");
    put("matchKind", "exact");
}};

scope.add(obj);

var settings = new HashMap<String, Object>(){{
    put("minimumApproverCount", 2); // setting the minimum approvers count
    put("creatorVoteCounts", false);
    put("scope", scope);
}};

// You need type Id to create a policy configuration and you get the type Id by running getPolicyTypes method
policy.createPolicyConfiguration("fa74gfbfd-cfnb-45rc-9dfa-490fhf74tfg1dd", true, false, settings);
```

- **Delete a policy configuration**

Delete a policy configuration using configuration id.

```java
policy.deletePolicyConfiguration(2);
```

- **Get a list of policy configurations**

Get a list of all available policy configurations.

```java
policy.getPolicyConfigurations();
```

- **Get a policy configuration**

Get a policy configuration using id.

```java
policy.getPolicyConfiguration(1);
```

- **Update a policy configuration**

Update an existing policy configuration. You need the configuration to update the specific configuration.

One such example [here](https://docs.microsoft.com/en-us/rest/api/azure/devops/policy/configurations/update?view=azure-devops-rest-6.1#examples).

```java
policy.updatePolicyConfiguration(
    1, // configuration id
    "fa74gfbfd-cfnb-45rc-9dfa-490fhf74tfg1dd", // Type id
    true, // Is Enabled
    false, // is blocking
    settings // Map of settings to update the configuration
);
```

- **Get a list of policy types**

Get a list of all policy types.

```java
policy.getPolicyTypes();
```