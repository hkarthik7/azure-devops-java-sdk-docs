# Git

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/git/?view=azure-devops-rest-6.1)
- API Version: 6.1

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `GitApi`.

```java
var git = webApi.getGitApi();
```

We have access to the functionalities that **Git** Api has.

### Available functions

#### Create a new repository

Create a new repository with repo name and project id.

```java
var core = webApi.getCoreApi();
var projectId = core.getProject("myProject").getId();
git.createRepository("WebApp-Deployment-Code", projectId);
```

#### Delete a repository

Delete a repository with repository id. You can get the repository id by running `getRepository` method.

```java
var repoId = git.getRepository("WebApp-Deployment-Code").getId();
git.deleteRepository(repoId);
```

#### Permanently delete a repository

Delete a repository from recycle bin.

```java
var repoId = git.getRepository("WebApp-Deployment-Code").getId();
git.deleteRepositoryFromRecycleBin(repoId);
```

#### List deleted repositories

Get a list of deleted repositories.

```java
git.getDeletedRepositories();
```

#### Get a list of recycle bin repositories

Get a list of repositories from recycle bin.

```java
git.getRecycleBinRepositories();
```

#### Get a repository

Get a repository by name.

```java
git.getRepository("My-repository");
```

#### Get a list of repositories

Get a list of all the repositories.

```java
git.getRepositories();
```

#### Restore a repository

Restore a deleted repository from recycle bin.

```java
// If the deleted parameter is set to false it will restore the deleted repository
git.restoreRepositoryFromRecycleBin(repoId, false);
```

#### Update a repository

Update a repository's default branch or rename a repository.

```java
// Update a repository's default branch
git.updateRepository(repoId, "my-repo", "develop");

// Rename a repository
git.updateRepository(repoId, "my-new-repo", "develop");
```

#### Create a pull request

Create a pull request and optionally set it to draft.

```java
var reviewerId = git.getPullRequestReviewers(24, "repo-name")
    .getPullRequestReviewers()
    .stream()
    .filter(x -> x.getDisplayName().equals("any-name@email.com"))
    .findFirst()
    .get()
    .getId();

git.createPullRequest(
    "repository-id",
    "source-reference-name", // This will be the source branch from which you're merging the code. E.g., develop
    "target-reference-name", // This will be the name of target brach to which you're merging the code. Eg., main
    "Title for the pull request",
    "Description for the pull request",
    new String[] { reviewerId }
);

// create pull request and set it to draft
git.createPullRequest(
    "repository-id",
    "source-reference-name", // This will be the source branch from which you're merging the code. E.g., develop
    "target-reference-name", // This will be the name of target brach to which you're merging the code. Eg., main
    "Title for the pull request",
    "Description for the pull request",
    true
);
```

#### Get a pull request

Get a pull request from a repository.

```java
git.getPullRequest("repository-name", "pull-request-id");
```

#### Get all pull requests from a project

Get all pull requests from a project.

```java
git.getPullRequestsByProject();
```

#### Update a branch lock

Lock and unlock a branch with this API.

```java
// lock develop branch
git.updateBranchLock("my-repo", "develop", true);

// unlock develop branch
git.updateBranchLock("my-repo", "develop", false);
```

#### Get work items associated with a pull request

Get work items associated with a pull request.

```java
git.getPullRequestWorkItems(22, "my-repo");
```

#### Create a pull request label

Add a tag to a pull request.

```java
git.createPullRequestLabel("my-repo", 22, "new-feature");
```

#### Delete a pull request label

Remove a tag from a pull request.

```java
git.deletePullRequestLabel("my-repo", 22, "new-feature");
```

#### Create pull request reviewer

Add a reviewer to a pull request and optionally make them as required.

```java
// 0 - corresponds to vote the reviewer gives for a PR
git.createPullRequestReviewer(32, "my-repo", "reviewer-id", 0, true);
```

!!! note

    There are many functionalities that Git API offers, explore it and feel free to add a new
    feature by opening a [PR](https://github.com/hkarthik7/azure-devops-java-sdk/blob/main/.github/PULL_REQUEST_TEMPLATE.md).
