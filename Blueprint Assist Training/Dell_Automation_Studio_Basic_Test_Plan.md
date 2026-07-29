# Dell Automation Studio Basic Test Plan

## Test Plan Identifier
- **Document ID:** DAS-BASIC-TP-001
- **Version:** 1.0
- **Date:** July 29, 2026
- **Author:** Dell Blueprint Assist Team
- **Target Audience:** Dell partners evaluating Dell Automation Studio (Blueprint Assist) for infrastructure automation

## 1. Introduction

### 1.1 Purpose
This test plan provides a structured 4-hour evaluation path for partners to validate Blueprint Assist capabilities, including installation, blueprint authoring with AI assistance, local validation, deployment, and lifecycle management.

### 1.2 Scope
**In Scope:**
- dap-bpa CLI installation and configuration
- AI IDE integration (Windsurf/Claude Code)
- Knowledge base discovery and blueprint analysis
- Local blueprint validation (linting and schema validation)
- AI-assisted blueprint authoring
- Blueprint upload, deployment, and execution (with orchestrator access)
- Blueprint visualization and risk analysis
- Deployment updates and cleanup

**Out of Scope:**
- Advanced custom plugin development
- CI/CD pipeline integration
- Multi-orchestrator environment management
- Production deployment validation

### 1.3 Prerequisites
- **System:** Windows 10/11, macOS 10.15+, or Ubuntu 20.04+ with 8GB RAM minimum
- **Software:** Git installed
- **Access:** Dell Automation Studio catalog for dap-bpa installer
- **IDE:** Windsurf (recommended) or Claude Code
- **Credentials:** DAP orchestrator credentials (optional — offline path available)

## 2. Test Strategy

### 2.1 Approach
The test plan is divided into 4 one-hour modules, each building on the previous. Partners can complete the full path with orchestrator access or an alternative offline path for Hour 3.

### 2.2 Test Environment
- **Local Workstation:** Partner's machine with dap-bpa CLI installed
- **AI IDE:** Windsurf or Claude Code with dap-bpa skills loaded
- **Orchestrator:** DAP/Dell Distributed Private Cloud (optional)
- **Test Blueprint:** Simple nginx deployment on Kubernetes or vSphere VM

### 2.3 Entry Criteria
- Partner has access to Dell Automation Studio
- Partner has AI IDE installed or can install during test
- Partner has orchestrator credentials (if testing deployment path)

### 2.4 Exit Criteria
- All test cases in the selected path pass
- Partner can independently perform core BPA workflows
- Partner completes self-assessment with positive feedback

## 3. Test Schedule

| Module | Duration | Description |
|--------|----------|-------------|
| Module 1 | 60 min | Installation, Setup, and Offline Discovery |
| Module 2 | 60 min | Blueprint Authoring with AI Assistance |
| Module 3A | 60 min | Deployment and Execution (with orchestrator) |
| Module 3B | 60 min | Offline Deep Dive (without orchestrator) |
| Module 4 | 60 min | Update, Cleanup, and Wrap-Up |
| Module 5 | 30 min | Learner Feedback and Assessment (post-test) |
| **Total** | **4.5 hours** | |

## 4. Test Cases

### Module 1: Installation, Setup, and Offline Discovery

| Test Case ID | TC-001 | TC-002 | TC-003 | TC-004 |
|--------------|--------|--------|--------|--------|
| **Description** | Install dap-bpa CLI | Install AI IDE and Load Skills | Explore Knowledge Base | Download and Lint Blueprint |
| **Duration** | 15 min | 15 min | 15 min | 15 min |
| **Preconditions** | Installer downloaded from Dell Automation Studio | dap-bpa CLI installed | dap-bpa CLI installed | dap-bpa CLI installed |
| **Test Steps** | 1. Run `.\bpa-win-x64-*-setup.exe`<br>2. Run `dap-bpa --version`<br>3. Run `dap-bpa --help` | 1. Install Windsurf from windsurf.com<br>2. Enter SSO key: dell<br>3. Run `dap-bpa setup-ide windsurf`<br>4. Run `dap-bpa status` | 1. Run `dap-bpa knowledge blueprints find "vm"`<br>2. Run `dap-bpa knowledge plugins list vsphere`<br>3. Run `dap-bpa knowledge plugins get vsphere dell.nodes.vsphere.Server` | 1. Download blueprint from catalog<br>2. Run `dap-bpa blueprint lint --file blueprint.yaml --verify`<br>3. Review diagnostics report |
| **Expected Result** | Version number returned<br>Help displays command groups | Windsurf launches with SSO<br>Skills installed in status<br>Agent recognizes `@dap-bpa` | Relevant blueprint results returned<br>Plugin node types listed<br>Node type details displayed | Lint completes without fatal errors<br>Diagnostics report displays findings<br>Partner understands linter checks |
| **Actual Result** | | | | |
| **Status** | | | | |
| **Notes** | Open new terminal if command not found | Re-run setup-ide if agent doesn't recognize skills | Works offline, no orchestrator needed | Findings are expected learning exercise |

---

### Module 2: Blueprint Authoring with AI Assistance

| Test Case ID | TC-005 | TC-006 | TC-007 | TC-008 |
|--------------|--------|--------|--------|--------|
| **Description** | Author Blueprint Framework | Add Inputs and Capabilities | Validate Locally | Generate Blueprint Visualizer |
| **Duration** | 20 min | 15 min | 15 min | 10 min |
| **Preconditions** | Windsurf with dap-bpa skills loaded | Blueprint framework created | Blueprint with inputs created | Blueprint validated |
| **Test Steps** | 1. Create folder `my-test-blueprint`<br>2. Create `blueprint.yaml`<br>3. Ask agent: "@dap-bpa Build me a blueprint framework for nginx deployment on Ubuntu using dell.nodes.kubernetes.resources.Deployment"<br>4. Review structure | 1. Ask agent: "@dap-bpa Add input groups for configuration and networking. Add constraint replica_count between 1 and 10. Add capability for service endpoint."<br>2. Review inputs/capabilities<br>3. Ensure CHANGELOG.yaml exists | 1. Run `dap-bpa blueprint lint --file blueprint.yaml --verify`<br>2. Run `dap-bpa blueprint validate-all --file blueprint.yaml`<br>3. Fix any errors | 1. Run `dap-bpa blueprint visualize --file blueprint.yaml`<br>2. Open generated HTML in browser |
| **Expected Result** | Valid blueprint framework generated<br>Proper node types, inputs, lifecycle<br>Partner can explain structure | Inputs have descriptions (IN-001)<br>Constraints properly defined<br>Capabilities use `capabilities:` (CP-001)<br>CHANGELOG.yaml exists (BS-009) | Passes all lint rules (TD-002: dell.* prefix)<br>Schema validation succeeds<br>No critical errors | HTML file generates<br>Browser displays topology diagram<br>Risk Analysis panel shows findings |
| **Actual Result** | | | | |
| **Status** | | | | |
| **Notes** | Use dell.nodes.kubernetes or dell.nodes.vsphere | Verify compliance with blueprint-rules.md | Fix node type references and plugin imports | Risk analysis runs ~25 rules across security/reliability |

---

### Module 3A: Deployment and Execution (with Orchestrator)

| Test Case ID | TC-009 | TC-010 | TC-011 | TC-012 |
|--------------|--------|--------|--------|--------|
| **Description** | Configure Orchestrator Connection | Upload Blueprint | Create Deployment | Execute and Monitor |
| **Duration** | 15 min | 10 min | 15 min | 20 min |
| **Preconditions** | DAP orchestrator credentials available | Blueprint validated locally | Blueprint uploaded | Deployment created |
| **Test Steps** | 1. Run `dap-bpa setup`<br>2. Enter portal/orchestrator domains, org ID, client ID/secret<br>3. Run `dap-bpa status`<br>4. Run `dap-bpa orchestrator blueprints list -o profile` | 1. Run `dap-bpa orchestrator blueprints upload --file blueprint.yaml --id my-test-blueprint --revision 1.0.0 -o profile`<br>2. Run `dap-bpa orchestrator blueprints get my-test-blueprint -o profile` | 1. Create inputs.json with test values<br>2. Run `dap-bpa orchestrator deployments create --blueprint-id my-test-blueprint --inputs inputs.json --display-name "my-test-deployment" -o profile`<br>3. Note deployment ID | 1. Run `dap-bpa orchestrator executions start --deployment-id <id> --workflow-id install -o profile`<br>2. Note execution ID<br>3. Run `dap-bpa orchestrator executions get <exec_id> -o profile`<br>4. Run `dap-bpa orchestrator events get <exec_id> -o profile` |
| **Expected Result** | Status shows orchestrator connected<br>Blueprint list succeeds | Upload completes successfully<br>Blueprint appears in list | Deployment created successfully<br>Deployment ID captured | Install workflow starts<br>Execution status monitorable<br>Events stream in real-time |
| **Actual Result** | | | | |
| **Status** | | | | |
| **Notes** | Use `--trust-all` for self-signed certs (dev only) | Revision follows semver | Inputs must match blueprint schema | Monitor for errors in event stream |

---

### Module 3B: Offline Deep Dive (without Orchestrator)

| Test Case ID | TC-013 | TC-014 | TC-015 |
|--------------|--------|--------|--------|
| **Description** | Blueprint Reasoning with AI | Explore Skills Capabilities | Risk Analysis and Remediation |
| **Duration** | 20 min | 20 min | 20 min |
| **Preconditions** | Complex blueprint downloaded from catalog | Windsurf with dap-bpa skills loaded | Blueprint visualizer generated |
| **Test Steps** | 1. Open blueprint in Windsurf<br>2. Ask: "@dap-bpa What does this blueprint deploy and what inputs does it require?"<br>3. Ask: "@dap-bpa Walk me through the install workflow step by step."<br>4. Ask: "@dap-bpa What are the security considerations?"<br>5. Review analysis | 1. Ask: "@dap-bpa What skills do you have?"<br>2. Ask: "@dap-bpa How do I add drift detection?"<br>3. Ask: "@dap-bpa How do I compose services using ServiceComponent?"<br>4. Ask: "@dap-bpa How do I update a deployment without reinstalling?" | 1. Run `dap-bpa blueprint visualize --file blueprint.yaml`<br>2. Review Risk Analysis panel in HTML<br>3. Note severity, category, remediation<br>4. Ask agent: "@dap-bpa Fix the risks flagged in the visualizer" |
| **Expected Result** | Agent provides accurate analysis<br>Partner understands blueprint purpose<br>Partner understands requirements | Agent demonstrates 7 skills knowledge<br>Partner understands when to use each skill | Risk Analysis panel displays findings<br>Agent applies suggested fixes<br>Blueprint passes risk checks after remediation |
| **Actual Result** | | | |
| **Status** | | | |
| **Notes** | Works offline, no orchestrator needed | Skills: dap, dap-scripts, dap-deployment-update, dap-service-composition, visualize-blueprint, blueprint-risk-fix, isv-blueprints | Risk rules cover security, reliability, lifecycle, operability |

---

### Module 4: Update, Cleanup, and Wrap-Up

| Test Case ID | TC-016 | TC-017 | TC-018 |
|--------------|--------|--------|--------|
| **Description** | Deployment Update | Secret Management | Cleanup |
| **Duration** | 20 min | 15 min | 15 min |
| **Preconditions** | Deployment running | Orchestrator connected | Deployment uninstalled |
| **Test Steps** | 1. Modify blueprint.yaml (change default inputs)<br>2. Run `dap-bpa orchestrator blueprints upload --file blueprint.yaml --id my-test-blueprint --revision 1.1.0 -o profile`<br>3. Create update-body.json with blueprint_version and skip_reinstall<br>4. Run `dap-bpa orchestrator deployment-updates initiate <id> --body update-body.json -o profile` | 1. Run `dap-bpa orchestrator secrets list -o profile`<br>2. Run `dap-bpa orchestrator secrets create --key test-secret --value "test-value" --display-name "Test Secret" -o profile`<br>3. Run `dap-bpa orchestrator secrets get test-secret -o profile` | 1. Run `dap-bpa orchestrator executions start --deployment-id <id> --workflow-id uninstall -o profile`<br>2. Run `dap-bpa orchestrator blueprints delete my-test-blueprint --force -o profile`<br>3. Run `dap-bpa orchestrator secrets delete test-secret -o profile` |
| **Expected Result** | New version uploads<br>Deployment update initiates<br>Partner understands update workflow | Secret created successfully<br>Secret metadata retrievable<br>Partner understands secret lifecycle | Deployment uninstalled<br>Blueprint deleted<br>Secrets cleaned up |
| **Actual Result** | | | |
| **Status** | | | |
| **Notes** | Update workflow supports version bumps, input changes, reinstall control | Secrets use type: secret_key (SC-002) | Deployment deletion via orchestrator UI only |

---

### Module 5: Learner Feedback and Assessment (Post-Test)

| Test Case ID | TC-020 | TC-021 | TC-022 |
|--------------|--------|--------|--------|
| **Description** | Capability Confidence Self-Assessment | Section Progress Tracking | Learner Feedback Submission |
| **Duration** | 10 min | 10 min | 10 min |
| **Preconditions** | All test cases completed | All test cases completed | All test cases completed |
| **Test Steps** | 1. Open `docs/LEARNER-ASSESSMENT.md` from training repo<br>2. Rate confidence level (1-5) for each capability area:<br>   - Blueprint Authoring<br>   - Blueprint Review<br>   - Blueprint Testing<br>   - Blueprint Deployment<br>   - Blueprint Maintenance<br>3. Save assessment locally | 1. Mark completed sections in assessment document:<br>   - Section 0: Quick Start<br>   - Section 1: Introduction<br>   - Section 2: Installation<br>   - Section 3: Orchestration Service Auth<br>   - Section 4: Skills Overview<br>   - Section 7: Building Blueprints<br>2. Note sections requiring additional review | 1. Navigate to [github.com/tme-tech-ops/blueprint-assist-training](https://github.com/tme-tech-ops/blueprint-assist-training)<br>2. Click "Issues" → "New Issue"<br>3. Select "Learner Feedback" template<br>4. Complete feedback form:<br>   - Most valuable capability<br>   - Most confusing/difficult area<br>   - Suggestions for improvement<br>   - Bugs or gaps encountered<br>5. Submit issue |
| **Expected Result** | Confidence ratings recorded for all 5 capability areas<br>Assessment saved locally | Completed sections marked<br>Gaps identified for follow-up learning | Learner feedback issue submitted<br>Feedback captured in public repository<br>Dell team notified for review |
| **Actual Result** | | | |
| **Status** | | | |
| **Notes** | Use 1-5 scale: 1=No confidence, 5=Fully confident | Refer to training repo `docs/LEARNER-ASSESSMENT.md` for template | Feedback template available at github.com/tme-tech-ops/blueprint-assist-training/issues/new?template=learner-feedback.yml |

---

## 5. Test Data

### 5.1 Test Blueprint
- **Name:** nginx-kubernetes-deployment
- **Node Type:** dell.nodes.kubernetes.resources.Deployment
- **Inputs:** replica_count (1-10), image_tag (string)
- **Capabilities:** service_endpoint (output)

### 5.2 Test Inputs
```json
{
  "replica_count": 3,
  "image_tag": "latest"
}
```

### 5.3 Test Secret
- **Key:** test-secret
- **Value:** test-value
- **Display Name:** Test Secret
- **Description:** For testing purposes

---

## 6. Defect Tracking

| Defect ID | Description | Severity | Status | Assigned To |
|-----------|-------------|----------|--------|-------------|
| | | | | |

---

## 7. Success Criteria Summary

| Test Objective | Success Indicator | Test Case Reference |
|----------------|-------------------|---------------------|
| Installation & Setup | dap-bpa CLI installed, IDE skills loaded, status verified | TC-001, TC-002 |
| Knowledge Base Discovery | Can search blueprints, list plugins, get node type details | TC-003 |
| Local Validation | Blueprint passes lint and schema validation | TC-004, TC-007 |
| AI-Assisted Authoring | Generated blueprint is valid and deployable | TC-005, TC-006 |
| Deployment (if applicable) | Blueprint uploaded, deployment created, install executed | TC-009, TC-010, TC-011, TC-012 |
| Visualization | HTML topology diagram generated with risk analysis | TC-008, TC-015 |
| Update & Cleanup | Deployment update performed, resources cleaned up | TC-016, TC-017, TC-018 |
| Learner Assessment | Confidence ratings recorded, progress tracked, feedback submitted | TC-020, TC-021, TC-022 |

---

## 8. Troubleshooting Quick Reference

| Issue | Fix | Test Case Reference |
|-------|-----|---------------------|
| `command not found: dap-bpa` | Open new terminal after install | TC-001 |
| Agent doesn't mention blueprints | Re-run `dap-bpa setup-ide <ide>` and restart IDE | TC-002 |
| `401` or `403` from orchestrator | Re-run `dap-bpa setup`, check credentials | TC-009 |
| SSL certificate error | Add `--trust-all` flag (dev only) | TC-009 |
| Lint returns findings | Expected — read as learning exercise | TC-004, TC-007 |
| Blueprint validation fails | Check node type references, plugin imports, input definitions | TC-007 |

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
- **Section 13:** Complete CLI command reference
- **Section 7:** Building blueprints with BPA
- **Section 9:** Blueprint reasoning and analysis

### 10.2 Next Steps for Partners
1. Practice with real infrastructure — adapt test blueprint to customer requirements
2. Explore Dell Automation Studio Catalog — browse 46 production-ready blueprints
3. Review full training — complete remaining sections in blueprint-assist-training repo
4. Integrate with CI/CD — use dap-bpa in pipelines for automated validation and deployment
5. Engage with Dell — provide feedback and request additional plugin documentation
