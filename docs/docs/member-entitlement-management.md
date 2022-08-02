# Member Entitlement Management

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/memberentitlementmanagement/?view=azure-devops-rest-6.1)
- API Version: 7.1

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `MemberEntitlementManagementApi`.

```java
var mem = webApi.getMemberEntitlementManagementApi();
```

We have access to the functionalities that **Member Entitlement Management** Api has.

### Available functions

#### Get all group entitlements

Get a list of group entitlements.

```java
mem.getGroupEntitlements();
```

#### Get members from a group

Get members from a group using group id.

```java
mem.getMembers("group-id");
```

#### Remove user from a group

Remove a member from the group using group id and member id.

```java
mem.removeMemberFromGroup("group-id", "member-id");
```

#### Add an user entitlement

Add a user and assign a license.

```java
mem.addUserEntitlement(AccountLicenseType.STAKEHOLDER, "jane.doe@gmail.com", GroupType.PROJECTCONTRIBUTOR, "project-id");
```

-**Delete an user entitlement**

Delete a user entitlement.

```java
mem.deleteUserEntitlement("user-id");
```

#### Get user entitlements

Get a list of user entitlements.

```java
mem.getUserEntitlements();
```

#### Update an user entitlement

Update an user's account type or licensing source.

```java
mem.updateUserEntitlement("user-id", AccountLicenseType.EXPRESS, LicensingSource.MSDN);
```
