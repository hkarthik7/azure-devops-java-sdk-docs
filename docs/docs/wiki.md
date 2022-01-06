# Wiki

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/wiki/wikis?view=azure-devops-rest-6.1)
- API Version: 6.1

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `WikiApi`.

```java
var wiki = webApi.getWikiApi();
```

We have access to the functionalities that **Wiki** Api has.

### Available functions

- **Create a wiki page**

Create a wiki page from the existing repository.

```java
var projectId = c.getProject("myProject").getId();
var repoId = g.getRepository("testRepository").getId();
var wikiPage = w.getWiki("NewWiki").getName();

wiki.createWiki("develop", WikiType.CODEWIKI, "MyProjectWiki", projectId, repoId, "/");
```

- **Get a wiki page**

Get a wiki page.

```java
wiki.getWiki("MyProjectWiki");
```

- **Get a list of wiki pages**

Get all wiki pages.

```java
wiki.getWikis();
```

- **Delete a wiki**

Delete a wiki page.

```java
wiki.deleteWiki("MyProjectWiki");
```
