---
title: "Invoke Snyk in Bitbucket Pipelines"
chapter: true
weight: 54
---

# Overview
These same scans can be added to Bitbucket Pipelines by making a few changes to the `bitbucket-pipelines.yml` pipeline definition. You may make the changes directly in the Bitbucket Cloud UI, or from Cloud9 with a `git push`.

### Step 1 - Add Snyk scanning of Source Code
To enable SAST Code Analysis in the pipeline, find the section below and uncomment as directed to enable the scan.

```yaml
test-app: &test-app
  - step:
      name: "Test application"
      caches:
        - node
      script:
        - cd app/goof
        - npm install
        # Uncomment the following lines to enable Snyk Scan of code
        # - curl https://static.snyk.io/cli/latest/snyk-linux -o snyk-linux
        # - chmod +x snyk-linux
        # - ./snyk-linux -d code test || true
        # - echo $EXIT_CODE
```

This is an example of a CLI-based invocation of Snyk. In this section entitled _Test Application_, we download the Snyk CLI via curl and run `snyk code test`. As Snyk will break the build any time issues are found, rather than break the build, we pipe to a boolean `true` to allow the pipeline to continue. 

### Step 2 - Add Snyk scanning of open source
To enable SCA for Application Dependencies, uncomment the `scan-app` stage in the pipeline. Here, we utilize a Bitbucket [Pipe](https://bitbucket.org/product/features/pipelines/integrations) to invoke the scan.  

```yaml
pipelines:
  default:
    - <<: *test-app
    # Uncomment this line to enable the scanning of the application.
    # - <<: *scan-app
    - <<: *scan-push-image
```

The `Pipe` uses a containerized Snyk CLI and supports publishing the results to Bitbucket as a Report artifact. This type of information is handy for teams that want to store Snyk results as evidence for each pipeline run.

### Step 3 - Add Snyk scanning of open source

Snyk's Bitbucket Pipe also supports SCA for Container Images. Uncommenting the section below uses the Snyk Pipe to scan the Goof Container Image after it's built.

```yaml
scan-push-image: &scan-push-image
  - step:
      name: "Scan and push container image"
      services:
        - docker
      script:
        - docker build -t $IMAGE ./app/goof/
        - docker tag $IMAGE $IMAGE:${BITBUCKET_COMMIT}
        - echo "Scan Container images"
      # Uncomment this block to enable Snyk Scan of container images.
      #  - pipe: snyk/snyk-scan:0.5.3
      #    variables:
      #      SNYK_TOKEN: $SNYK_TOKEN
      #      LANGUAGE: "docker"
      #      IMAGE_NAME: $IMAGE
      #      PROJECT_FOLDER: "app/goof"
      #      TARGET_FILE: "Dockerfile"
      #      CODE_INSIGHTS_RESULTS: "true"
      #      SEVERITY_THRESHOLD: "high"
      #      DONT_BREAK_BUILD: "true"
      #      MONITOR: "false"
...
```

## Step 4: Commit and build

Commit your changes to trigger a pipeline build.  In the next section we'll review the results.
