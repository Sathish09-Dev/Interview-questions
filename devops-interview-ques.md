


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
