# Release

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/release/?view=azure-devops-rest-6.1)
- API Version: 6.1

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `ReleaseApi`.

```java
var release = webApi.getReleaseApi();
```

We have access to the functionalities that **Release** Api has.

### Available functions

#### Create a release

Create a release using release pipeline/definition id, artifact alias name, artifact id, artifact name and optionally set it to draft. You can get the artifact alias name from `getReleaseDefinition` method and by passing the release definition id. Artifact id is build id and artifact name is the name of build pipeline. In this case it is `Demo-CI`.

```java
// Create a release using release definition id, artifact alias name, build id,  build pipeline name and is draft to false.
release.createRelease(2, "description", "_Demo-CI", 176, "Demo-CI", false);

```

!!! note

    Using the `createRelease` method won't actually deploy anything to the available stages. It creates a release and will be ready to kick
    off the deployment.

#### Get a release

Get a release using release id.

```java
release.getRelease(33);
```

#### Get release environment

Get environments associated to a release. You need environment id to get the associated environment(s). You can get it from the release object.

```java
release.getReleaseEnvironment(33, 4);
```

#### Get all releases

Get a list of all releases.

```java
release.getReleases();
```

#### Create a release definition

To create a release definition you need the json string from existing release definition. This is the easiest way to create a release definition. Download the json using `Export` option from the `Releases` section of any pipeline and modify it. Then you can use it as a reference to create a definition.
Alternatively you get the `Release` object from the existing pipeline and convert it to string and then create a pipeline based on that. 

```java
release.createReleaseDefinition(releaseDefinitionParameters);
```

#### Delete a release definition

Delete a release definition using definition id.

```java
release.deleteReleaseDefinition(3);
```

#### Get a list of release definitions

Get a list of release definitions.

```java
release.getReleaseDefinitions();
```