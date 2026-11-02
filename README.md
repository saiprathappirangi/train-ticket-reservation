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

### Tomcat deployment and validation  

```bash
sudo -i                                                                                   # To become a root use
apt update -y                                                                             # Upgrading packages.
apt install default-jdk -y                                                                # Install the default java application
java --version                                                                            # Check java installed
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.123/bin/apache-tomcat-9.0.123.tar.gz   # Install TOMCAT
tar -xf apache-tomcat-9.0.123.tar.gz                                                      # Extract files from a tar archive
mv apache-tomcat-9.0.12 tomcat                                                            # Rename the file
~/tomcat/webapps/host-manager/META-INF#                                                   # Navigate to META-INF Folder
vim context.xml                                                                           # open file called context.xml
<!--
<Valve className="org.apache.catalina.valves.RemoteAddrValve"                             # Comment out or remove this line for both manager and host-manager
       allow="127.0.0./8::1/128"/> -->                                                    # By default, Tomcat restricts Manager access to localhost
<role rolename="manager-gui"/>
<user username="tomcat" password="s3cret" roles="manager-gui"/>                           # Defined a user with the correct roles
<role rolename="manager-script"/>
<user username="prathap123" password="prathap123" roles="manager-script"/>                # Defined a user with the correct roles
<role rolename="admin-gui"/>
<user username="admin" password="admin123" roles="admin-gui"/>
~/tomcat/bin# ./shutdown.sh                                                               # Shutdown tomcat 
~/tomcat/bin# ./shartup.sh                                                                # Start the Tomcat
54.18372.186:8080/TrainBook/                                                              # Search in URL
```















  

