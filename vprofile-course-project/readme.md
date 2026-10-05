# VProfile App

## Project overview

This repository contains a Java Spring MVC application built as a Maven WAR project for a VProfile-style web application. It includes Spring MVC, Spring Security, JPA, RabbitMQ, Elasticsearch, Memcached, and a Java web frontend packaged for deployment in Tomcat.

The project is designed to support a modern GitOps flow with:

- pull request validation on `main`
- self-hosted SonarQube scanning and quality gate enforcement
- Docker image build and push to Amazon ECR on merge to `main`
- Helm values updates through a pull request in the GitOps repository before merge

## Tech stack

- Java: 21 in CI/runtime pipeline
- Maven: build and dependency management
- Packaging: WAR
- Spring Framework / Spring MVC / Spring Security / Spring Data JPA
- Tomcat 10 runtime
- Docker multi-stage build
- Amazon ECR for container registry
- Helm for deployment values management
- SonarQube self-hosted instance on EC2

## CI/CD workflow design

The intended GitHub Actions flow is:

1. Feature branch push
   - no workflow run

2. Pull request to `main`
   - Maven build
   - unit tests
   - Checkstyle validation
   - SonarQube scan
   - SonarQube quality gate
   - merge blocked if the quality gate fails

3. Push to `main`
   - build Docker image
   - push to Amazon ECR with commit SHA and `latest` tags
   - create a branch in the GitOps repository with the new image tag
   - open a pull request for review before the Helm values are merged

## GitOps approval gate

This project uses a protected Helm promotion flow.

The required behavior is:

- a merge to `main` in the app repo triggers the Docker build and ECR push
- the GitOps repo is updated on a separate branch with the new image tag
- a pull request is created automatically for the Helm change
- the PR must be reviewed and approved before `helm/vprofile/values.yaml` is merged

This prevents direct deployment configuration changes without human verification.

### Why this matters

Without this step, the deployment values file can be modified immediately after a merge, which means production config changes happen without review.

The review gate ensures:

- source changes are validated before deployment
- image builds are completed successfully
- deployment values are reviewed as a change request
- Helm updates only happen after approval

## Deployment pipeline summary

### PR validation flow
- Maven build
- unit tests
- Checkstyle
- SonarQube scan
- quality gate validation
- block merge when Sonar fails the gate

### Main branch deployment flow
1. build Docker image from `Docker-files/app/multistage/Dockerfile`
2. push image to Amazon ECR in `us-east-1`
3. tag with the commit SHA and `latest`
4. create a GitOps branch and update the image tag
5. open a pull request for the Helm values change
6. merge only after approval

## Required GitHub configuration

### Secrets
- `SONAR_TOKEN`
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `HELM_REPO_USER`
- `GITOPS_PAT`

### Variables
- `AWS_REGION`
- `ECR_REPOSITORY`
- `HELM_REPO_NAME`
- `SONAR_HOST_URL`

## Example GitOps flow

- App repo PR is approved and merged
- CI/CD builds the Docker image and pushes it to ECR
- GitOps repo gets a branch such as `update-image-<sha>`
- GitHub creates a pull request automatically
- a reviewer checks the `app.image` and `app.tag` values
- after approval, the PR is merged into the GitOps repo `main` branch

This is the required verification step before the deployment values are updated.

## Final note

This repository is a Spring MVC application targeted for automated validation and GitOps deployment. The CI system is designed to enforce both quality gates and deployment review before updating Helm values.