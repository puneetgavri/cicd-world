

### **1. Code Commit Phase (Trigger Point)**

* A developer commits code to a version control system (like Git in Bitbucket, GitHub, GitLab).
* This push to a specific branch (e.g., `develop`, `main`, or feature branches) triggers the CI/CD pipeline.
* A webhook from the repo notifies the CI system (e.g., Bitbucket Pipelines, Jenkins, GitHub Actions) to begin execution.

---

### **2. Continuous Integration (CI) Phase**

This phase ensures that changes are **valid, secure, and working at a code level**.

#### a. Checkout and Setup

* The pipeline checks out the latest code from the repo.
* Initializes environment variables and installs dependencies.

#### b. Static Analysis & Linting

* Runs static code analyzers (e.g., SonarQube, ESLint, pylint).
* Lints the code to ensure formatting and code style standards.

#### c. Security Scanning

* Scans for:

  * Secrets and credentials accidentally committed (e.g., truffleHog, detect-secrets).
  * Vulnerabilities in dependencies using SCA tools (e.g., Snyk, OWASP Dependency Check).
  * Misconfigurations in IaC (e.g., tfsec for Terraform, Checkov).

#### d. Unit Testing

* Executes all unit tests to validate individual modules.
* Collects code coverage reports.

#### e. Build Artifact Creation

* Compiles the source code (if applicable).
* Packages it into build artifacts (e.g., JARs, Docker images, ZIPs for Lambda).
* Pushes them to artifact repositories (e.g., Artifactory, ECR, S3).

---

### **3. Continuous Testing Phase**

This ensures the application functions **as a whole**, not just as individual components.

#### a. Integration Testing

* Deploys the app (or part of it) into a **test environment** (e.g., a dev/staging namespace or temp infra).
* Runs integration tests that test multiple components (e.g., backend + DB, microservice A + B).

#### b. End-to-End (E2E) Testing

* Tests full user flows via tools like Selenium, Cypress, or Playwright.
* Validates UI behavior and critical workflows in staging.

#### c. Contract Testing / API Testing

* Verifies that service APIs meet contract expectations using tools like Postman/Newman, Pact.

---

### **4. Continuous Delivery (CD) Phase**

This phase handles **deployment** of the code to target environments, and ensures it is **safe and validated**.

#### a. Terraform Plan (Infrastructure Code)

* Uses Terraform (or equivalent) to plan infrastructure changes.
* Shows a preview of resources to add/change/delete.

#### b. Terraform Apply

* Applies infrastructure changes to the target environment (dev, staging, or prod).

#### c. Application Deployment

* Deploys the packaged code (Docker image, ZIP, JAR) to:

  * ECS / EKS / EC2 / Lambda / S3 / Elastic Beanstalk, etc.
* Based on environment (`dev`, `qa`, `prod`), different deployment strategies may be used:

  * Rolling updates
  * Blue/Green deployment
  * Canary releases

#### d. Post-Deployment Smoke Tests

* Automatically run quick health checks or functional tests to confirm deployment success.

---

### **5. Continuous Monitoring & Feedback**

This ensures **live observability** of the system and rapid issue detection.

#### a. Logging and Metrics

* Logs are collected using tools like CloudWatch, ELK Stack, Fluent Bit.
* Metrics (CPU, memory, request rate, error rate) are collected using Prometheus, Grafana, or Datadog.

#### b. Alerting

* Set thresholds on metrics and logs.
* Integrate alerts with Slack, PagerDuty, email.

#### c. Synthetic Monitoring / RUM

* Use uptime monitoring (Pingdom, New Relic Synthetics) or Real User Monitoring to observe app behavior from user locations.

#### d. Rollback (if needed)

* If the smoke tests or monitoring show errors, automated rollback or manual intervention is triggered.

---

### **6. Promotion to Production**

Once the application has been validated in staging:

* A pipeline (manual or automated) promotes the deployment to the production environment.
* The same validations are repeated in production (canary or blue/green deployment).
* Monitoring continues in real time.

