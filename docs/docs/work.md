# Work

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/work/?view=azure-devops-rest-6.1)
- API Version: 6.1

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `WorkApi`.

```java
var work = webApi.getWorkApi();
```

We have access to the functionalities that **Work** Api has.

### Available functions

#### Get all team iterations

Get a list of team iterations.

```java
work.getTeamSettingsIterations("myTeam");
```

#### Get team settings iteration with time frame

Get all team settings iterations with time frame.

```java
work.getTeamSettingsIterations("myTeam", IterationsTimeFrame.CURRENT);
```

#### Get work items from team settings iterations

Get work items from team settings iterations.

```java
var id = work.getTeamSettingsIterations("myTeam").getIterations().stream().findFirst().get().getId();
var res = work.getTeamIterationWorkItems("myTeam", id);
res.get_links();
```

#### Delete a team iteration

Delete a team iteration using team iteration id.

```java
work.deleteTeamSettingsIteration("myTeam", "0000-00000-00000-00000-00000"); // team iteration GUID here.
```
