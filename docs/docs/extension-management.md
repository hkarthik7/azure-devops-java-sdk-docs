# Extension Management

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/extensionmanagement/installed-extensions?view=azure-devops-rest-6.1)
- API Version: 6.1-preview

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `ExtensionManagementApi`.

```java
var extension = webApi.getExtensionManagementApi();
```

We have access to the functionalities that **Extension Management** Api has.

### Available functions

- **Get an extension**

Get an extension with extension Id and publisher id.

```java
core.getExtension("SonarQube", "sonar-source");
```
