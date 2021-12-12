# Quick Start

## Acquire Personal access token

Before getting started you will require personal access token to authenticate **Azure DevOps services** REST API.
You can grab one by following the [documentation](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?WT.mc_id=docs-github-dbrown&view=azure-devops&tabs=preview-page).

## Create A Connection Object

Before using any methods in the library you should connect to **Azure DevOps services** Api. You can achieve this by creating a connection object. **Azure DevOps services** require *Organization name*, *Project name* and *Personal access token* to successfully authenticate to the REST API. You should create a connection object with all these parameters.

For instance refer this URL that allows you to list the available builds from pipelines, `https://dev.azure.com/{organization}/{project}/_apis/build/builds?api-version=6.1-preview.7`. You can get the *organization name* and *project name* from your **Azure DevOps services** URL similar to this.

Assuming that you have *Organization name*, *Project name* and *Personal access token* ready to access the REST API.

You can create a **Connection** object using **AzDClientApi** class in the library as shown below.

```java
String organizationName = "my-organization";
String projectName = "my-project";
String personalAccessToken = "7d45tsdgfh7ehYgd648jksy6grj847dGtFRk9";

var webApi = new AzDClientApi(organizationName, projectName, personalAccessToken);
```

This connection object returns an instance of the respective API that you call. To get started on how to use the connection object and to know all available functionalities that **azd** has to offer jump to next section.
