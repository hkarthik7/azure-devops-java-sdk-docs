# Accounts

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/account/accounts/list?view=azure-devops-rest-6.1)
- API Version: 7.1-preview.1

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `AccountsApi`.

```java
var accounts = webApi.getAccountsApi();
```

We have access to the functionalities that **Accounts** Api has.

### Available functions

#### Get a list of accounts that a member or an user has access to

```java
accounts.getAccounts("member-id");
```

To get the member id of any member/user you should call **Member Entitlement Management** Api. It gives you the flexibility of filtering the member id based on the display name. To use `Member Entitlement Management` Api you need an instance of it.

You can then use the `memberId` to get the list of accounts that the member has access to.

```java
var mem = webApi.getMemberEntitlementManagementApi();

var memberId = mem
        .getUserEntitlements()
        .getMembers()
        .stream()
        .filter(x -> x.getUser().getDisplayName().contains("my-name"))
        .findFirst()
        .get()
        .getId();

accounts.getAccounts(memberId);
```

#### Get a list of Organizations that an user has access to

Note that this is not exposed in **Accounts** Api, it is a helper method to get the list of available organizations that an user has access to.

```java
accounts.getOrganizations();
```

#### Get an user profile

You can get your profile by accessing the below method and optionally you can pass the specific user id to get the respective user's profile.

```java
accounts.getProfile();

accounts.getProfile("user-id");
```

You can get an user's id by calling **Graph** Api.

!!! note

    All methods throw `Connection` and `AzDException` and you should handle it in your code.
    `Connection` Exception is thrown if a connection object is created without any parameters or
    null values are supplied to the parameters.
    `AzDException` throws an user friendly message with an error type to easily identify what has
    gone wrong.
