# Graph

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/graph/?view=azure-devops-rest-6.1)
- API Version: 6.1

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `GraphApi`.

```java
var graph = webApi.getGraphApi();
```

We have access to the functionalities that **Graph** Api has.

### Available functions

- **Create a new user**

Create a new user. You need to provide user descriptor to successfully create a user account. This simply means that you've to clone (or materialize) an existing user using the `user descriptor`.

You can get the user descriptor by running `getUsers` method.

```java
graph.createUser("John.doe@gmail.com", "user-descriptor");

// Get the user descriptor from an existing user
graph.getUsers()
    .getUsers()
    .stream()
    .filter(x -> x.getDisplayName().equals("Jane.doe@gmail.com"))
    .findFirst()
    .get()
    .getDescriptor();
```

- **Add user to group**

After successfully creating a user you can add the user account to an existing group.

```java
graph.addUserToGroup("John.doe@gmail.com", "group-descriptor");

// Get group descriptor
graph.getGroups()
    .getGraphGroups()
    .stream()
    .filter(x -> x.getDisplayName().equals("Readers"))
    .findFirst()
    .get()
    .getDescriptor();
```

- **Delete a user**

Delete a user using user descriptor.

```java
graph.deleteUser("user-descriptor");
```

- **Get a user**

Get a user with user descriptor.

```java
graph.getUser("user-descriptor");
```