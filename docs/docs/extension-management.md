# Extension Management

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/extensionmanagement/installed-extensions?view=azure-devops-rest-6.1)
- API Version: 6.1-preview

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `ExtensionManagementApi`.

```java
var extensions = webApi.getExtensionManagementApi();
```

We have access to the functionalities that **Extension Management** Api has.

### Available functions

#### Get an extension

Get an extension with extension name and publisher id.

```java
extensions.getExtension("sonarqube", "sonarsource");
```

#### Get a list of installed extensions

Get a list of installed extensions.

```java
extensions.getExtensions();
```

#### Install an extension

Install an extension with extension name, publisher id and version. If version is null, latest version will be selected.

```java
extensions.installExtension("sonarsource", "sonarqube", null);
```

#### Uninstall an extension

Uninstall an extension with extension name and publisher id.

```java
extensions.uninstallExtension("sonarsource", "sonarqube");
```

#### Update an extension

You can enable and disable an extension with this API. If NONE is specified extension will be enabled.

```java
extensions.updateExtension("sonarsource", "sonarqube", ExtensionStateFlags.DISABLED);
```

!!! note

    You need administrator rights in organization level to manage the extensions. If you
    only have project level permission you can't install or uninstall an extension.
