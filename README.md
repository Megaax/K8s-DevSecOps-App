# Kubernetes DevSecOps CICD Project Using Github Actions and ArgoCD

## Video Tutorial

For a comprehensive guide on deploying and managing cloud-native applications using AWS, Kubernetes, and DevSecOps tools, watch the detailed tutorial:

[![Master Three-Tier Application | A Complete DevSecOps Guide on AWS with Kubernetes, GitOps & ArgoCD](http://img.youtube.com/vi/EVG51U3VcYs/0.jpg)](https://www.youtube.com/watch?v=EVG51U3VcYs "Mastering Cloud-Native Applications: A Complete DevSecOps Guide on AWS with Kubernetes")

Click on the image above to watch the video.

![gif2](https://github.com/cloudcore-hub/reactjs-quiz-app/assets/88560609/a0dfce93-3bde-45af-b82a-d7c9e2c47294)
# Kubernetes DevSecOps CI/CD Project

This repository implements a DevSecOps CI/CD pipeline for a Kubernetes-based application using GitHub Actions and ArgoCD. The project automates building, testing, securing, and deploying both frontend and backend applications, ensuring adherence to security best practices throughout the CI/CD process.

## Overview

The pipeline described in this project leverages multiple tools and technologies to achieve continuous integration and continuous deployment with a strong emphasis on security (DevSecOps). Key components of the pipeline include:

- **GitHub Actions**: Automates the CI/CD workflow, including building, testing, and deploying the application.
- **ArgoCD**: Facilitates continuous deployment using a GitOps approach, ensuring that the application is deployed consistently and reliably to the Kubernetes cluster.
- **SonarCloud**: Performs static code analysis to identify bugs, code smells, and security vulnerabilities.
- **Snyk**: Conducts security scans on both the application code and Docker images to detect vulnerabilities.
- **Trivy**: Scans Docker images for vulnerabilities before deployment.

## Pipeline Details

The pipeline consists of several jobs, each responsible for different aspects of the CI/CD process:

### 1. React.js CI

Triggered on a push to the `main` branch, this pipeline includes the following jobs:

#### Frontend Test (`frontend-test`)

- **Environment**: Ubuntu-latest with Node.js 20.x
- **Steps**:
  - Checkout the repository.
  - Setup Node.js.
  - Install dependencies, run linter, Prettier, and tests.
  - Build the frontend application.
  - Setup and run SonarQube analysis.

#### Backend Test (`backend-test`)

- **Environment**: Ubuntu-latest with Node.js 20.x
- **Steps**:
  - Checkout the repository.
  - Setup Node.js.
  - Install dependencies, run linter, Prettier, and tests.
  - Setup and run SonarQube analysis.

#### Frontend Security (`frontend-security`)

- **Prerequisite**: Needs `frontend-test` to complete successfully.
- **Steps**:
  - Checkout the repository.
  - Run Snyk to check for vulnerabilities.
  - Install and authenticate Snyk.
  - Run Snyk code test.

#### Backend Security (`backend-security`)

- **Prerequisite**: Needs `backend-test` to complete successfully.
- **Steps**:
  - Checkout the repository.
  - Run Snyk to check for vulnerabilities.
  - Install and authenticate Snyk.
  - Run Snyk code test.

#### Frontend Image (`frontend-image`)

- **Prerequisite**: Needs `frontend-security` to complete successfully.
- **Steps**:
  - Checkout the repository.
  - Build and push Docker image to DockerHub.
  - Run Trivy to scan the Docker image.
  - Install and authenticate Snyk.
  - Monitor Docker image with Snyk.
  - Run Snyk to check for vulnerabilities in the Docker image.

#### Backend Image (`backend-image`)

- **Prerequisite**: Needs `backend-security` to complete successfully.
- **Steps**:
  - Checkout the repository.
  - Build and push Docker image to DockerHub.
  - Run Trivy to scan the Docker image.
  - Install and authenticate Snyk.
  - Monitor Docker image with Snyk.
  - Run Snyk to check for vulnerabilities in the Docker image.

#### Kubernetes Manifest Scan (`k8s-manifest-scan`)

- **Prerequisite**: Needs `backend-security` and `frontend-security` to complete successfully.
- **Steps**:
  - Checkout the repository.
  - Run Snyk to check Kubernetes manifest files for issues.

## Prerequisites

- **Kubernetes Cluster**: A running Kubernetes cluster.
- **GitHub Account**: For repository and Actions.
- **ArgoCD**: Installed and configured on Kubernetes.
- **Helm**: For managing Kubernetes applications.
- **kubectl**: Command-line tool for Kubernetes.
- **Docker**: For building container images.