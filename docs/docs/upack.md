# Universal Package

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/artifactspackagetypes/universal?view=azure-devops-rest-7.1)
- API Version: 7.1

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

You can omit the project name for calling Universal Package API as it is not a mandatory parameter. If you do omit the project name you will end up working with feeds across your organisation. Provide the project name if you want to work with feeds scoped to a project. You can always set the project name parameter pointing to different project by creating a 'Connection' object.

```java
var upack = webApi.getUPackApi();
```

Now that we have the connection object `webApi`, we can get the instance of `UPackApi`.

### Available functions

#### Get a package

Get information about a package.

```java
upack.getPackageVersion("myFeed", "myGroupId", "myArtifactId", "1.0.0");
```

#### Get a deleted package

Get information about a deleted package

```java
upack.getPackageVersion("myFeed", "myGroupId", "myArtifactId", "1.0.0", true);
```

#### Get package from recycle bin

Get details of a package from recycle bin.

```java
upack.getPackageVersionFromRecycleBin("myFeed", "myGroupId", "myArtifactId", "1.0.0");
```

#### Update the state for a package version

Update the state for a package version.

```java
upack.updatePackageVersion("myFeed", "myGroupId", "myArtifactId", "1.0.0", upackPackagePromote.RELEASE);
```

#### Update the state for a package version for custom view

Update the state for a package version if you have a custom view.

```java
upack.updatePackageVersion("myFeed", "myGroupId", "myArtifactId", "1.0.0", "CustomView");
```

#### Delete a package

Delete a package version from the feed and move it to recycle bin.

```java
upack.deletePackageVersion("myFeed", "myGroupId", "myArtifactId", "1.0.0");
```

#### Restore a package from recycle bin

Restore a package version from the recycle bin to its associated feed..

```java
upack.restorePackageVersionFromRecycleBin("myFeed", "myGroupId", "myArtifactId", "1.0.0");
```

#### Permanently delete a package

Permanently delete a package from a feed's recycle bin.

```java
upack.deletePackageVersionFromRecycleBin("myFeed", "myGroupId", "myArtifactId", "1.0.0");
```

#### Get the upstream behaviour

Get the upstreaming behavior of a package within the context of a feed.

```java
upack.getUpstreamingBehavior("myFeed", "myGroupId", "myArtifactId");
```

#### Set or update package upstream behaviour

Allow externally sourced versions for your package.

```java
upack.setUpstreamingBehavior("myFeed", "myGroupId", "myArtifactId");
```

Allow externally sourced versions for your package or clear the upstreaming behaviour.

```java
upack.setUpstreamingBehavior("myFeed", "myGroupId", "myArtifactId", "AllowExternalVersions"); // allow externally sourced versions.
upack.setUpstreamingBehavior("myFeed", "myGroupId", "myArtifactId", "auto"); // to clear the upstreaming.
```

#### Clear upstream behaviour

Clear the upstream behaviour of your package.

```java
upack.clearUpstreamingBehavior("myFeed", "myGroupId", "myArtifactId");
```

#### Update a package

Promote packages to Release from the feed.

```java
List packages = new ArrayList<>();

Map<String, Object> p1 = new HashMap<>();
p1.put("group", "myGroupId1");
p1.put("artifact", "myArtifactId1");
p1.put("version", "1.0.0");
packages.add(p1);

Map<String, Object> p2 = new HashMap<>();
p2.put("group", "myGroupId2");
p2.put("artifact", "myArtifactId2");
p2.put("version", "1.0.0");
packages.add(p2);

upack.updatePackageVersions("myFeed", "Release", PackagesBatchOperation.PROMOTE, packages);
```

Delete packages from the feed and move it to the feed's recycle bin. (ViewId will be ignored).

```java
upack.updatePackageVersions("myFeed", "Release", PackagesBatchOperation.DELETE, packages)
```

#### Permanently delete a package from recycle bin

Permanently delete packages from a feed's recycle bin.

```java
upack.updateRecycleBinPackages("myFeed", PackagesBatchOperation.PERMANENTDELETE, packages);
```

Restore packages from the recycle bin to its associated feed.

```java
upack.updateRecycleBinPackages("myFeed", PackagesBatchOperation.RESTORETOFEED, packages)
```
