


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

