## 1. Walk through the pipeline that you have designed and what are type of application that have dealt with?
In a DevOps pipeline, once code is committed, several automated steps typically follow to build, test, and deploy the application. Here's a step-by-step breakdown of what usually happens after a code commit:

✅ 1. Code Commit (triggering point)
Developer commits code to a Git repository (like GitHub, GitLab, Azure Repos, Bitbucket).

This triggers a CI/CD pipeline.

⚙️ 2. Continuous Integration (CI) Pipeline
🧪 a. Code Checkout
The pipeline pulls the latest code from the repository.

🧱 b. Build Stage
Compiles the code (e.g., dotnet build, npm run build).

Restores dependencies (e.g., dotnet restore, npm install).

✅ c. Run Unit Tests
Executes automated unit tests.

Tools: xUnit, NUnit (.NET), Jest (JavaScript), etc.

🧼 d. Code Analysis / Linting
Static code analysis (e.g., SonarQube, ESLint) to detect bugs or bad practices.

🚦 3. Continuous Delivery (CD) Pipeline
🧪 a. Integration Testing / API Testing
Runs Postman, RestAssured, or Selenium for API/UI testing.

Verifies the app works as expected after integration.

📦 b. Create Artifact
Packages the application (e.g., zip, .dll, Docker image).

Tools: Azure Artifacts, JFrog Artifactory, GitHub Packages.

☁️ c. Push to Artifact Repository (optional)
Store the build output in a centralized location.

🚀 4. Deployment
🧩 a. Deploy to Environment (Dev / QA / Staging / Prod)
Tools: Azure DevOps Release Pipelines, GitHub Actions, Jenkins, GitLab CI/CD, Octopus Deploy.

📥 b. Infrastructure as Code (Optional)
Use Terraform, Bicep, or ARM templates to provision cloud infra.

🔍 c. Smoke Tests / Health Checks
Post-deployment verification to ensure the app is up.

📊 5. Monitoring & Feedback
Use tools like:

Application Monitoring: Azure Application Insights, Datadog, New Relic

Logging: ELK Stack, Splunk, Azure Log Analytics

Alerts set for performance or error thresholds.

💡 Example (Azure DevOps):
plaintext
Copy
Edit
Commit Code → Trigger Pipeline → Restore → Build → Test → Code Analysis → Package → Publish Artifact → Deploy to Azure App Service → Run Smoke Tests → Monitor Logs
If you'd like, I can generate a DevOps flow diagram or help you write a pipeline (e.g., Azure DevOps YAML or GitHub Actions) based on your stack (e.g., .NET Core + Angular).

## 2. What are the branching strategies that you have used in your organization?

✅ Common Git Branching Structures in DevOps Projects
🔹 1. Git Flow (Classic Enterprise Model)
Best for large teams, longer release cycles.

bash
Copy
Edit
main
│
├── develop
│   ├── feature/abc
│   └── feature/xyz
│
├── release/1.0
│
└── hotfix/1.0.1
Branches:

main: Always production-ready.

develop: Integration branch for features.

feature/*: For developing features.

release/*: Prepares for a release.

hotfix/*: Fixes production bugs.

2. Trunk-Based Development
Ideal for microservices, CI/CD.

bash
Copy
Edit
main (or trunk)
└── short-lived branches → merged multiple times/day
No long-lived feature branches.

Feature flags often used to hide unfinished work.

📌 Best Practices
✅ Use meaningful branch names: feature/login-api, bugfix/crash-on-submit

✅ Always merge using PRs

✅ Automate tests and builds for each PR

❌ Don’t commit directly to main unless hotfix with approval

✅ Use tags for releases: v1.0.0, v2.1.3

## 3. What is difference between Git rebase and Git merge?

🔁 git merge vs git rebase
Feature	git merge	git rebase
Purpose	Combines branches with a merge commit	Moves branch commits onto a new base
History	Keeps original history	Rewrites commit history
Merge commit	Yes (Merge branch 'feature/xyz')	No merge commit; creates linear history
Safe for shared branches	✅ Yes	⚠️ Only use rebase on local/unshared branches
Simpler to understand	✅ Yes	❌ Can be confusing for beginners
Use case	Collaboration, preserving context	Clean history before merging to main

📌 1. Merge: Keeps History
bash
Copy
Edit
git checkout main
git merge feature/login
Result:

plaintext
Copy
Edit
A---B---C (main)
     \
      D---E (feature/login)
           \
            M (merge commit)
✅ Keeps the commit tree as-is
❌ Can become messy with many branches

📌 2. Rebase: Clean History
bash
Copy
Edit
git checkout feature/login
git rebase main
Result:

plaintext
Copy
Edit
A---B---C (main)
             \
              D'---E' (rebased feature/login)
✅ Linear, clean history
❌ Rewrites history (dangerous if already pushed and shared)

🔁 Final Merge After Rebase (optional)
If you rebased and now want to merge cleanly:

bash
Copy
Edit
git checkout main
git merge feature/login  # Fast-forward merge
No extra merge commit is created — just a straight line.

🛑 When to Use What?
Scenario	Use merge	Use rebase
Working in a team	✅ Yes	⚠️ Only before pushing
Cleaning history before final merge	❌ No	✅ Yes
You need a full history with all merges	✅ Yes	❌ No
Want a linear, simple commit history	❌ No	✅ Yes

🔐 Safe Rule:
🔸 Use merge for collaboration.
🔸 Use rebase only for local branches before pushing.

## 4. If pipelines fails in production deployment, how you gonna rollback?

🔁 git merge vs git rebase
Feature	git merge	git rebase
Purpose	Combines branches with a merge commit	Moves branch commits onto a new base
History	Keeps original history	Rewrites commit history
Merge commit	Yes (Merge branch 'feature/xyz')	No merge commit; creates linear history
Safe for shared branches	✅ Yes	⚠️ Only use rebase on local/unshared branches
Simpler to understand	✅ Yes	❌ Can be confusing for beginners
Use case	Collaboration, preserving context	Clean history before merging to main

📌 1. Merge: Keeps History
bash
Copy
Edit
git checkout main
git merge feature/login
Result:

plaintext
Copy
Edit
A---B---C (main)
     \
      D---E (feature/login)
           \
            M (merge commit)
✅ Keeps the commit tree as-is
❌ Can become messy with many branches

📌 2. Rebase: Clean History
bash
Copy
Edit
git checkout feature/login
git rebase main
Result:

plaintext
Copy
Edit
A---B---C (main)
             \
              D'---E' (rebased feature/login)
✅ Linear, clean history
❌ Rewrites history (dangerous if already pushed and shared)

🔁 Final Merge After Rebase (optional)
If you rebased and now want to merge cleanly:

bash
Copy
Edit
git checkout main
git merge feature/login  # Fast-forward merge
No extra merge commit is created — just a straight line.

🛑 When to Use What?
Scenario	Use merge	Use rebase
Working in a team	✅ Yes	⚠️ Only before pushing
Cleaning history before final merge	❌ No	✅ Yes
You need a full history with all merges	✅ Yes	❌ No
Want a linear, simple commit history	❌ No	✅ Yes

🔐 Safe Rule:
🔸 Use merge for collaboration.
🔸 Use rebase only for local branches before pushing.

## 5. What are different types of deployment strategies?

🚀 Common Deployment Strategies
1. Recreate (Shutdown & Replace)
How it works: Stop the old version, then start the new one.

Use case: Simple, non-critical apps.

Risk: High downtime.

2. Rolling Deployment
How it works: Update instances one at a time or in batches.

Use case: Most production environments.

Pros: No full downtime.

Cons: Hard to roll back.

3. Blue-Green Deployment
How it works:

Two environments (Blue = live, Green = new).

Traffic is switched to Green after deployment.

Use case: Zero-downtime deployments.

Pros: Easy rollback (just switch back).

Cons: Requires double infrastructure.

4. Canary Deployment
How it works: Deploy to a small % of users first → monitor → increase traffic gradually.

Use case: Testing new features in production safely.

Pros: Low risk.

Cons: Needs traffic control logic (e.g., Azure Traffic Manager).

5. A/B Testing
How it works: Route different users to different versions (A or B).

Use case: Testing UI/UX or features.

Difference from Canary: A/B is for experiments, not for rollout.

6. Shadow Deployment
How it works: New version receives real traffic in parallel to production (but not user-visible).

Use case: Validating performance or output correctness.

Pros: No user impact.

Cons: Requires double processing.

🧰 Tools that support deployment strategies
Tool / Platform	Supports
Azure DevOps	Blue-Green, Canary, Rolling
GitHub Actions	All with custom logic
Kubernetes (AKS)	Rolling, Canary, Blue-Green
Azure App Services	Slot-based Blue-Green deployments
Azure Traffic Manager	Routing for Canary / A/B

## 6. Difference between Git and GitHub?

🔁 Git vs GitHub – Key Differences
Feature	Git	GitHub
What it is	A version control system (VCS)	A cloud-based Git repository hosting service
Type	Software/tool	Web platform
Function	Tracks code changes locally	Stores code remotely and supports collaboration
Installation	Installed on your system (CLI or tools like VS)	Accessed via browser (no install needed)
Main Purpose	Helps developers manage source code history	Allows sharing, collaboration, PRs, CI/CD, etc.
Usage	git init, git add, git commit, git push	Create repos, manage pull requests, issues, CI/CD
Works Without Internet	Yes (locally)	No (needs internet)
Owned By	Open-source, maintained by the community	Owned by Microsoft

🔧 Example
You use Git to:

bash
Copy
Edit
git init
git add .
git commit -m "Initial commit"
You use GitHub to:

Create a remote repository

Push your local Git repo to GitHub:

bash
Copy
Edit
git remote add origin https://github.com/yourname/repo.git
git push -u origin main
📌 Summary
🧠 Git is the tool → tracks versions locally

☁️ GitHub is the platform → stores your code online and helps you collaborate

## 7. How do you deploy static web app using different tools?

✅ 1. Azure Static Web Apps (Recommended)
Best for: React, Angular, Vue, Blazor, plain HTML/CSS/JS

Free Tier available

Automatically builds and deploys from GitHub or Azure DevOps

🔧 Tools:
GitHub Actions – CI/CD automation

VS Code Extension – for Azure Static Web Apps

Azure CLI:

bash
Copy
Edit
az staticwebapp create \
  --name my-static-site \
  --resource-group my-rg \
  --source https://github.com/user/repo \
  --location centralus \
  --app-location "/" \
  --output-location "build"
✅ 2. Azure Blob Storage (Static Website Hosting)
Best for: Static HTML/CSS/JS files

Configure container as static site

Cheap and simple

🔧 Tools:
Azure Storage Explorer

Azure CLI:

bash
Copy
Edit
az storage blob upload-batch \
  --account-name mystorageaccount \
  --source ./dist \
  --destination '$web'
Azure Portal UI to enable "Static Website" in Storage Account

✅ 3. Azure App Service (Optional for static)
Not ideal for static sites, but works

You can use App Service to host static files, especially if you're mixing APIs

🔧 Tools:
FTP / Web Deploy

GitHub Actions or Azure Pipelines

Visual Studio or VS Code

Summary Table:
Tool / Service	Best For	CI/CD Tool	Notes
Azure Static Web Apps	Modern static sites & SPAs	GitHub Actions	Automatic build/deploy
Azure Blob Storage	Plain HTML/CSS/JS websites	CLI/Explorer	Cheapest, fastest for static content
Azure App Service	Mixed static + backend APIs	DevOps, VS	Overkill for simple static sites

## 8. How do you perform rollback for production failure?

Rolling back a production deployment depends on whether you're rolling back:

🏗️ Application code (like a .NET app or Angular app), or

⚙️ Infrastructure (like Azure App Services, databases, load balancers)

Below are common rollback strategies for both scenarios in a production environment.

✅ 1. Application Rollback (e.g., .NET App in Azure)
Option 1: Using Azure App Service Deployment Slots (Blue-Green Strategy)
Use a staging slot to deploy new version.

If production fails after swap → just "swap back" to the previous slot.

🔁 Rollback steps:

Go to Azure Portal → App Service

Click “Deployment slots”

Click “Swap” to move staging back to production

🟢 Pros: Zero downtime rollback
🔴 Cons: You need to maintain two slots

Option 2: Redeploy Previous Build from CI/CD
If you deployed using Azure DevOps, Jenkins, or GitHub Actions:

Go to the pipeline history

Find the last successful build

Redeploy it manually (e.g., click "Deploy" or re-run with same artifact)

💡 Tip: Always tag your releases (v1.0, v1.1) so you know which version to go back to.

Option 3: Git Revert + Redeploy
If you don’t have slots or CI versioning:

Revert the Git commit or PR that caused the issue:

bash
Copy
Edit
git revert <commit SHA>
git push origin main
Let your pipeline re-trigger and deploy the stable code

✅ 2. Infrastructure Rollback (Using Terraform or ARM/Bicep)
If your production issue is due to infrastructure changes:

🛠 Terraform Rollback:

Go back to previous Git commit:

bash
Copy
Edit
git checkout <previous stable version>
terraform plan
terraform apply
🛑 Be cautious with resources like databases or identity (Azure AD) — rollback may not be clean.

💡 Tip: Enable versioning in Azure Blob/S3 for tfstate backup.

## 9. What is the Script that you have used for automation and Why?

🐚 What is a Bash Script?
A Bash script is a file containing a series of commands written in the Bash shell language, usually used to automate tasks on Linux, macOS, or within CI/CD tools like Jenkins.

File extension: .sh (e.g., build.sh, deploy.sh)

🧠 Why Use Bash Scripts?
🔧 Task	✅ Bash Script Use
Automate build steps	✔️ dotnet build, npm install, etc.
Run test suites	✔️ dotnet test, pytest, etc.
Setup environment variables	✔️ Set API keys, secrets, configs
Install dependencies	✔️ apt-get, yum, brew
Run CI/CD jobs in Jenkins, GitHub Actions	✔️ Trigger from sh ./build.sh
Deploy code	✔️ Use SSH, SCP, rsync, Azure CLI

📄 Example: Simple Bash Script (build.sh)
bash
Copy
Edit
#!/bin/bash

echo "🔧 Restoring dependencies..."
dotnet restore

echo "🏗️ Building project..."
dotnet build --configuration Release

echo "✅ Running tests..."
dotnet test

echo "🚀 Publishing..."
dotnet publish -c Release -o ./publish
🔥 How to Run a Bash Script
Make it executable first:

bash
Copy
Edit
chmod +x build.sh
Then run it:

bash
Copy
Edit
./build.sh
## 📌 Use in Jenkinsfile (example)
groovy
Copy
Edit
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh './build.sh'
            }
        }
    }
}
✅ Summary: When to Use Bash Scripts
When you want to automate repetitive tasks

When using CI/CD pipelines (Jenkins, GitHub Actions, GitLab)

For cross-platform scripting (in Docker, Linux VMs, etc.)

To keep your build logic separate from pipeline YAMLs



## 10. What are the steps involved in deplyoing .net application in appservice?

✅ CI (Continuous Integration) Steps for .NET Applications
These steps are triggered after every commit or pull request.

🔹 1. Checkout Code
Pull latest code from Git repository.

yaml
Copy
Edit
- task: Checkout@1
🔹 2. Restore Dependencies
Restore NuGet packages.

bash
Copy
Edit
dotnet restore
🔹 3. Build the Application
Compile your .NET project.

bash
Copy
Edit
dotnet build --configuration Release
🔹 4. Run Unit Tests
Execute tests (xUnit, NUnit, MSTest).

bash
Copy
Edit
dotnet test --no-build --configuration Release
🔹 5. Static Code Analysis (Optional but recommended)
Tools: SonarQube, ReSharper CLI, or .NET analyzers.

bash
Copy
Edit
dotnet sonarscanner begin ...
dotnet build
dotnet sonarscanner end
🔹 6. Publish Artifacts
Publish output (DLLs, config files) for deployment.

bash
Copy
Edit
dotnet publish -c Release -o $(Build.ArtifactStagingDirectory)
Save to artifact storage (Azure Artifacts, GitHub Packages, etc.).

🚀 CD (Continuous Deployment/Delivery) Steps
These steps are triggered after a successful build, or when you manually promote a release.

🔹 1. Download Build Artifacts
From CI pipeline.

🔹 2. Infrastructure Provisioning (Optional)
Use Terraform, ARM templates, or Bicep to provision Azure resources.

🔹 3. Deploy to Environment
Web App / App Service:

bash
Copy
Edit
az webapp deploy ...
Or use Azure DevOps release task:

yaml
Copy
Edit
- task: AzureWebApp@1
Or for containers:

bash
Copy
Edit
docker build -t myapp .
docker push <registry>
kubectl apply -f deployment.yaml
🔹 4. Run Post-Deployment Smoke Tests
Verify API endpoints or UI using Postman/Newman.

🔹 5. Monitor Application Health
Use Datadog, Azure Application Insights, or ELK Stack.

Set up alerts/logs for performance, failures, etc.

💡 Example: Typical Azure DevOps Pipeline Flow
markdown
Copy
Edit
CI Pipeline
-----------
1. Checkout Code
2. Restore NuGet packages
3. Build (.NET Core)
4. Run Tests
5. Publish Artifacts

CD Pipeline
-----------
1. Download Artifacts
2. Deploy to Azure Web App (Dev/UAT/Prod)
3. Run Smoke Tests
4. Notify (email/Teams/Slack)


## 11. what is Iaac?

🏗️ What is IaC (Infrastructure as Code)?
IaC (Infrastructure as Code) is the practice of managing and provisioning infrastructure (servers, networks, databases, etc.) using code, instead of manual setup via UI.

✅ IaC = Define your infrastructure (VMs, Storage, App Services) as version-controlled, repeatable code.

💡 Why Use IaC?
✅ Benefit	💬 Explanation
🌀 Automation	Automatically create/update/delete infrastructure
🎯 Consistency	Same infra across Dev, QA, and Prod environments
🕒 Faster Deployments	No manual portal clicks; fully scripted and repeatable
📦 Version Control	Infra changes are trackable just like code
🤝 Collaboration	Teams can work on infra using Git, pull requests, etc.

🛠️ Popular IaC Tools
Tool	Language	Cloud Support	Example Use
Terraform	HCL (HashiCorp)	Multi-cloud (Azure, AWS, GCP)	Create VMs, Storage, Networks
Bicep	Bicep (DSL)	Azure only	App Services, DBs, Key Vault, etc.
ARM Templates	JSON	Azure only	Infrastructure configuration
Pulumi	C#, TS, Python	Multi-cloud	Infra using real programming languages
Ansible	YAML + Python	Linux/Windows config	Software install, updates, patching


## 12. How do you troubleshhot build failure?

✅ 1. Read the Build Logs Carefully
Go to Jenkins Console Output or pipeline logs.

Find:

❌ Error messages (like CS1002: ; expected or npm ERR!)

🔚 Last successful step → tells where it broke.

📌 Focus on the first error, not the flood of errors that come after it.

✅ 2. Check Code for Build/Compilation Errors
For .NET:

Missing references?

NuGet packages restored?

Wrong framework version (.NET 6 vs .NET Core 3.1)?

Common Fix: Run the same build command locally:

bash
Copy
Edit
dotnet build MyApp.sln
✅ 3. NuGet Restore Issues
If build fails during restore:

Check nuget.config – is it pointing to the correct source?

Is the private feed (like Azure Artifacts) authenticated?

bash
Copy
Edit
dotnet restore
✅ 4. CI Tool Configuration Errors
Wrong working directory in Jenkins?

Missing credentials for GitHub or Azure?

Missing environment variables?

Fix:

Check pipeline script:

workingDirectory, dotnet publish, npm run build, etc.

✅ 5. Dependency Errors
For frontend (Angular, React) apps:

npm install fails → check package.json and package-lock.json.

For backend:

Missing or outdated NuGet packages.

✅ 6. Check Jenkins Agent / Runner Environment
Does the agent have .NET SDK, Node.js, Java, etc.?

Run the build in the same agent locally (Docker container or VM) if needed.

bash
Copy
Edit
dotnet --list-sdks
✅ 7. Disk Space / Out of Memory
Jenkins agents may crash if:

No disk space

Memory overflow (watch logs for OOMKilled or OutOfMemoryException)

✅ 8. Incorrect Branch or Git URL
Make sure correct branch is selected.

Webhook properly triggers build?

✅ 9. Authentication & Permissions
Build fails when:

Pushing to protected branch

Deploying to Azure with expired Service Principal

Fix:

Refresh tokens/secrets in Jenkins credentials

✅ 10. Retry the Build
Sometimes the error is transient:

GitHub rate-limits

Network blips

Azure temporarily fails

Use Retry Build to confirm.

🛠️ Common Jenkins Build Stage
groovy
Copy
Edit
pipeline {
  agent any

  stages {
    stage('Restore') {
      steps {
        sh 'dotnet restore'
      }
    }

    stage('Build') {
      steps {
        sh 'dotnet build --no-restore'
      }
    }

    stage('Test') {
      steps {
        sh 'dotnet test'
      }
    }
  }
}


## 13. Agile methodologies

Agile Methodologies are iterative and incremental approaches to software development that emphasize:

Customer collaboration

Frequent delivery

Responding to change

Working software over documentation

📌 Core Principles (from Agile Manifesto)
Individuals and interactions > Processes and tools

Working software > Comprehensive documentation

Customer collaboration > Contract negotiation

Responding to change > Following a plan

🚀 Popular Agile Methodologies
1. Scrum
Roles: Product Owner, Scrum Master, Development Team

Artifacts: Product Backlog, Sprint Backlog, Increment

Ceremonies:

Sprint Planning

Daily Stand-up

Sprint Review

Sprint Retrospective

Sprints: Time-boxed iterations (usually 2 weeks)

2. Kanban
Focuses on visualizing workflow

Uses boards (To Do → In Progress → Done)

No fixed iterations

Encourages continuous delivery

3. Extreme Programming (XP)
Focuses on engineering practices

Key practices:

Test-Driven Development (TDD)

Pair Programming

Continuous Integration

Simple Design

4. Lean
Derived from Lean manufacturing (Toyota)

Focus on:

Eliminating waste

Empowering team

Delivering fast

Amplifying learning

5. Crystal
Lightweight methodology

Adaptable based on team size and project criticality

Encourages face-to-face communication

🔄 Agile Lifecycle
Concept – Gather high-level ideas

Inception – Define team, scope, and vision

Iteration/Increment Planning

Development – Build features in sprints

Testing & Integration

Demo/Review

Release

Retrospective – Learn and improve

🧰 Agile Tools
Tool	Purpose
Jira	Project tracking, sprints
Azure Boards	Work items, backlogs
Trello	Lightweight Kanban board
Monday.com	Team collaboration
Confluence	Documentation

## 14. Reason to choose DevOps as career

“I’m passionate about improving the software development lifecycle. DevOps gives me the tools to automate, monitor, and deploy applications more efficiently. I find it rewarding to remove bottlenecks, help teams release faster, and ensure system reliability — all of which directly improve product quality and user experience.”
