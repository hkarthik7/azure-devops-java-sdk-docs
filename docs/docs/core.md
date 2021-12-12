# Core

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/core/?view=azure-devops-rest-6.1)
- API Version: 6.1-preview

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `CoreApi`.

```java
var core = webApi.getCoreApi();
```

We have access to the functionalities that **Core** Api has.

### Available functions

- **Get a list of process**

Get a list of available process that you can use to create a project. For instance Agile, Basic, Scrum etc.

```java
core.getProcesses();
```

- **Create a new project**

Create a new `Scrum` project with project name and description.

```java
core.createProject("my-awesome-project", "Description for my awesome project");
```

Create a new project with project template id and default parameters. You can get the project template id by running `getProcesses` method.

```java
String templateId = core.getProcesses()
    .getProcesses()
    .stream()
    .filter(x -> x.getName().equals("Agile"))
    .findFirst()
    .get()
    .getId();

String projectName = "Finance-Management";
String description = "Finance management project";
String sourceControlType = "Git";

core.createProject(projectName, description, sourceControlType, templateId);
```

- **Delete a project with project id**

Delete a project with project id. You can get project id by running `getProject` and passing the project name.

```java
core.deleteProject("project-guid-here");
```

- **Get a project**

Get a project by project name.

```java
core.getProject("my-awesome-project");
```

- **Get a project's properties**

Get a project's properties with project id.

```java
String projectId = core.getProject("my-project").getId();
core.getProjectProperties(projectId);
```

- **Get all projects**

List all available projects.

```java
core.getProjects();
```

- **Update a project**

Update a project's properties.

```java
var projectProperties = new HashMap<String, Object>() {{
    put("name", "new-project-name");
    put("description", "Description for updated project name.")
}};
core.updateProject("my-project", projectProperties);
```

- **Create a team within the project**

Create a team within the project.

```java
core.createTeam("my-project", "my-new-team");
```

- **Delete a team**

Delete a team within the project.

```java
core.deleteTeam("my-project", "my-new-team");
```

- **Get all teams**

Get a list of teams.

```java
core.getTeams();
```

- **Update an existing team within the project**

Update an existing team within the project with project name. Using this method you can only update a team's name and description.

```java
core.updateTeams("my-project", "my-team", "Description for my team");
```