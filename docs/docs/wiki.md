# Wiki

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/wiki/wikis?view=azure-devops-rest-6.1)
- API Version: 7.1

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `WikiApi`.

```java
var wiki = webApi.getWikiApi();
```

We have access to the functionalities that **Wiki** Api has.

### Available functions

#### Create a wiki page

Create a wiki page from the existing repository or a project wiki from scratch. 

- Create project wiki with the name of wiki, project id and wiki type as project wiki.

```java
var core = webApi.getCoreApi();
var projectId = core.getProject("myProject").getId();

var wikiCreateParams = new WikiCreateParameters("MyWiki", projectId, WikiType.PROJECTWIKI);
System.out.println(wiki.createWiki(wikiCreateParams)); // Returns a WikiV2 object.
```

- Create code wiki with wiki name, branch name of the repository, project id, repository id and path of the documents.

```java
var core = webApi.getCoreApi();
var git = webApi.getGitApi();

var projectId = core.getProject("myProject").getId();
var repoId = git.getRepository("testRepository").getId();

var versionDescriptor = new GitVersionDescriptor();
versionDescriptor.setVersion("develop"); // Version is the name of branch for code wiki.

var wikiCreateParams = new WikiCreateParameters("/docs", "NewWiki", projectId, repoId, WikiType.CODEWIKI, versionDescriptor);
wiki.createWiki(wikiCreateParams);
```

#### Get a wiki page

Get a wiki page.

```java
wiki.getWiki("MyProjectWiki");
```

#### Get a list of wiki pages

Get all wiki pages.

```java
wiki.getWikis();
```

#### Delete a wiki

Delete a wiki page.

```java
wiki.deleteWiki("MyProjectWiki");
```

#### Create or Update a Wiki page

Create or edit a wiki page. ETag of wiki page (version) is required if you are editing an existing page.

```java
var wikis = w.getWikis().getWikiPages();
var wikiId = wiki.get(0).getId();
var page = "DevOps Ways of Working";
String contents = "# DevOps Ways of Working \n This is the first line \n This is the second line.";

wiki.createOrUpdateWikiPage(wikiId, page, "Page initial commit", null, "develop", GitVersionType.BRANCH, GitVersionOptions.NONE, contents);
```

#### Delete a wiki page

Delete a wiki page.

```java
var wikis = w.getWikis().getWikiPages();
var wikiId = wiki.get(0).getId();
var page = "DevOps Ways of Working";

wiki.deleteWikiPage(wikiId, page, "Page deleted", "develop", GitVersionType.BRANCH, GitVersionOptions.NONE);
```

#### Get a Wiki page

Get a wiki page object.

```java
var wikis = w.getWikis().getWikiPages();
var wikiId = wiki.get(0).getId();
var page = "DevOps Ways of Working";

wiki.getWikiPage(wikiId, false, page, VersionControlRecursionType.FULL, "Get wiki page", "develop", GitVersionType.BRANCH, GitVersionOptions.NONE);
```

#### Get a Wiki page by id

Get a wiki page by id.

```java
var wikis = w.getWikis().getWikiPages();
var wikiId = wiki.get(0).getId();
var page = "DevOps Ways of Working";

var wikiPage = wiki.getWikiPage(wikiId, false, page, VersionControlRecursionType.FULL, "Get wiki page", "develop", GitVersionType.BRANCH, GitVersionOptions.NONE);

wiki.getWikiPageById(wikiPage.getId().toString(), wikiId, true, VersionControlRecursionType.FULL);
```

#### Get the contents of the Wiki page

Get the contents of a wiki page. This method returns a string, which means if you've any attachments in the Wiki page that won't be returned.

```java
var wikis = w.getWikis().getWikiPages();
var wikiId = wiki.get(0).getId();
var page = "DevOps Ways of Working";

var wikiPage = wiki.getWikiPage(wikiId, false, page, VersionControlRecursionType.FULL,
        "Get wiki page", "develop", GitVersionType.BRANCH, GitVersionOptions.NONE);

wiki.getWikiPageContent(wikiPage.getId().toString(), wikiId);
```

#### Download the Wiki page as a Zip file

Download the wiki page as a zip file.

```java
var wikis = w.getWikis().getWikiPages();
var wikiId = wiki.get(0).getId();
var page = "DevOps Ways of Working";

var wikiPage = wiki.getWikiPage(wikiId, false, page, VersionControlRecursionType.FULL,
        "Get wiki page", "develop", GitVersionType.BRANCH, GitVersionOptions.NONE);

// Input stream response from Api.
var res = wiki.getWikiPageAsZip(wikiPage.getId().toString(), wikiId);

StreamHelper.download(page + ".zip", res);
```


#### Update a wiki page

Update a wiki page. This Api demands the ETag version to update the contents of a wiki page successfully.

```java
var wikis = w.getWikis().getWikiPages();
var wikiId = wiki.get(0).getId();
var page = "DevOps Ways of Working";

var wikiPage = w.getWikiPage(wikiId, false, page, VersionControlRecursionType.FULL,
        "Get wiki page", "develop", GitVersionType.BRANCH, GitVersionOptions.NONE);

wiki.updateWikiPage(wikiPage.getId().toString(), wikiId, "Updated page content", wikiPage.geteTag(), "# Heading\n" +
        "This is updated content. \n ## Second Heading \n Place holder for safe landing project.");
```
