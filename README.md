# Multi‑Environment CI/CD Deployment for TOMCAT

## Workflow Summary

- **Trigger:** Runs automatically on every push to the `master` branch.  
- **Build Stage:**
  - Checkout source code.  
  - Set up **Java 17 (Temurin distribution)**.  
  - Run Maven lifecycle: `validate`, `compile`, `test`, `package`.  
  - Upload the generated WAR file as an artifact.  
- **Deployment Stages:**
  - Each environment downloads the artifact.  
  - Deploys to Tomcat via **Manager API** using `curl`.  
  - Credentials and host details are securely stored in **GitHub Secrets**.  
  - Higher environments (PrePROD, PROD) require manual approval via **GitHub → Settings → Environments**.

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

## EC2 Configuration

### AMI
- **Ubuntu 22.04 LTS**

### Instance Type
- **t2.micro** (for testing and lightweight workloads)

### Security Group
Open the following ports:
- **22 → SSH** (remote access)
- **8080 → Tomcat** (web application access)

### Connect EC2 instance and bash the following commands 

```bash





comment out or remove this line
By default, Tomcat restricts Manager access to localhost
defined a user with the correct role:












  

