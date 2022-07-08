# WorkItem Tracking

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/wit/?view=azure-devops-rest-6.1)
- API Version: 6.1

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `WorkItemTrackingApi`.

```java
var wit = webApi.getWorkItemTrackingApi();
```

We have access to the functionalities that **WorkItem Tracking** Api has.

### Available functions

#### Create a work item

Create a work item quickly. There are overloaded methods available to help you create the work item(s) on the fly or with additional details you want.

```java
// Create a work item only with title
wit.createWorkItem("user story", WorkItemOperation.ADD, "My first user story");

// Create a work item with description and add tags
wit.createWorkItem("user story", WorkItemOperation.ADD, "My first user story", 
    "This is my first user story, created from azd library.", new String[] {"DevOps", "azd", "java" });

// Create a work item with additional fields. Note that when you create a work item with additional fields
// you have to pass the internal field names. For instance, the internal name for title is System.Title and so on..

var additionalFields = new HashMap<String, Object>() {{
    put("System.Tags", String.join(",", "DevOps", "Java", "azd"));
}};

wit.createWorkItem("user story", "My first user story", 
    "This is my first user story, created from azd library.", additionalFields);
```

#### Delete a work item

Delete a work item by id.

```java
wit.deleteWorkItem(10);
```

#### Delete a work item permanently

Permanently delete a work item by id.

```java
wit.deleteWorkItem(10, true);
```

!!! warning

    If the destroy parameter is set to true, work items deleted by this command will
    **NOT** go to recycle-bin and there is no way to restore/recover them after deletion.

#### Get a work item

Get a work item by id. This returns a `WorkItem` object and you can use it to identify the internal field names to use it in *createWorkItem* method.

```java
wit.getWorkItem(15);

// Get a work item and optionally expand the field
wit.getWorkItem(15, WorkItemExpand.ALL);
```

#### Get a list of work item

Get a list of work item with array of id.

```java
wit.getWorkItems(new int[] { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 });
```

#### Get a work item's revisions

Get a work item's revisions with work item id.

```java
wit.getWorkItemRevisions(23);
```

#### Query work items

Get the results of work items by passing the team name and work item query.

```java
String query = "Select * From WorkItems Where [System.WorkItemType] = 'User Story'";
wit.queryByWiql("myTeam", query);
```

#### Remove work item from recycle bin

Remove a work item from recycle bin.

```java
wit.removeWorkItemFromRecycleBin(12);
```

#### Get a work item from recycle bin

Get a work item from recycle bin by id.

```java
wit.getWorkItemFromRecycleBin(22);
```

#### Get deleted work items from recycle bin

Get all deleted work items from recycle bin.

```java
wit.getDeletedWorkItemsFromRecycleBin();
```

#### Restore work item from recycle bin

Restore a work item from recycle bin by id.

```java
wit.restoreWorkItemFromRecycleBin(22);
```

#### Update a work item

Update a work item fields. The below example updates the `Assigned To` section of the work item. Note that we need to use the internal field name to update it.

```java
var fieldsToUpdate = new HashMap<String, Object>() {{ put("System.AssignedTo", "test@gmail.com"); }};
wit.updateWorkItem(7, fieldsToUpdate);
```

#### Add hyper links to the work item

Add hyper links to the work item by id.

```java
Map<String, String> hyperlinksMap = new HashMap<>();

hyperlinksMap.put("https://docs.microsoft.com/en-us/rest/api/azure/devops",
        "This is a hyperlink that points to the Azure DevOps REST documentation.");

wit.addHyperLinks(17, hyperlinksMap);
```

#### Remove hyper links from the work item

Remove a list of hyper links from the work item.

```java
List<String> hyperlinks = new ArrayList<>();

hyperlinks.add("https://docs.microsoft.com/en-us/rest/api/azure/devops");

wit.removeHyperLinks(17, hyperlinks);
```

#### Get all work item types

Get all applicable work item types.

```java
wit.getWorkItemTypes();
```

#### Get a work item type

Get a work item type by name.

```java
wit.getWorkItemType("bug");
```

#### Add an attachment to a work item

To add an attachment to a work item you should create an attachment first. This requires you to pass file name, attachment upload type, project team name, attachment contents and work item id.

```java
// Create an attachment
var attachment = wit.createAttachment("testFile.txt", AttachmentUploadType.SIMPLE, "my-team", "Sample content");
var attachmentFields = new HashMap<String, String>(){{ put(attachment.getUrl(), "Test File url."); }};

// add the attachment to work item.
wit.addWorkItemAttachment(133, attachmentFields);
```

#### Remove an attachment from a work item

To remove an attachment you should provide the attachment url, which you can get from the **Relations** section of [WorkItem](https://hkarthik7.github.io/azd-docs/org/azd/workitemtracking/types/WorkItem.html) object.

```java
// Remove an attachment from the work item.
var relations = wit.getWorkItem(133, WorkItemExpand.RELATIONS).getRelations();
String fileNameToRemove = "testFile.txt";
List<String> attachmentUrl = new ArrayList<>();

for (var relation: relations) {
    if (relation.getAttributes().getName().equals(fileNameToRemove)) {
        attachmentUrl.add(relation.getUrl());
        wit.removeWorkItemAttachment(133, attachmentUrl);
    }
}
```
