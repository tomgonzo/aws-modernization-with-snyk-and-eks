---
title: "Review Pipeline Invocation Output"
chapter: true
weight: 55
---

# Overview
In this section we'll navigate the output from the three scans executed in the Bitbucket Pipeline. Developers and Operations teams that don't want to use the Snyk UI can consume the same results visible from the Bitbucket or ECR integrations directly in the Pipeline execution logs. Each invocation style has its advantages.

## Review Snyk Code SAST Results
Start by reviewing the top-level bitbucket pipeline run

![Pipeline Overview](/images/workshop-bb-pipeline-overview.png)

We also enabled debug for additional visibility.  In normal situations, you would only use debug to troubleshoot an issue, and we include that output for a more complete picture.  If we review some of the output, you will see different types of outputs:

```shell
... LINES DELETED ...

Testing /opt/atlassian/pipelines/agent/build/app/goof ...
 ✗ [Medium] Use of Hardcoded Credentials 
   Path: db.js, line 52 
   Info: Do not hardcode passwords in code. Found hardcoded password used in password.
 ✗ [Medium] Information Exposure 
   Path: app.js, line 26 
   Info: Disable X-Powered-By header for your Express app (consider using Helmet middleware), because it exposes information about the used framework to potential attackers.

... LINES DELETED ...
 ✗ [High] Hardcoded Secret 
   Path: app.js, line 69 
   Info: Avoid hardcoding values that are meant to be secret. Found a hardcoded string used in here.
 ✗ [High] NoSQL Injection 
   Path: routes/index.js, line 39 
   Info: Unsanitized input from the HTTP request body flows into find, where it is used in an NoSQL query. This may result in an NoSQL Injection vulnerability.
 ✗ [High] NoSQL Injection 
   Path: routes/index.js, line 116 
   Info: Unsanitized input from an HTTP parameter flows into findById, where it is used in an NoSQL query. This may result in an NoSQL Injection vulnerability.

... LINES DELETED ...

Summary:
  11 Code issues found
  4 [High]   7 [Medium] 

```

This output identifies vulnerabilities, their line numbers, and severity (Medium and High in the example above).  The output also includes a summary of the number of vulnerabilities, which is useful for those teams that will gate a pipeline based on severity and counts.

The output is also available in JSON format, for those teams that want to perform additional processing with tools such as `jq`.

## Snyk Open Source

Next, let's review the stage named, _Scan open source dependencies._  Here, we utilize a Bitbucket Pipe as an example of an integration with the pipeline tool.  The integration uses Snyk in a container and publishes the results to Bitbucket as a Report artifact as shown below.

![Pipeline Overview](/images/workshop-bb-pipeline-summary.png)

Clicking into the report link shows Snyk results for your pipeline run:

![Pipeline Overview](/images/workshop-bb-pipeline-report.png)


## Snyk Container

The last scan is for dockerfiles.   Snyk container scans your use of base images and packages to identify known vulnerabilities in the open-source containers you and your team use.

The output below shows some of the results in the report from before.  _Note: we took the screenshots from two different runs to help focus on open-source only before._

The results now show the container issues.  The expansion shows additional details, the File reference takes you to the file, and the external report takes you to the publicly available description of the vulnerability.

_Note: at the time of this workshop, the File reference appears broken._

![Pipeline Overview](/images/workshop-bb-pipeline-report-2.png)

This is one of several ways to see the details.  In later sections, we'll show how your team may use the same information in other places for their daily workflow.

