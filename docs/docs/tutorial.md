# Call Azure DevOps REST API

**azd** library provides **Apis** for significant services that takes care of request & response and json deserialization. Not only that, it provides **Apis** to achieve a task with less code. For instance, you can trigger a build pipeline by calling **queueBuild()** method from **BuildApi** with just the id of build pipeline/definition.

Azure DevOps REST Api is vast and not all functionalities are included in **azd** library as it is still evolving and being constantly updated. The aim of this section is to understand the **Apis** that **azd** provides to call any Azure DevOps REST API.

**RestClient** Api exposes few `static` overloaded methods that takes care of building the url, sending request to REST Api and getting the response based on passed parameters. We will explore the REST Api with an example to understand the **RestClient** class better and know how & when to use the methods.

## Example

Api:

- [Base Instance](https://hkarthik7.github.io/azd-docs/org/azd/enums/Instance.html)
- [ResourceId](https://hkarthik7.github.io/azd-docs/org/azd/common/ResourceId.html)
- [ApiVersion](https://hkarthik7.github.io/azd-docs/org/azd/common/ApiVersion.html)
- [RequestMethod](https://hkarthik7.github.io/azd-docs/org/azd/enums/RequestMethod.html)
- [RestClient](https://hkarthik7.github.io/azd-docs/org/azd/utils/RestClient.html)
- [CustomHeader](https://hkarthik7.github.io/azd-docs/org/azd/enums/CustomHeader.html)

### Get a list of builds

Source: [Documentation](https://docs.microsoft.com/en-us/rest/api/azure/devops/build/builds/list?view=azure-devops-rest-7.1)

Let's inspect the url and see how it is constructed.

```url
GET https://dev.azure.com/{organization}/{project}/_apis/build/builds?api-version=7.1-preview.7
```


- **Base instance**: https://dev.azure.com
- **Azure DevOps organization**: {organization}
- **Project**: {project}
- **Api relative path**: _apis
- **Area**: build/builds
- **id**: NA
- **resource**: NA
- **Api version**: 7.1-preview.7

The above section shows how Azure DevOps URL is constructed and this is significant for us to successfully create the request and call the REST API. We need

- Resource id to get the base instance for **BuildApi**, this is provided by [ResourceId](https://hkarthik7.github.io/azd-docs/org/azd/common/ResourceId.html) class.
- Api version, this is provided by [ApiVersion](https://hkarthik7.github.io/azd-docs/org/azd/common/ApiVersion.html) class.

[RestClient](https://hkarthik7.github.io/azd-docs/org/azd/utils/RestClient.html) have `send` method that is created for various scenarios. Each method's return type is different and accepts various parameters. In this example we use `send` method that returns `String` as we need json string response from Api. Here the request method is GET so we use [RequestMethod](https://hkarthik7.github.io/azd-docs/org/azd/enums/RequestMethod.html) to construct the request.

1. Request Method - GET
2. Connection Object
3. Resource Id - Build
4. Project Name
5. Area - build/builds
6. Id - Since there is no id, the value is null
7. resource - null
8. Api version - Build Api version
9. Query string - null as there is no query string in the request
10. Request body - null as the request type is GET
11. header - application/json, as the Api returns a json string as response

```java
// Create a connection object.
String organizationName = "my-organization";
String projectName = "my-project";
String personalAccessToken = "7d45tsdgfh7ehYgd648jksy6grj847dGtFRk9";

var webApi = new AzDClientApi(organizationName, projectName, personalAccessToken);

// Connection object is exposed via webApi in getConnection() method
// Call the REST API

var res = RestClient.send(RequestMethod.GET, webApi.getConnection(), ResourceId.BUILD, projectName,
    "build/builds", null, null, ApiVersion.BUILD, null, null, CustomHeader.JSON);

// To add a query string, you create a map of key and value. 
// Say that you want to list only top 10 builds then the query string can be constructed as
var query = Map.of("$top", 10);
var res = RestClient.send(RequestMethod.GET, webApi.getConnection(), ResourceId.BUILD, projectName,
    "build/builds", null, null, ApiVersion.BUILD, query, null, CustomHeader.JSON);

System.out.println(res);

// You can deserialize the json response to java object using JsonMapper helper class which is extended from ObjectMapper
// of Jackson databind. JsonMapper provides the functionality of error handling from Api for you.

var mapper = new JsonMapper();

System.out.println(MAPPER.mapJsonResponse(res, Builds.class).getBuildResults());

// If a type is not available convert the response to Json tree to traverse through it.
// Here the return json is an array of builds which is encapsulated in {"count": <int>, "value": [] } format.
// We get the value and extract first Build object.
System.out.println(MAPPER.convertToJson(res).get("value").get(0));
```

You can also call the REST API with entire URL string as

```java
String url = MessageFormat.format("https://dev.azure.com/{0}/{1}/_apis/build/builds?api-version=7.1-preview.7",
    organization, project);

// Or

String url = MessageFormat.format("{0}/{1}/{2}/_apis/build/builds?api-version={3}",
    Instance.BASE_INSTANCE.getInstance(), organization, project, ApiVersion.BUILD);

var result = RestClient.send(url, RequestMethod.GET, null, CustomHeader.JSON, false);
```

Other methods are similar, it is that you should pass the request body for Post/Patch and Put requests.

To learn more about URL structure, navigate to the [documentation](https://docs.microsoft.com/en-us/rest/api/azure/devops/?view=azure-devops-rest-7.1#components-of-a-rest-api-requestresponse-pair).
