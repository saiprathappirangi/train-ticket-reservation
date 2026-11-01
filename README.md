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

### 3.1 Environment variables

Configure the following variables separately in each Environment:

  -----------------------------------------------------------------------------
  Variable                Description             Example
  ----------------------- ----------------------- -----------------------------
  `TOMCAT_HOST`           Public or reachable     need to give based on the server IP
                          private IP/DNS name of  
                          that environment's EC2  
                          instance                

  `TOMCAT_USER`           SSH deployment account  `deploy`

  `TOMCAT_WEBAPPS`        Tomcat webapps          `/var/lib/tomcat10/webapps`
                          directory               

  `TOMCAT_SERVICE`        systemd service name    `tomcat10`

  `APP_URL`               Application             `http://host:8080/myapp/`
                          health-check URL        
  -----------------------------------------------------------------------------




  

