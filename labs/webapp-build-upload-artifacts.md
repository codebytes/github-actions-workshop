# WebApp Build and Upload Artifacts

## Introduction

In this lab, you will learn how to build a web application using GitHub Actions. You will create a workflow that builds the application and uploads the artifacts. You will also learn multiple GitHub built-int actions like `actions/setup-dotnet` and `actions/upload-artifact`.

> Duration: 15-20 minutes

## Prerequisites

Create a new Web App by following the instructions in the [Create a Web App](./create-webapp.md) lab.

## Instructions

1. Open Visual Studio Code and open the project directory.

2. Open the existing directory `.github/workflows` in the root of the project.

3. Create a new file named `webapp-build.yml` and provide workflow name and event trigger information as shown below.

   ```yaml
   name: WebApp Build
   on:
     push:
       paths:
         - '.github/workflows/webapp-build.yml'
         - 'src/dotnet/WebApp/**'
     workflow_dispatch:
   ```

4. Add a job to build the .NET Core application. Note that we are using the `actions/setup-dotnet` action to set up the .NET Core SDK.

   ```yaml
   jobs:
     build:
       runs-on: ubuntu-latest
       steps:
         - name: checkout code
           uses: actions/checkout@v4.1.7

         - name: Set up .NET Core
           uses: actions/setup-dotnet@v4.0.1
           with:
           dotnet-version: '8.x'

         - name: Build code
           run: dotnet build --configuration Release

         - name: Publish code
           run: dotnet publish -c Release --property:PublishDir="${{runner.temp}}/webapp"
   ```

5. Add a new step to upload the artifacts.

   ```yaml
   - name: Upload artifacts
     uses: actions/upload-artifact@v2
     with:
       name: webapp-artifacts
   ```

6. Commit the changes and push them to the repository.

7. Navigate to the "Actions" tab in your repository to view the workflow runs.

8. Click on the latest workflow run to view the details.

9. Click on the job to view the logs.

10. Verify that the workflow has built and published the application successfully.

## Lab Solution

The complete solution is provided below.

```yaml
name: WebApp Build - Upload Artifacts

on:
  push:
    paths:
      - '.github/workflows/webapp-build-upload-artifacts.yml'
      - 'src/dotnet/WebApp/**'
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: ./src/dotnet/WebApp
    steps:
      - name: checkout code
        uses: actions/checkout@v4.1.7

      - name: Set up .NET Core
        uses: actions/setup-dotnet@v4.0.1
        with:
          dotnet-version: '8.x'

      - name: Build code
        run: dotnet build --configuration Release

      - name: Publish code
        run: dotnet publish -c Release --property:PublishDir="${{runner.temp}}/webapp"

      - name: Upload Artifact
        uses: actions/upload-artifact@v4.3.6
        with:
          name: .net-web-app # Artifact name
          path: ${{runner.temp}}/webapp
```

## Summary

In this lab, you created a GitHub Actions workflow to build a web application and upload the artifacts. You learned how to use the `actions/setup-dotnet` and `actions/upload-artifact` actions to build and upload the artifacts.
