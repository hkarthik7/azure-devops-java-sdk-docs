# Pipelines

- [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/pipelines/?view=azure-devops-rest-6.1)
- API Version: 7.1

## Prerequisites

See [QuickStart](quickstart.md)

## Examples

Now that we have the connection object `webApi`, we can get the instance of `PipelinesApi`.

```java
var pipelines = webApi.getPipelinesApi();
```

We have access to the functionalities that **Pipelines** Api has.

### Available functions

#### Get pipeline artifacts

Get the pipeline artifacts.

```java
pipelines.getArtifacts(22, 54, "drop");

// optionally get the artifacts section expanded
// This returns the PipelinesArtifact object and you can get the signed content URL from where you can
// download the artifacts
pipelines.getArtifacts(22, 54, "drop", PipelinesExpandOptions.SIGNEDCONTENT);

String url = pipelines
    .getArtifacts(23, 55, "drop", PipelinesExpandOptions.SIGNEDCONTENT)
    .getSignedContent()
    .getUrl();

try {
    var fileStream = new FileOutputStream("drop.zip");
    fileStream
        .getChannel()
        .transferFrom(
        Channels.newChannel(
        new URL(url).openStream()), 0, Long.MAX_VALUE);
    
    fileStream.close();
} catch (IOException e) {
    e.printStackTrace();
}
```

#### Get Pipeline logs

Get the logs from pipeline.

```java
// Pass the build definition id aka pipeline id and build id aka run id.
pipelines.getPipelineLogs(24, 513);
```

#### Create a pipeline

Create a pipeline from the existing *YAML* file in the repository. You should've already created and hosted the *YAML* configuration file in the repository to use
this functionality.

```java
String repo = g.getRepository("myRepo");
pipelines.createPipeline("Deploy-WebApp-CI", "/", "/azure-pipelines.yaml", repo.getId(), repo.getName());
```

#### Get a pipeline using pipeline id

Get a pipeline object using pipeline id. The pipeline id is the build definition id.

```java
pipelines.getPipeline(22);
```

#### Get a list of all pipelines

Get a list of all available build pipelines.

```java
pipelines.getPipelines();
```

#### Test a pipeline

Run a preview of the pipeline and get the *YAML* of pipeline as a result.

```java
pipelines.previewPipeline(24, true);
```

#### Get a pipeline run

Get the pipeline run or a build using pipeline id and run id.

```java
pipelines.getPipelineRun(22, 234);
```

#### Get pipeline runs

Get all the build of a pipeline.

```java
pipelines.getPipelineRuns(22);
```

#### Run a pipeline

Run a pipeline using it's id.

```java
pipelines.runPipeline(22);
```