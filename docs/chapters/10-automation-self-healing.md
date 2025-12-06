---
layout: default
title: "Chapter 10: Automation and Self-Healing"
parent: "Part III: Technical Implementation"
nav_order: 10
permalink: /chapters/10-automation-self-healing/
---

# Chapter 10: Automation and Self-Healing


## Introduction

Automation represents the pinnacle of Event Management maturity, transforming reactive operations into proactive, self-healing infrastructure. At its core, automation enables systems to detect, diagnose, and resolve issues without human intervention, dramatically reducing response times from minutes or hours to seconds. This capability directly supports Event Management's supporting goal: **Maximize Automation**, enabling organizations to handle increasing operational complexity while maintaining or even reducing operational costs.

The journey to automation excellence is progressive. Organizations typically begin with manual event handling, gradually introducing automated responses for simple, well-understood scenarios, and ultimately achieving comprehensive self-healing capabilities where 70% or more of events are automatically resolved. This chapter explores the complete automation lifecycle, from identifying automation candidates through development, testing, deployment, and continuous optimization. We examine the critical safety controls that prevent automation from causing more harm than good, the verification mechanisms that ensure automation success, and the metrics that track automation effectiveness.

As Activity 3.5: Execute Automated Response demonstrates, automation is not merely a technical capability but a core component of the Event Management process itself. When implemented correctly with appropriate safety controls and governance, automation delivers substantial benefits: reduced Mean Time to Detect (MTTD) and Mean Time to Resolve (MTTR), consistent execution eliminating human error, and operational efficiency enabling Event Analysts to focus on complex issues requiring human judgment. By the end of this chapter, you will understand how to build a comprehensive automation strategy that balances efficiency with control, ensuring your Event Management process operates at optimal maturity levels.

## Benefits of Automation

Automation delivers transformative benefits across multiple dimensions of Event Management operations. Understanding these benefits is essential for building business cases, prioritizing automation initiatives, and measuring automation success.

### Operational Efficiency

Automated responses execute instantly without requiring Event Analyst attention. While a manual response might take 15 to 30 minutes from event detection to resolution, automated responses complete within seconds. This efficiency compounds over thousands of events, freeing Event Analysts to focus on complex issues requiring human judgment, investigation, and cross-functional coordination. Organizations with mature automation capabilities report that 50% to 70% or more of all events are automatically resolved, dramatically reducing operational workload.

The efficiency gains extend beyond time savings. **Auto-operations Success Rate**, measured as the percentage of events successfully resolved through automated responses, serves as a key performance indicator (KPI) with a target of greater than or equal to 70% for mature implementations. This metric appears in Activity 5.9: Report on Event Management Performance, demonstrating automation's critical role in overall process effectiveness.

### Consistency and Reliability

Human operators introduce variability through fatigue, interpretation differences, and occasional errors. Automated responses execute identically every time, following documented procedures precisely without deviation. This consistency is particularly valuable for routine, repeatable tasks such as restarting failed services, clearing cache files, or resetting network connections. The elimination of human error reduces the risk of misconfigurations or incomplete remediation steps that could exacerbate problems rather than resolve them.

Consistency also supports governance and compliance objectives. Automated responses create complete audit trails showing exactly what actions were taken, when, and with what results. This documentation quality supports Activity 5.3: Document Event Outcome and Policy 2: Alert Logging, which mandates that every alert be logged with complete detail information including actions taken and resolution information.

### Speed of Response

Critical events demand immediate response. Automated responses react within seconds of event detection, often resolving issues before users notice any service degradation. This speed is essential for maintaining service level agreements (SLAs) and preventing minor issues from cascading into major incidents. For example, an automated response detecting a failed database connection can immediately restart the connection service, potentially preventing dozens or hundreds of application errors that would otherwise trigger additional events and impact users.

The speed advantage becomes even more pronounced during off-peak hours when staffing may be minimal. Automated responses operate 24x7 without shift handoffs or delays, ensuring consistent response times regardless of when events occur. This capability supports Policy 1: Event Console Monitoring, which mandates continuous 24x7 monitoring and response.

### Cost Reduction

While automation requires upfront investment in design, development, and testing, the return on investment is substantial. Organizations report significant reductions in operational costs through reduced staffing requirements, fewer escalations to expensive tier 2 and tier 3 support teams, and prevention of service disruptions that would otherwise generate costly incidents. The cost-benefit analysis typically shows positive returns within six to twelve months for most automation initiatives.

Cost savings manifest in multiple ways. Reduced manual handling lowers operational expenses. Prevented incidents avoid business impact costs. Improved efficiency enables organizations to manage growing infrastructure complexity without proportionally increasing staff. These financial benefits are tracked and reported in Activity 5.9: Report on Event Management Performance, where monthly executive reports highlight cost savings from automation.

### Scalability

Manual operations scale linearly with event volume, requiring proportional increases in staff as infrastructure grows. Automation scales exponentially, handling 10,000 events as easily as 100 events with no additional operational cost. This scalability is essential for organizations experiencing rapid growth, cloud migration, or infrastructure expansion. Automated responses enable Event Management to keep pace with organizational growth without becoming a bottleneck.

The scalability benefit extends to consistency across distributed environments. Organizations operating multiple data centers or cloud regions can deploy standardized automated responses globally, ensuring consistent handling regardless of geographic location or time zone. This capability supports Policy 3: Centralized Event Management, which mandates that all events be managed in a centralized event management system with consistent handling procedures.

## Identifying Automation Candidates

Not all events are suitable for automation. Successful automation programs begin with careful selection of automation candidates, prioritizing scenarios where automation delivers maximum value with minimal risk.

### Evaluation Criteria

The process of identifying automation candidates requires systematic evaluation against multiple criteria. **Table 10.1** provides a comprehensive framework for assessing potential automation candidates.

**Table 10.1:** Automation Candidates Evaluation Criteria

| Criterion | Description | Ideal Candidate Characteristics |
|-----------|-------------|----------------------------------|
| Event Volume | How frequently does this event occur? | High volume (daily or weekly), providing significant ROI |
| Resolution Complexity | How difficult is the resolution? | Low to medium complexity with well-documented procedures |
| Risk Level | What could go wrong if automation fails? | Low risk with minimal potential for service disruption |
| Success Rate | How often does manual resolution succeed on first attempt? | Greater than 95% success rate with consistent resolution steps |
| Business Impact | What happens if this event is not resolved? | Moderate impact, requiring timely response but not immediately critical |
| Procedure Clarity | Is the resolution procedure well-documented? | Clear, step-by-step procedure with no ambiguity or judgment calls |

### High-Priority Automation Candidates

Certain event types consistently emerge as excellent automation candidates across organizations:

**Service Restart Events:** When application services or system processes fail, automated responses can immediately attempt service restart. These scenarios typically have high volume, low complexity, and high success rates. For example, when an application server process terminates unexpectedly, an automated response can execute a service restart command and verify that the process successfully restarted before closing the event as `Auto Action`.

**Disk Space Management:** Events indicating disk space thresholds have been breached often have straightforward resolutions such as clearing temporary files, archiving old logs, or compressing inactive data. Automated scripts can identify files eligible for cleanup based on age, file type, or location, execute the cleanup, and verify that disk space has been recovered to acceptable levels.

**Cache and Temporary File Clearing:** Applications frequently generate cache files and temporary data that accumulate over time. Automated responses can clear these files on schedule or in response to performance degradation events. The low risk (cache can be regenerated) combined with high frequency makes this an ideal automation candidate.

**Connection Pool Reset:** Database connection pool exhaustion is a common issue with well-known remediation. Automated responses can reset connection pools, verify that new connections are successfully established, and monitor for recurrence. This scenario represents moderate complexity but delivers substantial value by preventing application failures.

**Certificate Validation Warnings:** Automated responses can check certificate expiration dates, verify certificate chains, and generate alerts if certificates are approaching expiration. While not resolving the issue (certificate renewal requires human intervention), automation can provide early warning and consistent monitoring.

### Events Unsuitable for Automation

Equally important is recognizing scenarios where automation is inappropriate or risky:

**Complex Diagnostic Requirements:** Events requiring extensive investigation, log analysis, or cross-system correlation are not good automation candidates. Human judgment and expertise are necessary to understand context, identify root causes, and determine appropriate remediation.

**High-Risk Operations:** Any operation with potential for significant service disruption or data loss should not be automated without extensive testing and multiple safety controls. Examples include database restarts, storage reconfigurations, or network topology changes.

**Ambiguous Symptoms:** When events could indicate multiple different underlying issues, automation cannot reliably determine the correct response. These scenarios require Event Analyst investigation to diagnose the true cause before remediation.

**Low Volume Events:** Events occurring infrequently (monthly or less) typically do not justify the investment in automation development and maintenance. Manual handling remains more cost-effective for rare occurrences.

**Regulatory or Compliance Sensitive:** Operations requiring human approval, documentation, or oversight for regulatory compliance should not be fully automated. While automation can assist (gathering data, preparing reports), final execution must involve human review.

## Automation Development Lifecycle

Successful automation follows a structured development lifecycle ensuring quality, safety, and maintainability. This lifecycle mirrors software development best practices while incorporating Event Management-specific considerations.

![Figure 10.1: Automation Development Lifecycle](../assets/images/Automation Development Lifecycle.jpeg)

*Figure 10.1: Automation Development Lifecycle - This circular diagram illustrates the six phases of automation development: Identify, Design, Develop, Test, Deploy, and Monitor. Each phase feeds into the next, with the Monitor phase connecting back to Identify to support continuous improvement. The cycle emphasizes that automation is not a one-time project but an ongoing process requiring regular refinement.*

### Phase 1: Identify Candidates

The lifecycle begins with systematic identification of automation opportunities using the evaluation criteria defined in Table 10.1. This phase involves reviewing historical event data from Activity 5.6: Perform Trend Analysis to identify high-volume, repetitive events with consistent resolution patterns. Event Analysts and Event Designers collaborate to compile a prioritized list of automation candidates based on potential ROI, implementation complexity, and organizational priorities.

Key activities include analyzing event volume trends to identify the most frequent events, reviewing closure codes to find events consistently closed as manual operational actions, consulting with operational teams to understand current manual procedures, and documenting the current state including average time to resolution and success rates for manual handling.

### Phase 2: Design Automation Solution

Once candidates are identified, Event Designers create detailed automation designs specifying exactly what the automation will do, under what conditions, and with what safety controls. This design phase is critical for ensuring automation operates safely and effectively.

Design documentation includes:

**Trigger Conditions:** Precisely defining which events should trigger the automated response. Conditions may include specific event types, severity levels, source CIs, or time windows. For example, a disk cleanup automation might trigger only for Warning events indicating disk space above 85% during off-peak hours (to minimize impact on production workloads).

**Pre-Execution Validation:** Checks performed before executing remediation actions. Validations might verify that the CI is not currently in maintenance mode, confirm no higher-priority events are active on the same CI, or check that recent automation attempts have not already been made (preventing repetitive failures).

**Remediation Actions:** Step-by-step actions the automation will execute. Actions must be documented with sufficient detail that they could be executed manually if needed. Each action should include expected outcomes and timing (how long should this step take?).

**Post-Execution Verification:** How the automation will confirm that remediation was successful. Verification might include checking service status, monitoring for related events, or querying system metrics to confirm the issue is resolved.

**Safety Controls:** Mechanisms to prevent automation from causing harm. Safety controls are discussed in detail in the following section but typically include execution throttling, blackout periods, and automatic rollback capabilities.

**Escalation Conditions:** Under what circumstances the automation should halt execution and escalate to manual handling. Examples include repeated failures (automation attempted three times but issue persists), unexpected system states (CI is in an unexpected configuration), or verification failures (post-execution checks indicate the issue was not resolved).

### Phase 3: Develop Automation Script

With design complete, Event Designers or automation engineers develop the actual automation scripts, workflows, or runbooks. Development follows standard coding best practices including version control, code review, and documentation.

Scripts typically include:

**Logging and Instrumentation:** Comprehensive logging of all actions taken, decisions made, and outcomes observed. Logging supports troubleshooting, audit requirements, and continuous improvement. Every automation execution should log sufficient detail to recreate exactly what happened.

**Error Handling:** Robust error handling to gracefully manage unexpected conditions. Scripts should anticipate potential failure modes and include appropriate error recovery or escalation logic. For example, if a service restart command fails, the script might retry once before escalating to manual handling rather than repeatedly attempting the same failing operation.

**Parameterization:** Scripts should be configurable through parameters rather than hard-coded values. Parameters enable reuse across multiple scenarios, simplify testing, and facilitate tuning without code changes. Common parameters include retry counts, timeout values, threshold levels, and escalation contacts.

**Modularity:** Breaking complex automations into smaller, reusable functions promotes maintainability and testing. A disk cleanup script might include separate functions for identifying files eligible for deletion, validating sufficient permissions, executing deletion, and verifying space recovery.

### Phase 4: Test Automation

Thorough testing is essential before deploying automation into production. Testing validates both functional correctness (does the automation do what it's supposed to do?) and safety (does the automation fail gracefully without causing harm?).

**Development/Test Environment Testing:** Initial testing occurs in non-production environments that mirror production configurations. Testing validates that the automation executes successfully under normal conditions and achieves the desired outcome.

**Failure Mode Testing:** Testing must include scenarios where remediation fails or unexpected conditions occur. Does the automation handle errors gracefully? Does it escalate appropriately? Does it avoid causing additional problems when remediation fails?

**Safety Control Testing:** Explicitly validate that safety controls operate as designed. Test that throttling limits are enforced, blackout periods prevent execution, and rollback mechanisms successfully reverse changes when needed.

**Performance Testing:** Ensure automation completes within acceptable timeframes and does not consume excessive system resources. Automation that resolves an issue but causes performance degradation is counterproductive.

**Documentation Review:** Validate that all documentation is complete, accurate, and sufficient for operational teams to understand, troubleshoot, and maintain the automation.

### Phase 5: Deploy to Production

Deployment introduces automation into the production environment using a controlled, phased approach to minimize risk:

**Pilot Deployment:** Initial deployment to a limited subset of CIs or a single data center. Pilot deployments enable real-world validation with limited blast radius. Monitor pilot automation closely, reviewing every execution for the first several days or weeks.

**Phased Rollout:** Gradual expansion to additional CIs, environments, or scenarios. Phased rollout allows identification and resolution of issues before full-scale deployment. Expansion decisions should be based on pilot success metrics, not calendar schedules.

**Production Monitoring:** Continuous monitoring of automation execution, success rates, and outcomes. Monitoring integrates with Activity 5.6: Perform Trend Analysis to track automation effectiveness over time.

**Deployment Documentation:** Update operational documentation, runbooks, and knowledge base articles to reflect the new automation. Ensure Event Analysts understand what automation exists, when it executes, and how to troubleshoot automation failures.

### Phase 6: Monitor and Optimize

Automation is not "set and forget." Continuous monitoring and optimization ensure automation remains effective as infrastructure, applications, and business requirements evolve.

**Success Rate Tracking:** Monitor the **Auto-operations Success Rate** KPI for this specific automation. Success rates below target (typically 95% or higher for individual automations) indicate the need for refinement. Analysis includes identifying common failure patterns, understanding why automation is failing, and determining whether failures indicate automation design issues or changing infrastructure conditions.

**Performance Analysis:** Track execution time, resource consumption, and impact on system performance. Automation that executes too slowly may not provide sufficient value. Resource-intensive automation may need optimization to reduce overhead.

**False Positive Detection:** Identify scenarios where automation executes unnecessarily or resolves events that were not actually problems. False positives waste resources and may mask genuine issues.

**Continuous Improvement:** Based on monitoring data, refine automation to improve success rates, expand applicability, or enhance efficiency. Improvements cycle back through the design, develop, test, and deploy phases following the same rigorous process as initial development.

The lifecycle's circular nature emphasizes that automation development is never truly complete. Monitoring feeds back to identification, where successful automations inspire similar automations for related scenarios, and automation failures identify opportunities for improvement or expansion of automation capabilities.

## Script Development and Testing

Effective automation requires well-engineered scripts that execute reliably, handle errors gracefully, and provide comprehensive logging for troubleshooting and audit purposes.

### Script Development Best Practices

**Idempotency:** Scripts should be idempotent, meaning they can be safely executed multiple times with the same outcome. For example, a script that restarts a service should first check whether the service is actually stopped. If the service is already running, the script should log this condition and exit successfully rather than attempting to start an already-running service (which could cause errors).

**Defensive Programming:** Anticipate unexpected conditions and handle them gracefully. Check that required files exist before attempting to read them. Verify that commands succeeded before proceeding to dependent steps. Validate input parameters before use. This defensive approach prevents minor anomalies from cascading into major failures.

**Clear and Comprehensive Logging:** Every significant action, decision, and outcome should be logged with sufficient detail for troubleshooting. Log entries should include timestamps, severity levels (INFO, WARNING, ERROR), and contextual information. Structured logging (JSON or key-value pairs) facilitates automated analysis and trending.

Example log entry:
```
2025-11-25 14:32:15 INFO [AutoRestartService] Event ID: EVT-2025-0042315
2025-11-25 14:32:15 INFO [AutoRestartService] CI: SERVER-PROD-001
2025-11-25 14:32:15 INFO [AutoRestartService] Service: ApplicationService
2025-11-25 14:32:15 INFO [AutoRestartService] Pre-check: Service status is STOPPED
2025-11-25 14:32:16 INFO [AutoRestartService] Action: Executing service start command
2025-11-25 14:32:18 INFO [AutoRestartService] Post-check: Service status is RUNNING
2025-11-25 14:32:18 INFO [AutoRestartService] Verification: Service responding to health check
2025-11-25 14:32:18 INFO [AutoRestartService] Result: SUCCESS
2025-11-25 14:32:18 INFO [AutoRestartService] Closure: Updating event status to Closed with code Auto Action
```

**Version Control and Code Review:** All automation scripts should be maintained in version control systems (Git, SVN) with proper branching strategies and change management. Code reviews by peer Event Designers or automation engineers catch errors, improve quality, and share knowledge across the team.

**Documentation as Code:** Maintain documentation alongside scripts in version control. Documentation should include the automation's purpose, trigger conditions, dependencies, known limitations, troubleshooting guidance, and version history. This approach ensures documentation remains synchronized with code and is available to anyone working with the automation.

### Testing Methodologies

**Unit Testing:** Test individual functions or script components in isolation. Unit tests validate that specific logic (file identification, permission checking, service status verification) operates correctly under various conditions. Automated unit testing enables rapid regression testing when scripts are modified.

**Integration Testing:** Test the complete automation end-to-end in test environments. Integration testing validates that all components work together correctly and that the automation integrates properly with the event management system, CI environment, and logging infrastructure.

**Scenario Testing:** Test specific scenarios including success cases, failure cases, and edge cases. Scenarios should be derived from the design specification and should cover both typical situations and unusual conditions.

Example test scenarios for a service restart automation:
- Service is stopped and restart succeeds (typical success case)
- Service is already running (edge case: automation should recognize this and exit gracefully)
- Service restart fails on first attempt but succeeds on retry (failure recovery case)
- Service restart fails after maximum retries (escalation case)
- CI is in maintenance mode (safety control case: automation should not execute)
- Another automation is currently executing on the same CI (concurrency case: automation should queue or skip)

**Load Testing:** For automations that may execute frequently or on many CIs simultaneously, validate that the automation can handle expected load without performance degradation or resource exhaustion. Load testing is particularly important for automations that interact with shared resources such as databases or network services.

**Security Testing:** Validate that automations do not introduce security vulnerabilities. Ensure scripts run with appropriate privileges (least privilege principle), validate inputs to prevent injection attacks, and protect sensitive data such as credentials or API keys. Security reviews should be conducted by information security teams before production deployment.

## Safety Controls

Automation without safety controls is dangerous. Well-designed safety mechanisms ensure automation resolves issues without creating new problems, maintains stability during failures, and provides operators with appropriate oversight and control.

![Figure 10.2: Automation Safety Controls](../assets/images/Automation Safety Controls.jpeg)

*Figure 10.2: Automation Safety Controls - This layered diagram illustrates the defense-in-depth approach to automation safety, with multiple control layers protecting against automation failures or unexpected consequences.*

### Throttling and Rate Limiting

Throttling prevents automation from executing too frequently or on too many CIs simultaneously. Without throttling, automation could overwhelm infrastructure during widespread issues, creating "thundering herd" problems where thousands of scripts execute simultaneously, consuming resources and potentially exacerbating the original problem.

**Per-CI Throttling:** Limit how frequently automation can execute on a specific CI. For example, a service restart automation might be limited to executing no more than three times per hour on any given server. If events continue to occur after throttling limits are reached, automation escalates to manual handling for investigation rather than repeatedly attempting the same failing remediation.

**Global Throttling:** Limit the total number of concurrent automation executions across the environment. Global throttling prevents automation from consuming excessive system resources during widespread events. For example, a disk cleanup automation might be limited to executing on no more than 50 servers simultaneously to prevent network or storage system overload.

**Adaptive Throttling:** Advanced implementations adjust throttling dynamically based on automation success rates or system load. If automation success rates drop below thresholds, adaptive throttling reduces execution frequency to prevent repeated failures. If system load is high, throttling reduces concurrent executions to preserve resources for critical operations.

### Blackout Periods

Blackout periods are time windows during which automation does not execute, even if trigger conditions are met. Blackout periods prevent automation from interfering with planned activities or executing during high-risk time windows.

**Maintenance Window Blackouts:** Automation is disabled during planned maintenance windows when systems may be in transitional states or when operational teams are actively making changes. Executing automation during maintenance could interfere with planned activities or generate false alerts.

**Business Critical Period Blackouts:** Organizations may define periods of high business criticality (end-of-quarter processing, peak sales periods, regulatory reporting windows) during which automation is restricted to minimize risk. During blackout periods, events are queued for manual handling or for automated handling after the blackout expires.

**Post-Change Blackouts:** After significant infrastructure or application changes, organizations may impose temporary blackout periods allowing systems to stabilize before resuming automated responses. This approach prevents automation from masking issues introduced by recent changes that require investigation and remediation rather than automated workarounds.

### Pre-Execution Validation

Before executing remediation actions, automation performs validation checks ensuring that execution is safe and appropriate:

**CI State Validation:** Check that the CI is in an expected state. Is the CI currently in service? Is it not in maintenance mode? Has it recently been modified? State validation prevents automation from executing on CIs where remediation might be inappropriate or risky.

**Dependency Checking:** Validate that dependent systems or services are available. For example, an automation that restarts an application service should verify that the database service (on which the application depends) is running before attempting the restart. Restarting the application service while the database is down will not resolve the issue and may mask the true problem.

**Recent Execution Checking:** Verify that the same automation has not recently been attempted. Repeated failures indicate a problem that requires investigation rather than continued automated remediation attempts. This check prevents automation from entering infinite loops where the same failing action is repeatedly executed.

**Concurrency Control:** Ensure that other automations are not currently executing on the same CI. Concurrent executions can interfere with each other, cause conflicts, or produce unpredictable results. Concurrency control queues automation executions or defers execution until existing automations complete.

### Rollback Capabilities

Rollback mechanisms enable automation to reverse changes when remediation fails or causes unexpected outcomes. Effective rollback requires careful design and implementation:

**State Capture:** Before executing remediation, capture the current state so it can be restored if needed. State capture might include configuration file backups, service status snapshots, or resource utilization baselines.

**Transaction-Style Execution:** When possible, implement remediation as transactions that can be committed (if successful) or rolled back (if unsuccessful). Transactional execution ensures that partial failures leave the system in a consistent state rather than an unknown or inconsistent state.

**Automatic Rollback Triggers:** Define conditions that automatically trigger rollback. Triggers might include verification failures (post-execution checks indicate the issue was not resolved), unexpected errors during execution, or violations of safety thresholds (resource utilization exceeded acceptable limits).

**Manual Rollback Capability:** Provide operators with the ability to manually trigger rollback if they identify issues that automated checks did not detect. Manual rollback interfaces should be clearly documented and readily accessible to Event Analysts and operational teams.

### Emergency Shutdown

Emergency shutdown mechanisms enable rapid disablement of automation when issues are detected. Every automation platform should include emergency shutdown capabilities:

**Individual Automation Disable:** Ability to disable specific automations without affecting others. If a particular automation is causing problems, operators should be able to immediately disable it while other automations continue operating normally.

**Category or Domain Disable:** Ability to disable all automations affecting a specific category of CIs (all database servers, all network devices) or a specific domain (a particular application or business service). Category disables are useful when widespread issues affect entire infrastructure segments.

**Global Emergency Stop:** Ability to disable all automation immediately. Global emergency stops are rarely needed but provide ultimate control when automation behavior is causing widespread problems. Global stops should require senior management approval and should be used only in genuine emergencies.

**Clear Re-Enable Procedures:** Emergency shutdowns must include clear procedures for re-enabling automation after issues are resolved. Re-enable procedures should include validation that the underlying issue is resolved, review of automation logs to understand what went wrong, and approval from appropriate management before resuming automated operations.

## Activity 3.5: Execute Automated Response

Activity 3.5: Execute Automated Response is the operational heart of Event Management automation, representing the moment when self-healing infrastructure resolves issues without human intervention. This activity occurs within Activity 3: Manage Event, specifically during the real-time processing phase where events are handled as they arrive.

### Activity Purpose and Context

The primary purpose of Activity 3.5 is to execute self-healing actions automatically resolving events without manual intervention. This activity directly supports Event Management's supporting goal to Maximize Automation, enabling organizations to handle high event volumes efficiently while freeing Event Analysts to focus on complex issues requiring human judgment.

Activity 3.5 executes only when specific conditions are met:

1. The event matches predefined automation trigger conditions configured during Activity 2: Create and Maintain EM Solutions
2. The event has not been filtered out as Informational during Activity 3.2: Filter Events
3. Pre-execution validation checks confirm that automated execution is safe and appropriate
4. No blackout periods or throttling limits prevent execution

### Execution Flow

When trigger conditions are met, the Event Management system initiates automated response following this sequence:

**Step 1: Pre-Execution Validation:** The system performs all configured pre-execution checks including CI state validation, recent execution checking, and concurrency control. If any validation fails, the automation halts and the event is routed to Activity 4: Correlate and Escalate Event for manual handling.

**Step 2: Execute Remediation Actions:** The automation executes the configured remediation actions, logging each step and capturing outcomes. Execution follows the script or workflow developed during the automation development lifecycle.

**Step 3: Post-Execution Verification:** After remediation completes, the automation performs verification checks confirming that the issue is resolved. Verification might include monitoring system status, checking for related events, or validating service availability. If verification fails, the automation may trigger rollback (if configured) and escalate to manual handling.

**Step 4: Update Event Status:** Upon successful verification, the automation updates the event status to `Closed` with closure code `Auto Action` as defined in Activity 5.4: Update Event Status. This status update signals that the event was successfully resolved through automation, contributing to the **Auto-operations Success Rate** KPI.

**Step 5: Generate Closure Confirmation:** The automation generates resolution confirmation feeding into Activity 5.1: Receive Resolution Confirmation. This confirmation includes complete details of what actions were taken and what outcomes were observed, supporting Activity 5.3: Document Event Outcome.

### Integration with Event Management Process

Activity 3.5 integrates tightly with other Event Management activities:

**Input from Activity 2:** Automation trigger conditions, scripts, safety controls, and validation rules are all configured during Activity 2: Create and Maintain EM Solutions. Event Designers define exactly which events trigger which automations and under what conditions.

**Input from Activity 3.1-3.3:** Events reaching Activity 3.5 have already been detected (Activity 3.1), filtered (Activity 3.2), and categorized (Activity 3.3). Automation operates on validated, categorized events rather than raw, unfiltered alerts.

**Output to Activity 5:** Successful automation generates resolution confirmation (Activity 5.1), which then flows through verification (Activity 5.2), documentation (Activity 5.3), and status update (Activity 5.4) to complete the event lifecycle. Automation execution data contributes to trend analysis (Activity 5.6) and performance reporting (Activity 5.9).

**Escalation to Activity 4:** When automation fails, cannot execute due to validation failures, or encounters unexpected conditions, events are escalated to Activity 4: Correlate and Escalate Event for human handling. This escalation ensures that issues are not ignored when automation cannot resolve them.

### Governance and Control

Activity 3.5 operates under governance controls ensuring safe and effective automation:

**Policy 2: Alert Logging** mandates that automated actions are logged with complete detail including timestamp, event information, actions taken, and resolution outcome. This logging supports audit requirements and continuous improvement.

**Control Objective EM-C08** requires Event Management to provide trend analysis reports including automation success rates. This requirement ensures that automation effectiveness is regularly monitored and reported to stakeholders.

**Safety Controls** including throttling, blackout periods, and rollback capabilities operate during Activity 3.5 execution, preventing automation from causing harm or executing inappropriately.

## Verification of Automation Success

Determining whether automation succeeded or failed is not always straightforward. Robust verification mechanisms ensure that only genuinely successful automations are recorded as successes, preventing false positives that mask underlying issues.

### Verification Methods

**Status Monitoring:** The most basic verification method queries the status of the affected CI or service. For example, after a service restart automation executes, the script queries the service status to confirm it is running. While simple, status monitoring may be insufficient for complex scenarios where a service is running but not functioning correctly.

**Health Check Validation:** More sophisticated verification includes executing health checks or synthetic transactions confirming that the service is not only running but also responding correctly. A health check might send a test request to an application and verify that it responds with the expected result within acceptable timeframes.

**Event Monitoring:** Verification can monitor for absence of related events during a defined observation period. After executing remediation, the automation waits (typically 5 to 15 minutes) and confirms that the original event does not recur and no related error events appear. Absence of events during the observation window indicates successful remediation.

**Metric Validation:** Verification can check that relevant metrics have returned to acceptable ranges. For example, after executing disk space cleanup, the automation verifies that disk utilization has dropped below warning thresholds. After restarting a connection pool, the automation verifies that connection counts are within normal ranges.

**Multi-Layer Verification:** The most robust verification combines multiple methods. An automation might verify service status, execute a health check, and monitor for events over an observation period before declaring success. Multi-layer verification reduces false positives but increases verification time and complexity.

### Handling Verification Failures

When verification indicates that remediation failed, automation must respond appropriately:

**Retry Logic:** For transient issues, automation may retry remediation a configured number of times before declaring failure. Retry delays should increase (exponential backoff) to avoid overwhelming systems with rapid retry attempts. Typically, automations retry once or twice before escalating to manual handling.

**Rollback Execution:** If configured, failed verification triggers automatic rollback to restore the pre-remediation state. Rollback is particularly important for configuration changes or operations that could leave systems in unstable states.

**Escalation to Manual Handling:** After retry exhaustion or when verification clearly indicates a problem that automation cannot resolve, the event escalates to Activity 4: Correlate and Escalate Event for human investigation. Escalation includes complete details of automation attempts, actions taken, and verification results to assist Event Analysts in troubleshooting.

**Enhanced Logging:** Verification failures trigger enhanced logging capturing additional diagnostic information useful for troubleshooting. Enhanced logging might include detailed error messages, system state snapshots, or dependency status information.

## Auto Action Closure Code

When automation successfully resolves an event, the event is closed with closure code `Auto Action` as defined in Activity 5.4: Update Event Status. This closure code is one of seven mandatory closure codes used to classify event resolution methods, enabling performance tracking and trend analysis.

### Auto Action Closure Code Criteria

An event receives closure code `Auto Action` when:

1. Automated response executed successfully without human intervention
2. Post-execution verification confirmed that the issue is resolved
3. The event did not escalate to Incident, Problem, or Change Management
4. The event reached status `Closed` directly from automated handling

### Importance for Metrics and Reporting

The `Auto Action` closure code is essential for calculating the **Auto-operations Success Rate** KPI:

**Formula:** (Events Closed as Auto Action / Total Events Requiring Response) × 100

This KPI measures automation effectiveness and is reported in Activity 5.9: Report on Event Management Performance. The target for mature implementations is greater than or equal to 70%, meaning that 70% or more of events requiring response are successfully resolved through automation without human intervention.

The `Auto Action` closure code also supports:

**Trend Analysis (Activity 5.6):** Tracking which event types are most successfully automated, identifying opportunities for expanding automation, and detecting degradation in automation success rates that might indicate infrastructure changes or automation failures.

**Cost Benefit Analysis:** Quantifying the operational cost savings from automation by tracking the volume of events resolved automatically versus those requiring manual handling. This data supports business cases for continued automation investment.

**Maturity Assessment:** Organizations progress through maturity levels as their **Auto-operations Success Rate** increases. Level 3 (Defined/Standardized) implementations typically achieve 50-70% automation success rates, while Level 4 (Measured/Managed) and Level 5 (Optimized) implementations exceed 70%.

### Audit and Compliance

Events closed with `Auto Action` maintain complete audit trails documenting:

- Exact actions taken by automation
- Timestamp of execution
- Pre-execution validation results
- Post-execution verification results
- Any errors or warnings encountered
- System or script logs supporting the resolution

This documentation supports Policy 2: Alert Logging requirements and provides evidence for compliance audits demonstrating that automated operations are properly controlled and monitored.

## Auto-Operations Success Rate KPI

The **Auto-operations Success Rate** is a critical KPI measuring Event Management automation effectiveness. This metric quantifies the percentage of events successfully resolved through automated responses without human intervention.

### KPI Definition

**Metric Name:** Auto-operations Success Rate
**Category:** Efficiency
**Target:** Greater than or equal to 70% (for mature implementations)
**Formula:** (Events Closed as Auto Action / Total Events Requiring Response) × 100

The denominator (Total Events Requiring Response) includes all events except those filtered as Informational during Activity 3.2. Events that require response include Warning events and Exception events regardless of whether they are ultimately handled through automation, manual operations, or escalation to other processes.

### Interpretation and Benchmarks

**0-30% (Initial):** Organizations at this level have minimal automation, relying primarily on manual event handling. This range is typical for Level 1 (Reactive/Initial) maturity.

**30-50% (Developing):** Organizations are developing automation capabilities with automated responses for common, straightforward scenarios. This range is typical for Level 2 (Managed/Repeatable) maturity transitioning toward Level 3.

**50-70% (Mature):** Significant automation coverage with automated responses for most routine events. Human intervention is reserved for complex issues, investigation-requiring events, or scenarios outside automation scope. This range is typical for Level 3 (Defined/Standardized) maturity.

**Greater than 70% (Optimized):** Advanced automation with self-healing infrastructure resolving the majority of events automatically. This range is typical for Level 4 (Measured/Managed) and Level 5 (Optimized/Continuous Improvement) maturity levels.

### Factors Affecting Auto-Operations Success Rate

**Automation Coverage:** The breadth of automation (how many different event types have automated responses) directly impacts the success rate. Organizations with automation for only a few event types will have lower success rates than organizations with comprehensive automation coverage.

**Infrastructure Stability:** Stable infrastructure with predictable failure patterns enables higher automation success rates. Unstable or rapidly changing infrastructure may experience novel issues that automation cannot handle, reducing success rates.

**Automation Quality:** Well-designed, thoroughly tested automation with appropriate safety controls achieves higher success rates than hastily developed automation. Quality includes robust error handling, comprehensive verification, and appropriate escalation logic.

**Event Mix:** The types of events an organization experiences affect achievable success rates. Organizations experiencing primarily routine, predictable events can achieve higher automation success rates than organizations dealing with diverse, complex events requiring human analysis.

### Using the KPI for Continuous Improvement

Tracking **Auto-operations Success Rate** over time reveals trends and improvement opportunities:

**Declining Success Rates:** May indicate infrastructure changes that broke existing automation, increasing frequency of events outside automation scope, or automation quality issues. Declining trends trigger Activity 5.8: Update Event Management Solutions to refine or expand automation.

**Plateau Effect:** Success rates reaching a plateau suggest that existing automation candidates have been fully exploited. Further improvement requires identifying new automation candidates or refining marginal scenarios to increase success rates incrementally.

**Variation by Event Type:** Analyzing success rates by event category or CI type identifies strengths and weaknesses in automation coverage. Low success rates for specific event types highlight opportunities for new automation development.

The KPI is reported in Activity 5.9: Report on Event Management Performance with daily operational dashboards showing current status, weekly tactical reports showing trends, and monthly executive reports comparing performance against targets and explaining significant variations.

## CSF 8: Balanced Automation

Critical Success Factor 8: Balanced Automation establishes the strategic principle that automation must be balanced with appropriate human oversight and control. This CSF recognizes that while automation delivers substantial benefits, over-automation or poorly controlled automation can introduce significant risks.

### Principles of Balanced Automation

**Appropriate Automation Scope:** Not all events should be automated. Balanced automation focuses automation efforts on scenarios where automation provides clear value and where risks are manageable. Complex diagnostics, high-risk operations, and ambiguous scenarios remain under human control.

**Maintained Human Oversight:** Even highly automated environments require human oversight. Event Analysts monitor automation execution, review automation logs, investigate automation failures, and make decisions about expanding or constraining automation scope. Organizations should never implement "lights out" operations where automation runs completely unsupervised.

**Safety Controls and Governance:** Balanced automation includes comprehensive safety controls (throttling, blackout periods, validation, rollback) ensuring automation fails safely. Governance processes review automation decisions, approve new automations, and maintain appropriate controls over automation behavior.

**Continuous Validation:** Automation that worked reliably in the past may become ineffective or inappropriate as infrastructure, applications, or business requirements change. Balanced automation includes continuous monitoring (Phase 6 of the Automation Development Lifecycle) and regular reviews ensuring automation remains appropriate and effective.

### Risks of Imbalanced Automation

**Over-Automation:** Automating too aggressively without sufficient testing or safety controls can lead to automation causing or exacerbating problems rather than resolving them. Over-automation may mask underlying issues that require fundamental resolution rather than automated workarounds.

**Under-Automation:** Failing to automate appropriate scenarios leaves organizations dependent on manual operations that are slower, less consistent, and more expensive than automated alternatives. Under-automation limits scalability and prevents organizations from achieving mature Event Management capabilities.

**Hidden Dependency:** Over-reliance on automation can create hidden dependencies where operational teams lose the knowledge and skills to manually handle issues when automation fails. Balanced automation maintains documentation and training ensuring teams can operate manually when necessary.

**Automation Debt:** Just as software development accumulates technical debt, automation can accumulate "automation debt" where hastily developed automations lack proper documentation, testing, or maintainability. This debt eventually requires repayment through refactoring, documentation, or replacement.

### Achieving Balanced Automation

**Structured Development Lifecycle:** Following the six-phase Automation Development Lifecycle (Identify, Design, Develop, Test, Deploy, Monitor) ensures automations are properly designed, tested, and deployed with appropriate controls.

**Graduated Automation:** Implementing automation progressively allows organizations to develop expertise and confidence before tackling more complex scenarios. Start with simple, low-risk automations, demonstrate success, and gradually expand to more sophisticated scenarios.

**Transparent Operations:** Automation should be transparent with clear logging, monitoring, and reporting enabling operators to understand what automation is doing and why. Transparency supports troubleshooting, builds confidence, and enables continuous improvement.

**Regular Review and Refinement:** Balanced automation includes regular reviews (Activity 5.7: Review Event Effectiveness) assessing automation quality, identifying false positives, and refining automation to maintain effectiveness. Reviews should involve both technical teams (Event Designers) and operational teams (Event Analysts).

CSF 8: Balanced Automation ensures that the pursuit of automation efficiency does not compromise operational stability, service quality, or organizational capability to respond to unexpected scenarios requiring human judgment and expertise.

## Common Automation Failures and Mitigation

Even well-designed automation encounters failures. Understanding common failure patterns and implementing appropriate mitigation strategies ensures automation remains reliable and effective.

### Failure Pattern 1: Environment Drift

**Description:** Automation that worked reliably begins failing as infrastructure configurations change. Applications are updated with new dependencies, server configurations are modified, or network topologies evolve, causing automation to encounter unexpected conditions.

**Symptoms:** Declining automation success rates over time, specific automations that previously succeeded now consistently failing, automation errors indicating missing files or services.

**Mitigation:**
- Implement configuration management ensuring infrastructure remains consistent with automation expectations
- Include environment validation in pre-execution checks detecting configuration changes before executing remediation
- Maintain automation in version control alongside infrastructure-as-code enabling synchronized updates
- Establish change management integration where infrastructure changes trigger automation review and updates

### Failure Pattern 2: Insufficient Error Handling

**Description:** Automation encounters unexpected errors and fails ungracefully, leaving systems in unknown or inconsistent states. Scripts may terminate abruptly without cleanup, rollback, or appropriate escalation.

**Symptoms:** Events closed as `Auto Action` but issues persist, CIs in abnormal states after automation execution, incomplete or missing log entries for automation execution.

**Mitigation:**
- Implement comprehensive error handling in all automation scripts catching exceptions and handling them appropriately
- Include cleanup logic ensuring temporary files, locks, or resources are released even when errors occur
- Design scripts with transaction-style execution enabling rollback when errors prevent successful completion
- Enhance logging during error conditions capturing additional diagnostic information useful for troubleshooting

### Failure Pattern 3: Verification False Positives

**Description:** Automation's verification logic incorrectly reports success when remediation actually failed. This pattern is particularly insidious because it masks issues that continue to affect services or users.

**Symptoms:** Events closed as `Auto Action` but users continue reporting problems, recurring events shortly after automation declares success, service degradation persisting despite automation reporting successful resolution.

**Mitigation:**
- Implement multi-layer verification combining multiple validation methods (status checks, health checks, event monitoring, metric validation)
- Extend verification observation windows allowing sufficient time for issues to manifest before declaring success
- Include synthetic transactions in verification testing actual functionality rather than only checking status
- Implement feedback loops where user-reported issues trigger automation review and verification enhancement

### Failure Pattern 4: Resource Exhaustion

**Description:** Automation consumes excessive system resources (CPU, memory, network bandwidth, disk I/O) causing performance degradation or triggering additional events. Without proper resource controls, automation can become part of the problem rather than the solution.

**Symptoms:** Performance degradation coinciding with automation execution, automation triggering additional events related to resource utilization, operations teams reporting system slowness during automation execution.

**Mitigation:**
- Implement resource limits on automation processes (CPU quotas, memory limits, I/O throttling)
- Use global throttling limiting the number of concurrent automation executions across the environment
- Schedule resource-intensive automation during off-peak hours per Policy 7: Trend Monitoring
- Monitor automation resource consumption and optimize scripts to reduce overhead

### Failure Pattern 5: Dependency Failures

**Description:** Automation depends on external systems, services, or data that become unavailable or return unexpected results. Dependencies might include CMDB queries, API calls to management systems, or access to shared resources.

**Symptoms:** Automation fails with errors indicating connectivity problems, timeout errors, or missing data, automation success rates correlating with availability of dependent systems.

**Mitigation:**
- Implement dependency checking in pre-execution validation verifying that required systems are available before attempting remediation
- Include timeout and retry logic with appropriate backoff strategies for dependent service calls
- Design automation to degrade gracefully when optional dependencies are unavailable (logging the condition and proceeding with reduced functionality)
- Establish monitoring for dependent systems ensuring issues are detected and resolved proactively

### Failure Pattern 6: Timing and Race Conditions

**Description:** Automation fails due to timing issues such as services not fully restarting before verification checks execute, concurrent operations interfering with each other, or time-dependent conditions changing between validation and execution.

**Symptoms:** Intermittent automation failures with no clear cause, failures occurring more frequently during high-load periods, automation succeeding when executed manually but failing when executed automatically.

**Mitigation:**
- Implement appropriate wait periods between remediation actions and verification allowing services time to stabilize
- Use polling with timeout rather than fixed delays checking repeatedly until conditions are met or timeout occurs
- Implement concurrency controls preventing simultaneous automation execution on the same CI
- Add sufficient logging to identify timing-related issues enabling diagnosis and optimization

### Learning from Failures

Automation failures are opportunities for improvement. Implement systematic failure analysis:

**Failure Trend Analysis:** Track automation failures by type, CI, time of day, and other dimensions. Patterns in failure data reveal systemic issues requiring attention.

**Root Cause Investigation:** Conduct lightweight root cause investigation for recurring automation failures. Understanding why automation fails enables targeted improvements rather than generic "better error handling."

**Knowledge Base Updates:** Document common failure scenarios and their resolutions in the knowledge base. This documentation assists Event Analysts in troubleshooting automation failures and informs future automation design.

**Automation Refactoring:** Based on failure analysis, refactor automation to address identified weaknesses. Refactoring cycles through the Automation Development Lifecycle phases (Design, Develop, Test, Deploy) ensuring improvements are properly validated.

## Key Takeaways

- **Automation transforms Event Management** from reactive to proactive operations, reducing Mean Time to Detect and Mean Time to Resolve while improving consistency and enabling scalability. Organizations with mature automation resolve 70% or more of events automatically without human intervention.

- **The Automation Development Lifecycle** provides a structured approach ensuring automation is properly designed, developed, tested, and deployed. The six phases (Identify, Design, Develop, Test, Deploy, Monitor) mirror software development best practices while incorporating Event Management-specific considerations.

- **Not all events should be automated.** Successful automation programs begin with careful candidate selection prioritizing high-volume, low-complexity scenarios with clear resolution procedures and low risk. Complex diagnostics, high-risk operations, and ambiguous scenarios remain under human control.

- **Safety controls are essential.** Throttling, blackout periods, pre-execution validation, rollback capabilities, and emergency shutdown mechanisms ensure automation fails safely and maintains stability. Automation without safety controls is dangerous and can cause more harm than good.

- **Activity 3.5: Execute Automated Response** represents the operational heart of automation, executing self-healing actions and updating event status to `Closed` with closure code `Auto Action` upon successful verification. This activity integrates tightly with other Event Management activities, receiving configuration from Activity 2 and feeding outcomes to Activity 5.

- **The Auto-operations Success Rate KPI** measures automation effectiveness with a target of greater than or equal to 70% for mature implementations. This metric tracks the percentage of events successfully resolved through automation and serves as a key indicator of Event Management maturity progression.

- **CSF 8: Balanced Automation** emphasizes that automation must be balanced with appropriate human oversight and control. Over-automation without sufficient controls creates risks, while under-automation limits scalability and prevents organizations from achieving mature capabilities.

- **Common automation failures** including environment drift, insufficient error handling, verification false positives, resource exhaustion, dependency failures, and timing issues have well-understood mitigation strategies. Learning from failures and implementing systematic improvements ensures automation remains reliable and effective.

## Summary

Automation and self-healing capabilities represent the difference between reactive and proactive Event Management. Organizations beginning their Event Management journey rely heavily on manual operations with Event Analysts responding to each event individually. As maturity increases, automation progressively handles more events, freeing operational teams to focus on complex issues, strategic improvements, and proactive initiatives.

The journey to automation excellence is progressive and structured. It begins with careful identification of automation candidates using systematic evaluation criteria prioritizing scenarios where automation delivers maximum value with minimal risk. The Automation Development Lifecycle provides the framework for developing high-quality automation following proven phases: Identify, Design, Develop, Test, Deploy, and Monitor. Each phase includes specific activities, quality checks, and governance ensuring automation meets organizational standards for safety, reliability, and maintainability.

Safety controls form the foundation of responsible automation. Throttling and rate limiting prevent automation from overwhelming infrastructure during widespread events. Blackout periods protect critical business periods and maintenance windows. Pre-execution validation ensures automation executes only when safe and appropriate. Rollback capabilities enable recovery when remediation fails or causes unexpected outcomes. Emergency shutdown mechanisms provide ultimate control when automation behavior becomes problematic. Together, these controls ensure that automation resolves issues without creating new problems.

Activity 3.5: Execute Automated Response transforms automation design into operational reality. This activity executes during real-time event processing, applying configured automation rules to eligible events, executing remediation actions, verifying success, and updating event status with the `Auto Action` closure code. Successful automation generates complete documentation supporting audit requirements and continuous improvement while contributing to the Auto-operations Success Rate KPI that measures automation effectiveness.

CSF 8: Balanced Automation reminds us that the goal is not maximum automation but optimal automation. Automation should enhance rather than replace human judgment. Organizations achieve balance through graduated implementation starting with simple scenarios and progressively expanding, maintaining transparency through comprehensive logging and monitoring, and preserving human oversight ensuring operational teams understand what automation does and why. This balance enables organizations to achieve high automation rates (70% or more) while maintaining the capability to respond effectively when automation cannot resolve issues or when unexpected scenarios require human expertise.

As we move to Chapter 11, we will explore how Event Management integrates with other IT Service Management processes through escalation and handoff procedures. While automation handles the majority of routine events, those requiring human intervention must be routed to appropriate processes—Incident Management for service disruptions, Problem Management for recurring issues, or Change Management for configuration adjustments. Understanding these integration points and escalation criteria ensures that events receive appropriate attention regardless of whether they are resolved through automation or require human involvement.

## Review Questions

1. **Identify three benefits of automation and explain how each contributes to Event Management effectiveness.** Consider operational efficiency, consistency, speed, cost reduction, and scalability. How do these benefits support the goal to Maximize Automation?

2. **What criteria should be used to evaluate whether an event is a good automation candidate?** Apply the evaluation framework from Table 10.1 to a specific example such as service restarts, disk space management, or connection pool resets. Which criteria are most important and why?

3. **Describe the six phases of the Automation Development Lifecycle and explain why each phase is essential.** What risks might arise from skipping phases or rushing through the lifecycle? How does the lifecycle ensure automation quality and safety?

4. **Explain three different types of safety controls and provide examples of each.** How do throttling, blackout periods, pre-execution validation, rollback capabilities, and emergency shutdown work together to create defense-in-depth protection?

5. **How is the Auto-operations Success Rate KPI calculated, and what does it measure?** What success rate is typical for organizations at different maturity levels? How should organizations interpret changes in this KPI over time?

---
**Chapter 10 References**

ITIL Foundation, ITIL 4 Edition. AXELOS Limited, 2019.

Event Management Handbook Source Material: Part III Technical Implementation, Event Management Process Activities, Event Management Policies and Governance.

---

## Chapter Navigation

| Previous | Next |
|----------|------|
| [Chapter 9: Event Correlation](/EventManagementHandbook/chapters/09-event-correlation/) | [Chapter 11: ITSM Integration](/EventManagementHandbook/chapters/11-itsm-integration/) |

---
