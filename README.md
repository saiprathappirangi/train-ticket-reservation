# Multi‑Environment CI/CD Deployment for TOMCAT

## 1. Continuous Integration and Continuous Deployment Workflow

This repository implements a GitHub Actions pipeline that builds a Maven‑based WAR file once and 
promotes the same artifact through four environments:

-   **DEV** -- automatic deployment after a push to the configured
    deployment branch.
-   **TEST** -- deployment starts after DEV succeeds and requires
    approval.
-   **PREPROD** -- deployment starts after TEST succeeds and requires
    approval.
-   **PROD** -- deployment starts after PREPROD succeeds and requires
    approval.
  Each environment runs on its own Tomcat EC2 instance with separate secrets and variables configured in GitHub Environments.

## 2. Architecture



## 3. Environment configuration

**Repository → Settings → Environments**

  GitHub Environment   Purpose                     Approval
  -------------------- --------------------------- ----------------------
  `dev`                Development deployment      No required reviewer
  `test`               Test deployment             Required reviewers
  `preprod`            Pre-production validation   Required reviewers
  `prod`               Production deployment       Required reviewers

### 3.1 Environment Secrets

Configure the following variables separately in each Environment:

| Environment | Secret Name             | Purpose                          | Example Value                |
|-------------|-------------------------|----------------------------------|------------------------------|
| **DEV**     | `DEV_TOMCAT_USER`       | Tomcat Manager username          | `admin`                      |
|             | `DEV_TOMCAT_PASSWORD`   | Tomcat Manager password          | `prathap123`                 |
|             | `DEV_TOMCAT_HOST`       | EC2 host IP/DNS                  | `13.57.14.61`                |
| **QA**      | `QA_TOMCAT_USER`        | Tomcat Manager username          | `admin`                      |
|             | `QA_TOMCAT_PASSWORD`    | Tomcat Manager password          | `prathap123`                 |
|             | `QA_TOMCAT_HOST`        | EC2 host IP/DNS                  | `54.67.119.218`              |
| **PREPROD** | `PREPROD_TOMCAT_USER`   | Tomcat Manager username          | `admin`                      |
|             | `PREPROD_TOMCAT_PASSWORD` | Tomcat Manager password        | `prathap123`                 |
|             | `PREPROD_TOMCAT_HOST`   | EC2 host IP/DNS                  | `13.57.14.61`                |
| **PROD**    | `PROD_TOMCAT_USER`      | Tomcat Manager username          | `admin`                      |
|             | `PROD_TOMCAT_PASSWORD`  | Tomcat Manager password          | `prathap123`                 |
|             | `PROD_TOMCAT_HOST`      | EC2 host IP/DNS                  | `54.183.72.186`              |

## 4 EC2 and AMI Configuration
### 4.1 EC2 & Tomcat Install for DEV environment:
### 4.1.1 Launch EC2 Instance
      AMI: Ubuntu 22.04 LTS
      Instance type: t2.micro (for testing)
      Security group: open ports
                      22 → SSH
                      8080 → Tomcat












  

