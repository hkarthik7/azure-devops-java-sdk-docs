# Distributed Task

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/distributedtask/?view=azure-devops-rest-6.1)
- API Version: 6.1-preview

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `DistributedTaskApi`.

```java
var task = webApi.getDistributedTaskApi();
```

We have access to the functionalities that **Distributed Task** Api has.

### Available functions

#### Delete an agent

Delete a task agent by pool id and agent id.

!!! Tip

    You can get the agent Id and pool Id from the URL when you navigate to the agent pools section under
    organization Settings. Go to Organization settings --> Pipelines --> Agent Pools --> Select any agent.
    For the agent pool *Azure Pipelines* which is used for default tasks, if you click on agents tab
    you can get the pool Id and agent Id from the URL
    https://dev.azure.com/organization-name/project-name/_settings/agentpools?agentId=8&poolId=9&view=jobs

```java
task.deleteAgent(9, 8);
```

#### Get an agent

Get a task agent by pool id and agent id.

```java
task.getAgent(9, 8);
```

#### Get all agents

Get a list of all available agents with pool id.

```java
task.getAgents(9);
```

#### Update an agent

Update a task agent. You can get the parameters for request body from the API [documentation](https://docs.microsoft.com/en-us/rest/api/azure/devops/distributedtask/agents/update?view=azure-devops-rest-6.1#request-body).

```java
task.updateAgent(9, 8, requestBody);
```

#### Add a deployment group

Add a deployment group with name and description.

```java
task.addDeploymentGroup("myDeploymentGroup", "This is a test deployment group");

/// Optionally create a deployment group and add a deployment pool to it
task.addDeploymentGroup("myDeploymentGroup", "This is a test deployment group", 9);
```

#### Delete a deployment group

Delete a deployment group by id.

```java
task.deleteDeploymentGroup("deploymentGroupId");
```

#### Get a deployment group

Get a deployment group by id.

```java
task.getDeploymentGroup("deploymentGroupId");
```

#### Get all deployment groups

Get a list of deployment groups.

```java
task.getDeploymentGroups();
```

#### Update a deployment group

Update a deployment group's name and description by passing it's id.

```java
task.updateDeploymentGroup("deploymentGroupId", "myNewDeploymentGroup", "This is my new deployment group");
```

#### Add an environment

Add an environment by passing name and description.

```java
task.addEnvironment("myEnvironment", "Description for my environment");
```

#### Delete an environment

Delete an environment by id.

```java
task.deleteEnvironment("environmentId");
```

#### Get an environment

Get an environment by id.

```java
task.getEnvironment("environmentId");
```

#### Get all environments

Get a list of environments.

```java
task.getEnvironments();
```

#### Update an environment

Update an environment's name or description.

```java
task.updateEnvironment("environmentId", "myNewEnvironment", "Description for my new environment");
```

#### Add a variable group

Add a variable group.

```java
var definition = new VariableGroupDefinition();
var projectReference = new ProjectReference();
var core = webApi.CoreApi();
var project = core.getProject("myProject");

var variables = new HashMap<String, Object>(){{
    put("userName", new HashMap<String, String>(){{
        put("value", "testUser");
    }});
    put("code", new HashMap<String, Integer>(){{
        put("value", 2255);
    }});
    put("password", new HashMap<String, Object>(){{
        put("value", "Test Value");
        put("isSecret", true);
    }});
}};

projectReference.setName(project.getName());
projectReference.setId(project.getId());

definition.setName("myVariable");
definition.setDescription("Development variable group");
definition.setVariables(variables);
definition.setProjectReference(projectReference);
definition.setType(VariableGroupType.Vsts);

task.addVariableGroup(definition);
```

You can also use the helper method available to add the variable group.

```java
task.addVariableGroup("myVariable", "Development variable group", variables);
```

#### Delete a variable group

Delete a variable group by id.

```java
task.deleteVariableGroup(12, new String[] { "projectId" });
```

#### Get a variable group

Get a variable group by id.

```java
task.getVariableGroup(11);
```

#### Get all variables groups

Get a list of variable groups.

```java
task.getVariableGroups();
```

#### Update a variable group

Update a variable group by id.

!!! warning

    You should pass all the existing variables along with the variable(s) that you want to update.
    Else the existing variables will be removed except the variable(s) that you're updating.

```java
var variablesToUpdate = new HashMap<>(){{
    put("userName", new HashMap<>(){{
        put("value", "testUser");
    }});

    put("password", new HashMap<>(){{
        put("value", "testUser");
        put("isSecret", true);
    }});
}};

var group = d.getVariableGroups("myVariables")
    .getVariableGroups()
    .stream()
    .findFirst()
    .get();

task.updateVariableGroup(group.getId(), group.getName(), group.getDescription(), variablesToUpdate);
```