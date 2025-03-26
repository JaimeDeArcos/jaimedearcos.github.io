---
date: 2024-06-15T11:00:00.000Z
layout: post
title: Automating Java Builds with Maven and GitHub Actions CI/CD
subtitle: A Step-by-Step Guide to Continuous Integration and Deployment
description: Learn how to set up automated Java build pipelines using Maven and GitHub Actions, including practical examples and detailed instructions.
image: /assets/img/uploads/java-maven-github-actions.png
optimized_image: /assets/img/uploads/java-maven-github-actions.png
category: code
tags:
  - code
  - java
  - maven
  - github-actions
  - cicd
author: jaimedearcos
paginate: false
---

> Continuous Integration and Continuous Deployment (CI/CD) streamline development workflows, improve software quality, and accelerate delivery. This tutorial guides you through setting up automated Java builds using **Maven** and **GitHub Actions**.

## Prerequisites
- A Java project with Maven configured (`pom.xml`).
- GitHub repository.

## Step 1: Setting Up Your GitHub Actions Workflow

In your repository, create the following directory structure:

```
.github/workflows/
```

Create a new file named `maven-ci.yml` within this directory:

```yml
name: Maven CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
      - name: Set up JDK 21
        uses: actions/setup-java@v4
        with:
          java-version: '21'
          distribution: 'temurin'

      - name: Build with Maven
        run: mvn clean install
```

## Step 2: Understanding the Workflow

- **Trigger:** Activated on push or pull request to the `main` branch.
- **Checkout:** Retrieves the source code from your repository.
- **JDK Setup:** Installs Java 21 from Temurin.
- **Maven Build:** Executes Maven goals (`clean install`) to compile, test, and package your Java application.

## Step 3: Add Maven Test Reports

Enhance your workflow by generating and uploading test reports:

```yml
- name: Run tests and upload reports
  run: |
    mvn test
  continue-on-error: true

- name: Upload test report
  uses: actions/upload-artifact@v4
  with:
    name: test-report
    path: target/surefire-reports/
```

## Step 4: Deploy Artifacts (Optional)

To automatically deploy artifacts, include the deployment steps:

```yml
- name: Deploy package
  env:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
  run: mvn deploy
```

Ensure your `pom.xml` is configured for deployment (e.g., GitHub Packages).

## Step 5: Committing and Running Your Workflow

Commit your workflow file and push the changes:

```bash
git add .github/workflows/maven-ci.yml
git commit -m "Add Maven CI GitHub Actions workflow"
git push
```

Navigate to the **Actions** tab on your GitHub repository to view and monitor your pipeline.

## Advantages of Automating Java Builds
- Rapid feedback on code integration.
- Early identification of bugs and issues.
- Automated testing and deployment.
- Improved reliability and consistency.

## Pricing

GitHub Actions offers a generous free tier suitable for most projects:

- **Free Tier:**
    - 2,000 minutes per month (private repositories).
    - Unlimited minutes for public repositories.
    - Free usage of GitHub-hosted runners for most common tasks.

- **Additional Minutes:**
    - $0.008 per minute for Linux runners.
    - $0.016 per minute for Windows runners.
    - $0.08 per minute for macOS runners.

For detailed and up-to-date pricing, visit [GitHub Actions Pricing](https://github.com/pricing#actions).

## Conclusion

Using GitHub Actions with Maven provides an efficient and robust CI/CD pipeline, significantly improving your development lifecycle. Start implementing these practices today and streamline your Java application delivery.

# Further Reading
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Maven Official Documentation](https://maven.apache.org/guides/)
- [Publishing Java Packages with GitHub Packages](https://docs.github.com/en/packages/working-with-a-github-packages-registry)