# Dell Automation Studio Advanced Test Plan

## Test Plan Identifier
- **Document ID:** DAS-ADVANCED-TP-001
- **Version:** 1.0
- **Date:** July 29, 2026
- **Author:** Dell Blueprint Assist Team
- **Target Audience:** Dell partners who have completed the basic evaluation and need to create complex blueprints and integrations for customer deployments
- **Prerequisite:** Completion of Dell Automation Studio Test Plan (DAS-PARTNER-TP-001)

## 1. Introduction

### 1.1 Purpose
This advanced test plan provides partners with hands-on experience creating complex multi-service blueprints, implementing custom scripts, handling integration scenarios, and troubleshooting real-world deployment issues. Partners completing this plan will be self-sufficient in authoring production-ready blueprints for customer environments.

### 1.2 Scope
**In Scope:**
- Multi-service blueprint authoring (3+ services with dependencies)
- Custom Python script development with `from dell import ctx`
- Drift detection and update workflows
- Service composition with ServiceComponent and SharedResource
- Integration with customer-specific plugins and environments
- Troubleshooting deliberate failures and diagnostician usage
- Blueprint optimization and best practices

**Out of Scope:**
- Custom plugin development (wgn file creation)
- Multi-orchestrator cross-environment deployments
- Production disaster recovery scenarios
- Advanced CI/CD pipeline integration

### 1.3 Prerequisites
- **Completed:** Dell Automation Studio Test Plan (DAS-PARTNER-TP-001)
- **System:** Same as basic plan (Windows 10/11, macOS 10.15+, Ubuntu 20.04+, 8GB RAM)
- **Access:** DAP orchestrator with production-like environment
- **IDE:** Windsurf or Claude Code with dap-bpa skills loaded
- **Skills:** Proficiency with basic blueprint authoring and dap-bpa CLI

## 2. Test Strategy

### 2.1 Approach
The advanced plan is divided into 4 two-hour modules, each focusing on a complex scenario. Partners will build progressively more sophisticated blueprints, starting with multi-service applications and advancing to integration and troubleshooting scenarios.

### 2.2 Test Environment
- **Local Workstation:** Partner's machine with dap-bpa CLI installed
- **AI IDE:** Windsurf or Claude Code with dap-bpa skills loaded
- **Orchestrator:** DAP/Dell Distributed Private Cloud (production-like environment)
- **Test Scenarios:** Multi-tier web application, microservices architecture, customer integration

### 2.3 Entry Criteria
- Partner has completed basic test plan successfully
- Partner has orchestrator access with appropriate permissions
- Partner is comfortable with basic blueprint authoring
- Partner has access to customer-specific plugin documentation (if applicable)

### 2.4 Exit Criteria
- All advanced test cases pass
- Partner can independently author multi-service blueprints
- Partner can implement custom scripts and drift detection
- Partner can troubleshoot and resolve deployment failures
- Partner completes advanced self-assessment with positive feedback

## 3. Test Schedule

| Module | Duration | Description |
|--------|----------|-------------|
| Module 1 | 120 min | Multi-Service Blueprint Authoring |
| Module 2 | 120 min | Custom Scripts and Drift Detection |
| Module 3 | 120 min | Service Composition and Integration |
| Module 4 | 120 min | Troubleshooting and Optimization |
| Module 5 | 30 min | Advanced Learner Assessment |
| **Total** | **8.5 hours** | |

## 4. Test Cases

### Module 1: Multi-Service Blueprint Authoring

| Test Case ID | TC-ADV-001 | TC-ADV-002 | TC-ADV-003 | TC-ADV-004 |
|--------------|-----------|-----------|-----------|-----------|
| **Description** | Design Multi-Service Architecture | Author Web Service Blueprint | Author Database Service Blueprint | Author Load Balancer Service |
| **Duration** | 30 min | 30 min | 30 min | 30 min |
| **Preconditions** | Basic blueprint authoring proficiency | Architecture designed | Web service authored | Database service authored |
| **Test Steps** | 1. Define 3-tier architecture: Web → App → Database<br>2. Identify node types for each tier<br>3. Define service dependencies<br>4. Plan shared resources (network, storage) | 1. Create web-service-blueprint/<br>2. Ask agent: "@dap-bpa Build a web service blueprint using dell.nodes.kubernetes.resources.Deployment with nginx. Include ingress configuration."<br>3. Add inputs for replica count, image tag, ingress host<br>4. Add capability for service endpoint | 1. Create database-service-blueprint/<br>2. Ask agent: "@dap-bpa Build a database service blueprint using dell.nodes.kubernetes.resources.StatefulSet with PostgreSQL. Include persistent volume claim."<br>3. Add inputs for storage size, database name, credentials<br>4. Add capability for connection string | 1. Create load-balancer-blueprint/<br>2. Ask agent: "@dap-bpa Build a load balancer blueprint using dell.nodes.kubernetes.resources.Service with type LoadBalancer. Configure to route to web service."<br>3. Add inputs for port configuration<br>4. Add capability for external IP |
| **Expected Result** | Architecture diagram documented<br>Node types identified<br>Dependencies mapped<br>Shared resources planned | Valid web service blueprint<br>Ingress configured<br>Inputs defined with constraints<br>Service endpoint capability exposed | Valid database service blueprint<br>Persistent volume configured<br>Credentials managed via secrets<br>Connection string capability exposed | Valid load balancer blueprint<br>LoadBalancer type configured<br>Port inputs defined<br>External IP capability exposed |
| **Actual Result** | | | | |
| **Status** | | | | |
| **Notes** | Use dell.nodes.kubernetes or dell.nodes.vsphere based on environment | Ensure ingress controller available in cluster | Use dell.nodes.kubernetes.resources.PersistentVolumeClaim | Ensure LoadBalancer type supported by cluster |

---

### Module 2: Custom Scripts and Drift Detection

| Test Case ID | TC-ADV-005 | TC-ADV-006 | TC-ADV-007 | TC-ADV-008 |
|--------------|-----------|-----------|-----------|-----------|
| **Description** | Create Custom Install Script | Create Custom Runtime Properties Script | Implement Drift Detection | Implement Update Workflow |
| **Duration** | 30 min | 30 min | 30 min | 30 min |
| **Preconditions** | Web service blueprint authored | Install script created | Runtime properties script created | Drift detection implemented |
| **Test Steps** | 1. Create scripts/install.py in web service<br>2. Write script using `from dell import ctx`<br>3. Implement custom configuration logic<br>4. Add error handling and logging | 1. Create scripts/runtime_properties.py<br>2. Use `ctx.instance.runtime_properties` to store deployment metadata<br>3. Store service endpoint, pod IPs, configuration hash<br>4. Return properties for capability outputs | 1. Add check_drift interface to web service node<br>2. Create scripts/check_drift.py<br>3. Compare current state vs desired state (image tag, replica count)<br>4. Return drift information in system_properties | 1. Add update interface to web service node<br>2. Create scripts/update.py<br>3. Implement idempotent update logic<br>4. Add postupdate interface for verification<br>5. Add skip_reinstall handling |
| **Expected Result** | Custom install script created<br>Uses ctx API correctly<br>Error handling implemented<br>Logging added for debugging | Runtime properties captured<br>Metadata stored correctly<br>Capability outputs reference runtime properties<br>Properties accessible in later workflows | Drift detection implemented<br>Compares current vs desired state<br>Drift information stored in system_properties<br>Check returns no drift when unchanged | Update workflow implemented<br>Idempotent update logic<br>Postupdate verification passes<br>skip_reinstall respected<br>Blueprint passes ND-009 rule |
| **Actual Result** | | | | |
| **Status** | | | | |
| **Notes** | Refer to dap-scripts skill for ctx API patterns | Use get_attribute, get_property, get_input intrinsic functions | Drift stored in system_properties["configuration_drift"] | Update workflow requires check_drift per ND-009 |

---

### Module 3: Service Composition and Integration

| Test Case ID | TC-ADV-009 | TC-ADV-010 | TC-ADV-011 | TC-ADV-012 |
|--------------|-----------|-----------|-----------|-----------|
| **Description** | Create Composite Blueprint | Implement ServiceComponent | Implement SharedResource | Chain Blueprint Deployments |
| **Duration** | 30 min | 30 min | 30 min | 30 min |
| **Preconditions** | All service blueprints authored | Composite blueprint created | ServiceComponent implemented | SharedResource implemented |
| **Test Steps** | 1. Create composite-blueprint/<br>2. Import web, database, load balancer blueprints<br>3. Define node templates for each service<br>4. Connect services via relationships | 1. Add ServiceComponent node to composite<br>2. Configure sub-deployment reference to web service<br>3. Pass inputs from composite to sub-deployment<br>4. Capture outputs from sub-deployment | 1. Add SharedResource node for network<br>2. Configure shared network across services<br>3. Add SharedResource node for storage (if applicable)<br>4. Ensure proper isolation and naming | 1. Create deployment-chain-blueprint/<br>2. Use deployment relationships to order deployments<br>3. Configure wait conditions between deployments<br>4. Test deployment order execution |
| **Expected Result** | Composite blueprint created<br>All services imported<br>Node templates defined<br>Relationships connect services | ServiceComponent configured<br>Inputs passed correctly<br>Outputs captured<br>Sub-deployment executes successfully | SharedResource configured<br>Network shared across services<br>Storage shared appropriately<br>Isolation maintained | Deployment chain created<br>Deployments execute in correct order<br>Wait conditions respected<br>Chain completes successfully |
| **Actual Result** | | | | |
| **Status** | | | | |
| **Notes** | Use `imports:` section to reference other blueprints | Refer to dap-service-composition skill for patterns | SharedResource requires dell.nodes.SharedResource node type | Use relationship type: dell.relationships.DependsOn |

---

### Module 4: Troubleshooting and Optimization

| Test Case ID | TC-ADV-013 | TC-ADV-014 | TC-ADV-015 | TC-ADV-016 |
|--------------|-----------|-----------|-----------|-----------|
| **Description** | Introduce Deliberate Failure | Diagnose Failure via Events | Use Diagnostician for Auto-Repair | Optimize Blueprint Performance |
| **Duration** | 30 min | 30 min | 30 min | 30 min |
| **Preconditions** | Composite blueprint deployed | Failure introduced | Diagnostician configured | Blueprint deployed successfully |
| **Test Steps** | 1. Modify blueprint to introduce error (invalid node type, missing required property)<br>2. Upload and attempt deployment<br>3. Capture execution failure<br>4. Document error symptoms | 1. Run `dap-bpa orchestrator events get <execution_id>`<br>2. Analyze event stream for error root cause<br>3. Identify failing node and operation<br>4. Correlate with blueprint structure | 1. Configure diagnostician adapter (Bedrock, OpenAI, Claude Code, or Devin)<br>2. Run `dap-bpa monitor --file blueprint.yaml --inputs inputs.json`<br>3. Monitor auto-repair attempts<br>4. Review diagnostician suggestions<br>5. Apply approved fixes | 1. Analyze blueprint for optimization opportunities<br>2. Reduce unnecessary node dependencies<br>3. Optimize input defaults and constraints<br>4. Improve documentation and descriptions<br>5. Re-validate and re-deploy |
| **Expected Result** | Failure reproduced reliably<br>Error symptoms documented<br>Failure isolated to specific node/operation | Root cause identified<br>Event stream analysis complete<br>Fix approach determined | Diagnostician configured<br>Auto-repair attempts executed<br>Suggestions reviewed and applied<br>Blueprint passes validation | Blueprint optimized<br>Dependencies reduced<br>Inputs improved<br>Documentation enhanced<br>Deployment succeeds faster |
| **Actual Result** | | | | |
| **Status** | | | | |
| **Notes** | Use common failure patterns: missing plugin, invalid property, credential error | Look for error codes in event stream (e.g., 400, 404, 500) | Diagnostician requires LLM adapter configured via `dap-bpa setup` | Use `dap-bpa blueprint lint --verify` to catch optimization opportunities |

---

### Module 5: Advanced Learner Assessment

| Test Case ID | TC-ADV-017 | TC-ADV-018 | TC-ADV-019 |
|--------------|-----------|-----------|-----------|
| **Description** | Advanced Capability Confidence | Complex Blueprint Self-Assessment | Advanced Feedback Submission |
| **Duration** | 10 min | 10 min | 10 min |
| **Preconditions** | All advanced test cases completed | Complex blueprint authored | All modules completed |
| **Test Steps** | 1. Rate confidence (1-5) for advanced capabilities:<br>   - Multi-service authoring<br>   - Custom script development<br>   - Drift detection implementation<br>   - Service composition<br>   - Troubleshooting and repair<br>2. Save assessment | 1. Document complex blueprint created during test<br>2. Describe architecture and design decisions<br>3. List challenges encountered and resolutions<br>4. Identify areas for further improvement | 1. Navigate to GitHub repo<br>2. Select "Learner Feedback" template<br>3. Complete advanced feedback form:<br>   - Most valuable advanced capability<br>   - Most challenging area<br>   - Suggestions for advanced training<br>   - Real-world scenarios to add<br>4. Submit issue |
| **Expected Result** | Confidence ratings recorded for all 5 advanced areas<br>Assessment saved locally | Complex blueprint documented<br>Design decisions explained<br>Challenges and resolutions captured<br>Improvement areas identified | Advanced feedback submitted<br>Real-world scenarios captured<br>Dell team notified for review |
| **Actual Result** | | | |
| **Status** | | | |
| **Notes** | Use 1-5 scale: 1=No confidence, 5=Fully confident | Include blueprint YAML or diagram in documentation | Feedback template available at github.com/tme-tech-ops/blueprint-assist-training/issues/new?template=learner-feedback.yml |

---

## 5. Test Data

### 5.1 Multi-Service Architecture
- **Web Service:** nginx deployment (dell.nodes.kubernetes.resources.Deployment)
- **Database Service:** PostgreSQL StatefulSet (dell.nodes.kubernetes.resources.StatefulSet)
- **Load Balancer:** LoadBalancer Service (dell.nodes.kubernetes.resources.Service)
- **Shared Resources:** Network namespace, PersistentVolumeClaim

### 5.2 Custom Script Templates
- **install.py:** Custom configuration logic using `from dell import ctx`
- **runtime_properties.py:** Metadata capture using `ctx.instance.runtime_properties`
- **check_drift.py:** State comparison logic
- **update.py:** Idempotent update logic

### 5.3 Integration Scenarios
- **Customer Plugin:** Custom node type from customer-specific plugin
- **Service Composition:** 3-tier web application with dependencies
- **Deployment Chain:** Sequential deployment with wait conditions

---

## 6. Defect Tracking

| Defect ID | Description | Severity | Status | Assigned To |
|-----------|-------------|----------|--------|-------------|
| | | | | |

---

## 7. Success Criteria Summary

| Test Objective | Success Indicator | Test Case Reference |
|----------------|-------------------|---------------------|
| Multi-Service Authoring | Can design and author 3+ service blueprints with dependencies | TC-ADV-001, TC-ADV-002, TC-ADV-003, TC-ADV-004 |
| Custom Scripts | Can implement install, runtime properties, drift detection, update scripts | TC-ADV-005, TC-ADV-006, TC-ADV-007, TC-ADV-008 |
| Service Composition | Can implement ServiceComponent and SharedResource patterns | TC-ADV-009, TC-ADV-010, TC-ADV-011 |
| Deployment Chaining | Can chain deployments with proper ordering and wait conditions | TC-ADV-012 |
| Troubleshooting | Can diagnose failures via events and use diagnostician for auto-repair | TC-ADV-013, TC-ADV-014, TC-ADV-015 |
| Optimization | Can optimize blueprints for performance and maintainability | TC-ADV-016 |
| Advanced Assessment | Confidence ratings recorded, complex blueprint documented, feedback submitted | TC-ADV-017, TC-ADV-018, TC-ADV-019 |

---

## 8. Troubleshooting Quick Reference

| Issue | Fix | Test Case Reference |
|-------|-----|---------------------|
| ServiceComponent deployment fails | Check sub-deployment inputs match parent blueprint | TC-ADV-010 |
| SharedResource conflict | Ensure unique resource names and proper isolation | TC-ADV-011 |
| Drift detection always reports drift | Verify check_drift logic compares correct properties | TC-ADV-007 |
| Update workflow causes reinstall | Check skip_reinstall flag and update logic idempotency | TC-ADV-008 |
| Custom script fails with ctx error | Verify `from dell import ctx` import and correct API usage | TC-ADV-005 |
| Diagnostician not repairing | Verify LLM adapter configured and credentials valid | TC-ADV-015 |
| Deployment chain hangs | Check wait conditions and relationship dependencies | TC-ADV-012 |

---

## 9. Sign-Off

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Partner Tester | | | |
| Dell Support | | | |
| Test Manager | | | |

---

## 10. Appendix

### 10.1 Resources
- **Blueprint Assist Training Repo:** [github.com/tme-tech-ops/blueprint-assist-training](https://github.com/tme-tech-ops/blueprint-assist-training)
- **Dell Automation Studio Catalog:** [automation.dell.com/catalog](https://automation.dell.com/catalog)
- **Section 5:** Skills Architecture (deep dive into skill structure)
- **Section 7:** Building Blueprints (advanced authoring patterns)
- **Section 8:** Blueprint Monitoring (diagnostician configuration)
- **Section 11:** Skill Anatomy (detailed skill breakdown)

### 10.2 Next Steps for Partners
1. Apply advanced patterns to customer-specific use cases
2. Develop organization-specific blueprint templates
3. Integrate with existing CI/CD pipelines
4. Contribute blueprints to Dell Automation Studio Catalog
5. Pursue Dell Automation Studio certification (if available)

### 10.3 Real-World Scenarios for Future Training
- Multi-cloud deployments (AWS + Azure + on-prem)
- Disaster recovery and failover workflows
- Blue-green deployment strategies
- Canary deployment patterns
- GitOps integration with blueprint versioning
