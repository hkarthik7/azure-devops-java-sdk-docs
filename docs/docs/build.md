# Build

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/build/builds?view=azure-devops-rest-6.1)
- API Version: 7.1-preview

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `BuildApi`.

```java
var build = webApi.getBuildApi();
```

We have access to the functionalities that **Build** Api has.

### Available functions

#### Get a list of builds

```java
build.getBuilds(); // Gives you Build object.
build.getBuilds().getBuildResults(); // Gives you list of builds to loop through.
```

This returns a list of build object which you can iterate through and get the build Id, build definition or pipeline name etc.

#### Create a build definition (aka build pipeline)

`createBuildDefinition` method accepts the build definition parameters as a string. So, you have to create a json string with all the parameters your need and call this method. There are two ways you can achieve this

1. Get the build definition parameters from the existing pipeline, store it in a file, edit it and use it in your code.
2. The second way is to get the build definition object from existing pipeline using `getBuildDefinition` method and use `convertToString` method from `JsonMapper`.

You can choose either of the ways that suits your requirement or scenario.

```java
// assuming that you have stored the json build definition template in a file
var definition = Files.readString(Path.of("definition.json"));
build.createBuildDefinition(definition);
```

```java
var buildDefinition = build.getBuildDefinition(22);
buildDefinition.setName("Deploy-WebApp-CI-Copy");
var mapper = new JsonMapper();
var definitionString = mapper.convertToString(buildDefinition);

// now we're ready to create the definition
build.createBuildDefinition(definitionString);
```

!!! note

    There is a helper method available to clone the existing pipeline and create a copy of it.
    You can use `cloneBuildDefinition` if you want to clone the existing pipeline and later edit it in the UI.

#### Queue a build

Kick off a build by passing the build id.

```java
build.queueBuild(9);
```

#### Delete a build

Deletes a build with build Id.

```java
build.deleteBuild(77);
```

#### Get the changes associated with a build

Get the changes associated with a build by passing the build Id.

```java
build.getBuildChanges(78);
```

#### Get logs of a build

Get the logs of a build with build id and log id.

```java
build.getBuildLog(87, 4);

// alternatively get all build logs from a build by passing only build Id
build.getBuildLogs(87)
```

#### Get all build work items associated to a build

Get all build work items.

```java
build.getBuildWorkItems(87);
```

#### List all the changes between two builds

Get all the changes between two builds and top number of changes to return.

```java
build.getChangesBetweenBuilds(77, 87, 5);
```

#### Clone a build definition

This is a helper method to easily and quick clone a build pipeline.

```java
String pipelineName = "Deploy-WebApp-CI";
String pipelineToCloneName = "Deploy-WebApp-CI-Copy";

build.cloneBuildDefinition(pipelineName, pipelineToCloneName);
```

#### Delete a build definition

Delete a build definition/pipeline by definition id.

!!! tip

    You can get the build definition id from the **Azure DevOps services** URL or
    you can run `getBuildDefinitions` and filter out the build definition name and
    it's id.

```java
build.deleteBuildDefinition(24)

// get the build definition id from URL
// https://dev.azure.com/<organization-name>/<project-name>/_build?definitionId=24 --> That's the definition id
// or you can use build definitions API to get the results and you can filter it.
int definitionId = build.getBuildDefinitions()
    .getBuildDefinition()
    .stream()
    .filter(x -> x.getName().equals("Deploy-WebApp-CI"))
    .findFirst()
    .get()
    .getId();

// Now call the delete definition API
build.deleteBuildDefinition(definitionId)
```

#### Restore build definition

Restore a deleted build definition/pipeline

```java
// if false the deleted build definition will be restored
build.restoreBuildDefinition(24, false)
```

#### Update build definition

Update a build pipeline or definition. The below example shows how to update a build definition's description.

```java
// Get the exiting definition by definition id
var def = build.getBuildDefinition(23);
def.setDescription("Build pipeline for the my project");

build.updateBuildDefinition(def)
```

#### Create a folder

Create folder to organize your build pipelines.

```java
var folder = new Folder();
folder.setDescription("New demo folder");
folder.setPath("\\Demo-Folder\\sub-demo"); // create a sub folder

// create a folder by passing the folder path and folder object.
build.createFolder(folder.getPath(), folder);
```

#### Delete a folder

Delete a folder with the folder path.

```java
build.deleteFolder("\\Demo-Folder\\sub-demo");
```

#### Get all the folders

Get a list of folder available.

```java
build.getFolders();
```

#### Update a folder

Update a folder.

```java
var folder = build.getFolders().getFolders().get(1);
folder.setDescription("Demo folder for azd project");

build.updateFolder(folder.getPath(), folder);
```

#### Add build tag

Add tag to a build by passing build Id and tag name. This helps you to filter out the builds based on tags.

```java
build.addBuildTag(77, "Demo")
```

#### Add multiple tags to a build

Add multiple tags to a build. This API accepts a `String array` of tags.

```java
build.addBuildTags(77, new String[]{ "Demo", "Test" })
```

#### Add definition tag

Add a tag to a build definition/pipeline.

```java
build.addDefinitionTag(22, "WebAppCI");
```

#### Add multiple tags to build definition

Add multiple tags to build definition.

```java
build.addDefinitionTags(22, new String [] { "WebAppCI", "Stage-Build" });
```

#### Delete a build tag

Delete a build tag with tag name and build id.

```java
build.deleteBuildTag(77, "Demo");
```

#### Delete definition tag

Delete a tag attached to a build definition.

```java
build.deleteDefinitionTag(22, "WebAppCI");
```

#### Delete a tag

Delete a tag from tag store.

```java
build.deleteTag("buildTag");
```

!!! note

    You can't delete the tag(s) with special characters using `delete` methods. To delete the tags with special
    characters use `updateBuildTags` and `updateDefinitionTags` methods.

#### Update a build tag

Update a build tag and optionally specify if the tag has to be removed or not.

```java
int buildId = 77;
String[] tags = new String[] { "Demo", "Test", "TestDeploy" };
boolean toRemove = false;

build.updateBuildTags(buildId, tags, toRemove);

// Optionally you can use this method to remove the tags with special characters such as -, _ etc.
String[] tags = new String[] { "Stage-Build" };
boolean toRemove = true;

// This will remove the tags with special characters 
build.updateBuildTags(buildId, tags, toRemove);
```

#### Get file contents

Get a source provider's file contents from the repository. Given that you should provide the source provide name which can be fetched from method
`build.getSourceProviders()`.

```java
// If the complete repository name is not provided you will get an error.
build.getFileContents("Github", "a7054ra9-0a34-46ac-bfdf-b8a1da865tdfd6", "userName/repositoryName", "master", "LICENSE");
```

!!! tip

    You can get the service endpoint Id from **ServiceEndpointApi** and method `getServiceEndpoints()`.

#### Get path contents

Get the contents of source provider repository from a particular path.

```java
build.getPathContents("Github", "a7054ra9-0a34-46ac-bfdf-b8a1da865tdfd6", "userName/repositoryName", "master", "/");
build.getPathContents("Github", "a7054ra9-0a34-46ac-bfdf-b8a1da865tdfd6", "userName/repositoryName", "master", "/Classes");
build.getPathContents("Github", "a7054ra9-0a34-46ac-bfdf-b8a1da865tdfd6", "userName/repositoryName", "master", "/Models/Data");
```

#### Get a pull request

Get a pull request from a source provider repository.

```java
build.getPullRequest("Github", "2", "userName/repositoryName", "a7054ra9-0a34-46ac-bfdf-b8a1da865tdfd6");
```

#### Get a list of source providers

```java
build.getSourceProviders();
```

#### Get all branches

Get all available branches or a particular branch from a source provider repository.

```java
build.getBranches("Github", "a7054ra9-0a34-46ac-bfdf-b8a1da865tdfd6", "userName/repositoryName");
```

#### Get source providers repositories

Get all available repositories or a particular repository from a source provider.

```java
build.getRepositories("Github", "a7054ra9-0a34-46ac-bfdf-b8a1da865tdfd6");
```

#### Get web hooks

Get a list of web hooks associated to a source provider repository.

```java
build.getWebHooks("Github", "a7054ra9-0a34-46ac-bfdf-b8a1da865tdfd6", "userName/repositoryName");
```

#### Create a build artifact

Create a new build artifact or attach the existing artifact to other build.

```java
// Get the build artifact from a build
var artifact = build.getArtifact(1629, "Test");

// create the artifact in other build
build.createArtifact(176, artifact);
```

#### Download the build artifact

Download the build artifact as zip.

```java
var res = build.getArtifactAsZip(1593, "drop");
StreamHelper.download("drop.zip", res);
```

#### Update a build

Update a build and optionally retry the failed build.

```java
var buildObj = build.getBuild(buildId);
buildObj.setTags(new String[]{"Demo"});

// This updates the build tags
build.updateBuild(build, buildObj.getId(), false);
```

!!! note

    Set retry to true only when previous attempt of a build has failed and if retry is true build object
    should be set to null.

```java
build.updateBuild(null, buildObj.getId(), true);
```

#### Update multiple builds

Update multiple builds. This will be useful if you want to update a similar settings for many builds.

```java
var builds = build.getBuilds(2);

for (var b : builds.getBuildResults()) {
    b.setPriority(QueuePriority.LOW);
}

build.updateBuilds(builds);
```

!!! note

    There are many other functionalities that BuidApi offers, if you find any missing feature
    you can open a new feature request in [github](https://github.com/hkarthik7/azure-devops-java-sdk/issues)
    or create a pull request and add the functionality.
