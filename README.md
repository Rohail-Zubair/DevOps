# **Terraform Best Practices**

---

## **Table of Contents**

- [Directory Structure and Organization](#directory-structure-and-organization)
- [Modular Design](#modular-design)
- [Testing](#testing)
- [Collaboration and Sharing](#collaboration-and-sharing)
- [Security](#security)
- [Reusability](#reusability)
- [Tool Integration](#tool-integration)
- [Documentation](#documentation)
- [Deployment](#deployment)
- [Cost Estimation](#cost-estimation)
- [Version Control](#version-control)
- [Backend Configuration](#backend-configuration)
- [Input and Output Values](#input-and-output-values)
- [Code Formatting and Linting](#code-formatting-and-linting)
- [Logging and Monitoring](#logging-and-monitoring)
- [Error Handling](#error-handling)
- [Production Readiness](#production-readiness)

---

## **Directory Structure and Organization**

A proper directory structure is critical for organizing and scaling Terraform configuration.

1. Separate infrastructure into environments (`dev`, `staging`, `prod`) with separate state files and datasources.  
2. Modularize configs by infrastructure layers (`network`, `compute`, `database`, etc.) or features to improve readability and reusability.  
3. Use a **root module** to call child modules and avoid cluttering the root with all configuration details.  
4. Place shared modules in a separate `/modules` folder for reusability.  
5. Create one folder per environment (`/dev`, `/staging`, `/prod`) containing that environment’s `main.tf`, `variables.tf`, etc.

### **Example**

project-name
├── dev
│   ├── main.tf
│   ├── variables.tf
│   └── output.tf
├── modules
│   ├── vpc
│   │   └── main.tf
│   └── compute
│       └── main.tf
└── prod
    ├── main.tf
    ├── variables.tf
    └── output.tf


---

## **Modular Design**

Modular design allows you to break Terraform configuration into reusable, composable blocks called **modules**.

### **Best Practices**

1. Encapsulate infrastructure layers (`network`, `compute`, `DB`, etc.) into separate modules with input variables and outputs.  
2. Use modules extensively to avoid duplication.  
3. Keep modules self-contained and loosely coupled.  
4. Avoid hardcoded values; use input variables.  
5. Use modules as **building blocks** for composition.  
6. Place reusable modules in a central `/modules` directory.  
7. Use **module outputs** instead of data sources.  
8. Version modules using **semantic versioning**.

### **Example**

project-name
├── modules
│   ├── vpc
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── output.tf
│   └── compute
│       ├── main.tf
│       ├── variables.tf
│       └── output.tf

---

## **Testing**

1. Use **Terratest** for automated tests of modules and infrastructure.  
2. Implement **integration**, **regression**, and **contract** testing.  
3. Use **Terraform workspaces** for isolated testing.  
4. Automate tests with CI/CD pipelines.  
5. Include **negative test cases** (invalid inputs, bad versions, etc.).  
6. Validate variable boundaries for safety.  
7. Enforce code formatting using `terraform fmt` in CI.

---

## **Collaboration and Sharing**

1. Use **Terraform Cloud workspaces** for shared state and locking.  
2. Enforce code reviews via **VCS integrations** (e.g., GitHub).  
3. Share modules via **Terraform Registry** or private registries.  
4. Use **semantic versioning** for shared modules.  
5. Treat modules as **internal SDKs** with ownership and support.  
6. Maintain module input/output contracts.  
7. Document all modules clearly.

---

## **Security**

1. Mark sensitive variables and store actual values securely.  
2. Encrypt and back up state files.  
3. Scan providers and modules for vulnerabilities.  
4. Restrict direct state access.  
5. Enable audit logs and secret detection in CI/CD.  
6. Use read-only plans for safe previews.  
7. Enforce reviews and automated policy checks.

---

## **Reusability**

1. Build modular, reusable components from the start.  
2. Encapsulate complexity in modules.  
3. Prefer composition over one-off configs.  
4. Maintain centralized reusable module repositories.  
5. Use **semantic versioning** for module APIs.  
6. Provide validated **reference architectures**.  
7. Maintain certified, well-tested modules.

---

## **Tool Integration**

1. **Git:** Version control and GitOps workflows.  
2. **CI/CD:** Automated testing and deployment.  
3. **Cloud IDEs:** Development without local setup.  
4. **Linters:** Early issue detection (e.g., `tflint`, `checkov`).  
5. **Terraform Cloud:** Remote state management and access control.

---

## **Documentation**

1. Follow **Terraform Registry format** for module documentation.  
2. Include a `README.md` for every module with usage examples.  
3. Add inline comments explaining key resources.  
4. Link to related design docs or decisions.  
5. Include warnings or limitations clearly.

---

## **Deployment**

1. Automate deployments with **CI/CD pipelines**.  
2. Use **blue-green** or **canary** deployment patterns.  
3. Run pre/post checks for credentials and dependencies.  
4. Never store credentials in code.  
5. Add **health checks** and **gates**.  
6. Minimize blast radius using small, isolated modules.

---

## **Cost Estimation**

1. Use Terraform’s **cost estimation tools** pre-deployment.  
2. Define **budget alerts** and **quota limits** as code.  
3. Monitor cost vs usage and right-size resources.  
4. Tune auto-scaling based on actual load.  
5. Reuse shared assets and caching.  
6. Enforce tagging for cost accountability.

---

## **Version Control**

1. Require **reviews** for all code changes.  
2. Apply changes in lower environments first.  
3. Use **branch protections** and status checks.  
4. Track configuration drift and enable rollbacks.  
5. Treat Terraform like production code.

---

## **Backend Configuration**

1. Prefer **Terraform Cloud** backend.  
2. Enable **state locking** to prevent conflicts.  
3. Restrict access to backend state files.  
4. Maintain **local + remote backups**.  
5. Use **short-lived credentials** with least privilege.

---

## **Input and Output Values**

1. Define input variables for configurable values.  
2. Set sensible defaults to simplify use.  
3. Add descriptions, types, and validations.  
4. Expose only necessary outputs.  
5. Mark sensitive variables appropriately.  
6. Use consistent naming (`snake_case`).

---

## **Code Formatting and Linting**

1. Use `terraform fmt` in CI/CD pipelines.  
2. Enforce **linting** with `tflint` or `checkov`.  
3. Enable IDE integrations for real-time feedback.  
4. Use Terraform’s official formatter for consistency.

---

## **Logging and Monitoring**

1. Enable resource-level logging and central aggregation.  
2. Track infrastructure changes across environments.  
3. Export metadata to CMDB or tracking systems.  
4. Integrate state visibility with monitoring dashboards.

---

## **Error Handling**

1. Validate environments pre- and post-deployment.  
2. Add assertions and preconditions to modules.  
3. Test with invalid inputs for resilience.  
4. Use `create_before_destroy` for safe rollouts.  
5. Test rollback procedures periodically.

---

## **Production Readiness**

1. Run **health checks** before production rollout.  
2. Test **rollback** workflows in staging.  
3. Use **canary** or **gradual** deployments.  
4. Define and enforce **SLAs**.  
5. Simulate **disaster recovery** scenarios.  
6. Run **smoke tests** under production-like load.  
7. Require **ops reviews** for production changes.

---

