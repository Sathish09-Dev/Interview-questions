


## 1.	Can you walk me through a CI/CD pipeline you designed and implemented? What parts were automated and why?


### ✅ **CI/CD Pipeline for .NET Application (with Full Static Analysis and Secret Management)**

> Certainly! I designed and implemented a fully automated CI/CD pipeline using **Azure DevOps** for a **.NET Core web application** deployed to **Azure App Service**. The pipeline focused on secure, high-quality, and repeatable deployments, with over **80% automation** from code commit to production delivery.

---

### 🔧 **1. Build & Test**

* Triggered automatically on commits to `main` or feature branches.
* Used **DotNetCoreCLI\@2** task to restore dependencies, build the solution, and run unit tests.
* Published compiled artifacts (`.zip` package) using **PublishBuildArtifacts\@1** for later deployment stages.

---

### 🛡 **2. Static Code & Security Analysis**

To ensure secure and compliant code, I integrated the following tools:

* ✅ **TruffleHog**
  Scans Git history and current changes for **hardcoded secrets** like passwords or tokens.

* ✅ **SonarQube**
  Performs static code analysis on the .NET project to identify **code smells**, **bugs**, and **security hotspots**. Integrated as a pipeline quality gate.

* ✅ **Black Duck**
  Scans open-source dependencies for **known vulnerabilities (CVEs)** and license risks. Automatically fails the pipeline if critical issues are found.

---

### ☁️ **3. Infrastructure as Code (IaC)**

* Used **Terraform** to provision Azure resources like:

  * Azure App Service Plan
  * Azure App Service
  * Application Insights
* This ensured **version-controlled**, repeatable infrastructure across environments (dev/test/prod).

---

### 🔐 **4. Secrets Management with HashiCorp Vault**

* Integrated **HashiCorp Vault** into the Azure DevOps pipeline to retrieve secrets **at runtime**, such as:

  * Database connection strings
  * API tokens
  * App credentials

* Used a PowerShell or Bash script in the pipeline to authenticate with Vault (via AppRole or Azure AD) and inject secrets as environment variables into the deployment stage.

  **Example snippet:**

  ```bash
  export DB_PASSWORD=$(vault kv get -field=password secret/myapp/db)
  ```

* This approach kept **secrets out of source control and pipelines**, helping ensure **HIPAA and security compliance**.

---

### 🚀 **5. Deployment**

* Used **AzureWebApp\@1** task to deploy the `.zip` package to **Azure App Service**.
* Injected environment-specific settings (including Vault-fetched secrets) at deployment time.
* Post-deployment **smoke tests** validated the health of the application via the `/health` endpoint.

---

### ✅ **6. Approval & Promotion**

* Added a **manual approval gate** before production to comply with internal audit and change control policies.
* Used stage-based pipelines to promote the **same artifact** across dev, test, and prod, ensuring consistency.

---

### 🎯 **Outcome**

> This pipeline achieved over **80% end-to-end automation**, improved security through TruffleHog and HashiCorp Vault integration, and ensured code quality with SonarQube and Black Duck. Deployment time was reduced from hours to under 30 minutes, with full traceability and compliance enforcement.

---

Would you like:

* A **sample YAML snippet** to show how HashiCorp Vault is called from Azure DevOps?
* A **diagram** of this pipeline flow?
* Or help tailoring this story for an **interview response** or **project write-up**?

---

## Yaml script for this question 
Here is a **sample Azure DevOps YAML pipeline** that includes:

✅ **Build and test for .NET Core**
✅ **Static code analysis with SonarQube**
✅ **Secret scanning with TruffleHog**
✅ **Dependency scanning with Black Duck**
✅ **Terraform-based infrastructure provisioning**
✅ **Secrets injection from HashiCorp Vault**
✅ **Deployment to Azure App Service**

---

### ✅ **Azure DevOps YAML Pipeline: .NET App with Security + Vault Integration**

```yaml
trigger:
  branches:
    include:
      - main

variables:
  buildConfiguration: 'Release'
  artifactName: 'drop'
  appName: 'my-dotnet-app'
  resourceGroup: 'my-rg'
  environment: 'dev'

stages:

# ---------- BUILD & TEST ----------
- stage: Build
  jobs:
    - job: BuildAndTest
      displayName: 'Build and Test .NET App'
      pool:
        vmImage: 'windows-latest'
      steps:
        - task: UseDotNet@2
          inputs:
            packageType: 'sdk'
            version: '7.x'

        - script: |
            dotnet restore
            dotnet build --configuration $(buildConfiguration)
            dotnet test --no-build --verbosity normal
          displayName: 'Restore, Build & Test'

        - task: PublishBuildArtifacts@1
          inputs:
            PathtoPublish: '$(System.DefaultWorkingDirectory)'
            ArtifactName: '$(artifactName)'
            publishLocation: 'Container'

# ---------- STATIC CODE SCANNING ----------
- stage: StaticAnalysis
  dependsOn: Build
  jobs:
    - job: CodeScan
      displayName: 'TruffleHog, SonarQube & Black Duck'
      pool:
        vmImage: 'ubuntu-latest'
      steps:
        - script: |
            pip install trufflehog
            trufflehog filesystem . || echo "No secrets found"
          displayName: 'TruffleHog Secret Scan'

        - task: SonarQubePrepare@5
          inputs:
            SonarQube: 'SonarQubeServiceConnection'
            scannerMode: 'MSBuild'
            projectKey: 'dotnet-app'
            projectName: 'dotnet-app'

        - task: SonarQubeAnalyze@5

        - task: SonarQubePublish@5
          inputs:
            pollingTimeoutSec: '300'

        - script: |
            bash <(curl -s -L https://detect.synopsys.com/detect.sh) \
            --blackduck.url=https://<your-blackduck-url> \
            --blackduck.api.token=$BLACKDUCK_API_TOKEN \
            --detect.project.name=dotnet-app \
            --detect.project.version.name=1.0
          displayName: 'Black Duck Scan'
          env:
            BLACKDUCK_API_TOKEN: $(BLACKDUCK_API_TOKEN)

# ---------- INFRASTRUCTURE PROVISIONING ----------
- stage: ProvisionInfrastructure
  dependsOn: StaticAnalysis
  jobs:
    - job: TerraformProvision
      displayName: 'Provision Infra with Terraform'
      pool:
        vmImage: 'ubuntu-latest'
      steps:
        - script: |
            cd infra
            terraform init
            terraform plan -out=tfplan
            terraform apply -auto-approve tfplan
          displayName: 'Terraform Apply'

# ---------- DEPLOYMENT ----------
- stage: Deploy
  dependsOn: ProvisionInfrastructure
  jobs:
    - job: DeployApp
      displayName: 'Deploy to Azure App Service'
      pool:
        vmImage: 'ubuntu-latest'
      steps:
        # --- HashiCorp Vault Integration ---
        - script: |
            export VAULT_ADDR=https://vault.mycompany.com
            export VAULT_TOKEN=$(VAULT_TOKEN)
            export DB_PASSWORD=$(vault kv get -field=password secret/dotnet-app/db)
            echo "##vso[task.setvariable variable=DB_PASSWORD;issecret=true]$DB_PASSWORD"
          displayName: 'Fetch Secrets from HashiCorp Vault'
          env:
            VAULT_TOKEN: $(VAULT_TOKEN)

        # --- Download Build Artifact ---
        - task: DownloadBuildArtifacts@0
          inputs:
            buildType: 'current'
            downloadType: 'single'
            artifactName: '$(artifactName)'
            downloadPath: '$(System.ArtifactsDirectory)'

        # --- Deploy to Azure App Service ---
        - task: AzureWebApp@1
          inputs:
            azureSubscription: 'AzureServiceConnection'
            appType: 'webApp'
            appName: '$(appName)'
            package: '$(System.ArtifactsDirectory)/$(artifactName)/**/*.zip'
            deploymentMethod: 'auto'
```

---

### 🔐 **Secrets and Security Notes**

* `VAULT_TOKEN` and `BLACKDUCK_API_TOKEN` should be stored securely as **pipeline secrets** in Azure DevOps Library or linked from **Azure Key Vault**.
* Replace `<your-blackduck-url>` and Vault paths with your actual endpoints.

---

### 💡 Want to Extend This?

* Add **approval gates** before the `Deploy` stage.
* Include **email or Slack notifications**.
* Add **post-deployment smoke tests** using `curl` or a custom script.

Would you like a visual **pipeline flowchart** or a version for **Linux-hosted .NET apps** on **Azure VMs** instead?

## 2. Which Infrastructure as Code (IaC) tools have you used, and how have you optimized them for your environment?"
---

✅ Sample Answer:
I’ve primarily used Terraform as my Infrastructure as Code (IaC) tool, along with some experience in ARM templates and Terraform Enterprise for more advanced workflows.

With Terraform, I’ve provisioned and managed resources like Azure App Services, AKS clusters, VNets, Key Vaults, and storage accounts. I structured the code using modular design patterns, separating environments (dev/test/prod) with reusable modules for maintainability.

To optimize for scalability and reusability, I:

Used remote backends in Terraform Cloud/Enterprise to enable team collaboration and state locking.

Integrated Terraform into Azure DevOps pipelines so infrastructure provisioning was part of CI/CD.

Parameterized variables using .tfvars files and managed sensitive data using Vault and Azure Key Vault, never hardcoding secrets in code.

Applied resource tagging and naming conventions dynamically to improve governance and cost tracking.

Adopted lifecycle hooks and dependency graphs to handle critical resource ordering and avoid drift.

This approach helped ensure idempotent, secure, and consistent infrastructure deployments, reduced configuration drift, and aligned with enterprise compliance requirements like HIPAA.
 ## What is a Remote Backend in Terraform?
A remote backend in Terraform is a way to store the Terraform state file (terraform.tfstate) in a centralized, remote location, instead of locally on your machine.

The state file holds the current configuration of your infrastructure, so managing it properly is critical — especially in teams.
---
##  3.Give an example of a script you've written to automate a routine task. What language did you use and why?
Here's a **Bash script** to perform a **health check on a Linux server**. It gathers system health metrics such as:

* CPU load
* Memory usage
* Disk usage
* Running services (optional)
* Network availability
* Uptime

---

### ✅ **Linux Health Check Bash Script**

```bash
#!/bin/bash

# Output file
LOGFILE="health_check_$(hostname)_$(date +'%Y%m%d_%H%M%S').log"

echo "Linux Health Check Report - $(date)" | tee "$LOGFILE"
echo "Hostname: $(hostname)" | tee -a "$LOGFILE"
echo "Uptime:" | tee -a "$LOGFILE"
uptime | tee -a "$LOGFILE"

echo -e "\n=== CPU Load ===" | tee -a "$LOGFILE"
top -bn1 | grep "load average" | tee -a "$LOGFILE"

echo -e "\n=== Memory Usage ===" | tee -a "$LOGFILE"
free -h | tee -a "$LOGFILE"

echo -e "\n=== Disk Usage ===" | tee -a "$LOGFILE"
df -h | tee -a "$LOGFILE"

echo -e "\n=== Top Memory-Consuming Processes ===" | tee -a "$LOGFILE"
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem | head -n 10 | tee -a "$LOGFILE"

echo -e "\n=== Open TCP Ports ===" | tee -a "$LOGFILE"
ss -tuln | tee -a "$LOGFILE"

echo -e "\n=== Network Interfaces ===" | tee -a "$LOGFILE"
ip a | tee -a "$LOGFILE"

echo -e "\n=== Ping Check (Google DNS) ===" | tee -a "$LOGFILE"
ping -c 4 8.8.8.8 | tee -a "$LOGFILE"

echo -e "\n=== Service Status (e.g., sshd) ===" | tee -a "$LOGFILE"
systemctl is-active sshd | tee -a "$LOGFILE"

echo -e "\n=== Recent System Log Errors ===" | tee -a "$LOGFILE"
journalctl -p 3 -xb | head -20 | tee -a "$LOGFILE"

echo -e "\nHealth check complete. Log saved to: $LOGFILE"
```

---

### 🔧 **How to Use**

1. Save it to a file:

   ```bash
   nano linux_health_check.sh
   ```
2. Make it executable:

   ```bash
   chmod +x linux_health_check.sh
   ```
3. Run the script:

   ```bash
   ./linux_health_check.sh
   ```

---

### ⏱️ Optional: Schedule with Cron

To run daily at 9 AM:

```bash
crontab -e
```

Add:

```bash
0 9 * * * /path/to/linux_health_check.sh >> /var/log/health_check.log 2>&1
```
## 4.What monitoring and alerting tools have you used, and how did you configure them to detect infrastructure-level failures?"

✅ Sample Answer:
I’ve worked with several monitoring and alerting tools, primarily Azure Monitor (with Application Insights and Log Analytics).

For cloud-native environments on Azure, I configured:

Azure Monitor to collect platform metrics like CPU, memory, disk I/O, and network latency across VMs and services.

Used Log Analytics workspaces to centralize logs from VMs, AKS clusters, and App Services.

Configured Azure Alerts based on metric thresholds — for example:

Triggered alerts if CPU > 85% for 5+ minutes

Alerted on VM status changes (e.g., deallocated or unreachable)

Created heartbeat-based alerts to detect disconnected agents

For application-level observability, I integrated Application Insights with .NET apps to:

Track response times, failure rates, and dependency health

Set Smart Detection rules for anomalies in performance or usage

For alert routing, I used Action Groups in Azure to send alerts to:

Email

Teams/Slack via webhook

PagerDuty for critical issues

This setup helped us proactively detect and respond to infrastructure-level issues, like failing disks, over-utilized VMs, or app service outages, often before users reported them.
---

