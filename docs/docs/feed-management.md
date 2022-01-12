# Feed Management

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/artifacts/feed-management?view=azure-devops-rest-6.1)
- API Version: 6.1-preview

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `FeedManagementApi`.

```java
var feed = webApi.getFeedManagementApi();
```

We have access to the functionalities that **Feed Management** Api has.

### Available functions

#### Create a feed

Create a feed in the artifacts section.

```java
// create a feed with feed name, description, badges enabled, hide deleted packages version
feed.createFeed("myFeed", "Description for myFeed", true, true);
```

#### Create a feed view within the feed

Create a feed view within the feed.

```java
feed.createFeedView("myFeed", "myFeedView", FeedViewType.IMPLICIT, FeedVisibility.ORGANIZATION);
```

#### Delete a feed with feed id

Delete a feed with feed id. You can get the feed id by calling `getFeed` method and passing the feed name.

```java
String feedId = feed.getFeed("myFeed");
feed.deleteFeed(feedId.getId());
```

#### Get and set feed permissions

Get feed permissions and modify the permissions. To modify the feed permissions you need identity descriptor which can be acquired from
`getFeedPermissions` method.

```java
String identityDescriptor = feed.getFeedPermissions("myFeed")
            .getFeedPermission()
            .stream()
            .filter(x -> x.getDisplayName().equals("any-user-name"))
            .findFirst()
            .get()
            .getIdentityDescriptor();

feed.setFeedPermissions("feedName", "displayName", identityDescriptor, false, "reader");
```

#### Change the attributes of a feed

Update a feed and change it's attributes such as badges enabled, description,  hide deleted packages, upstream enabled.

```java
feed.updateFeed("myFeed", false, "This is new description", true, true);
```

#### Update a feed view

Update a feed view, you can change it's type and visibility.

```java
// In versions 2.5.7 and below you have to specify the type and visibility as string.
// This has been changed in newer version(s).
feed.updateFeedView("TestFeed", "myView", FeedViewType.RELEASE, FeedVisibility.ORGANIZATION);
```