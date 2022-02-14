# Release Notes

One way of getting to know what's new in **azd** is to review the release notes. It is easy to update the version of the library that you're using by just changing the version in your *pom.xml* or any other project relevant files.

## 3.0.2

**Minor incremental release**

- Merged PR: [Issue #13: Change size from int to long #14](https://github.com/hkarthik7/azure-devops-java-sdk/pull/14)
- Extended support for **Git Api**.

## 3.0.1

- Bug fix for executor service in **AzDAsyncApi**.

## 3.0.0

**Breaking changes**

- Removed **ConnectionException** class as it is not used anymore.  
- Extended functionality of **Release Api**.
  - Delete and Update a release
  - Manage release environments
  - Kick off a release pipeline using `queueRelease` method
  - Approve a release
  - Update the manual intervention for a release
- Extended functionality of **Build Api**.
  - Get the `yaml` for a designer build pipeline
  - Update a stage or all stages in the build pipeline
- Change in input parameters for **createSubscription** in **ServiceHooksApi**.
  - **createSubscription** method is tied to type **ServiceHooks** that extracts the parameters from it.
- Added **AzDAsyncApi** that helps to create asynchronous operation across all the methods in the library.

## Version 2.5.9

- Expanded the WorkItemTrackingDetails interface with two updateWorkItem methods and added hyperlinks support. [Associated PR](https://github.com/hkarthik7/azure-devops-java-sdk/pull/10)
- Added support for **Distributed Task Api**

## Version 2.5.8

- Merged PR: [Added support for **policy Api** #8](https://github.com/hkarthik7/azure-devops-java-sdk/pull/8)
- Merged PR: [Added support for **build tags** API #9](https://github.com/hkarthik7/azure-devops-java-sdk/pull/9)

## Version 2.5.7

- Created a helper method in **BuildApi** to easily create/clone a pipeline.
- Added functionality for managing pull request tags/labels.
- Added functionality for managing pull request reviewers. 

## Version 2.5.6

- Merged PR: [Added support to **Service Endpoint Api** #6](https://github.com/hkarthik7/azure-devops-java-sdk/pull/6)
- Added Wiki example
- Added support to **Extension management Api**

## Version 2.5.5

- Introduced **AzDClientApi** to easily create the connection object and call respective API.
- Added support to **OAuth2**.
- Merged PR: [Connection using oauth token #5](https://github.com/hkarthik7/azure-devops-java-sdk/pull/5).
- Create the authorization endpoint, get the access token and refresh the access token using **OAuthApi**.
**Connection** class automatically refreshes the token if it's expired.
- Moved version details to a single class **ApiVersion**.
- Added support to **Accounts** Api.

## Version 2.4.5

- Bug fix for Workitems delete Api.
- Added support to retrieve releases from artifact version id (aka build id).
- Add createPullRequest method in GitApi to optionally create the pull request in draft mode.
- Added support to create and update workitems with additional fields.

## Version 2.4.4

**Minor incremental release**

- Added Pull requests services to GitApi. Retrieve workitems associated to a Pull request.
- Added types for Team and Project services under Core Api.

## Version 2.4.3

**Breaking changes**

- Changed the name of AzDDefaultParameters class to Connection.
- Renamed DefaultParametersException to ConnectionException.
- Moved resource area identifiers to its respective classes.
- Added support for Wiki, Graph and Member Entitlement Management Apis.
- Removed Url class and added buildUrl function within Client class.

## Version 1.4.3

- Bug fix for ServiceHooksApi
- Added Release definition Api services
- Workitem request body type for easily creating workitems.

## Version 1.4.2

- Added support for Service hooks and Release Api.
- Merged PR: [read non-system workitem fields as "other fields" in a map (#4)](https://github.com/hkarthik7/azure-devops-java-sdk/pull/4)
- Removed stale properties from WorkitemFields

## Version 1.3.1

- Changes to name of the main Apis.
- Added types & definitions.

## Version 1.2.1

**Major changes**

- for exception handling
- Decoupled classes by introducing interfaces. This helps to indicate what we support.
- Added tests.
- Added WorkApi and WorkItems Api.

## Version 0.2.1

- Changes to exception handling
- Added functions to manage pull requests in Git API call
- Bug fix in setting environment variables

## Version 0.1.0

SDK initial release.
