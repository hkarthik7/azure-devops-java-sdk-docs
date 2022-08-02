# Security

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/security/?view=azure-devops-rest-7.1)
- API Version: 7.1

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `SecurityApi`.

```java
var security = webApi.getSecurityApi();
```

We have access to the functionalities that **Security** Api has.

!!! note

    **SecurityApi** incorporates [IdenitiesApi](https://docs.microsoft.com/en-us/rest/api/azure/devops/ims/?view=azure-devops-rest-7.1)  functionality as well.

### Available functions

#### List namespaces

Get all available namespaces.

```java
security.getNamespaces();
```

#### Get a namespace

Get a specific security namespace.

```java
var ns = security.getNamespace(SecurityToken.Scope.ReleaseManagement.getNamespace());
```

#### List the identites from descriptor

Get the identity descriptor by passing user descriptor.

```java
// Get the descriptor for a user from GraphApi.
var identities = security.getIdentitiesFromSubjectDescriptors(user.getDescriptor());
var securityDescriptor = identities.getIdentities().get(0).getDescriptor();
```

#### Generate a resource security identifier token

Generate a resource security identifier token.

```java
var resourceToken = SecurityToken.generate(SecurityToken.Scope.GIT,
Map.of("PROJECT_ID", "05d37331-f4e4-4c55-9830-37c64e50346d",
        "REPO_ID", "da9108b8-4ed4-41db-a44f-8a428a355772"
));
```

#### Get access control lists for user and resource

Get access control lists for user and resource

```java
var identities = security.getIdentitiesFromSubjectDescriptors(user.getDescriptor());
var securityDescriptor = identities.getIdentities().get(0).getDescriptor();

// boolean fields are 'includeExtendedInfo' and 'recurse'
security.getAccessControlLists(SecurityToken.Scope.GIT.getNamespace(),
        new String[]{securityDescriptor},
        resourceToken,
        false,
        false
);
```

#### Remove access control entries

Remove access control entries.

```java
security.removeAccessControlEntries(SecurityToken.Scope.GIT.getNamespace(),
        new String[]{securityDescriptor},
        new String[]{resourceToken}
);
```

#### Set access control entries

Set access control entries

```java
// create access control entry objects with bitmask allow/deny permission (not included implies inherited)
var entries = new ACEs();
entries.setToken(resourceToken);
entries.setMerge(false); // replace or merge with existing in scope entries

var entry = new ACE();
entry.setDescriptor(securityDescriptor);
entry.setAllow(133); // bitmask corresponding to namespace actions identified above. i.e. 133=128+4+1
entry.setDeny(10);

entries.setAccessControlEntries(List.of(entry));

// apply ACEs
security.setAccessControlEntries(SecurityToken.Scope.Build.getNamespace(), entries);
```
