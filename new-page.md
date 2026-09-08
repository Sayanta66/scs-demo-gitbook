---
description: >-
  Use the Zero Day Agent to detect and remediate zero-day vulnerabilities across
  your software supply chain.
tags:
  - harness-scs
  - zero-day-agent
  - ai-security
title: Zero Day Agent
sidebar_position: 10
nodeTitle: Zero Day Agent
inputFilePath: docs/software-supply-chain-assurance/agents/zero-day-agent.md
originalUrl: >-
  https://developer.harness.io/docs/software-supply-chain-assurance/agents/zero-day-agent/
---

# Zero Day Agent

SCS helps you identify vulnerable open source software (OSS) dependencies across your software supply chain through continuous SBOM analysis. However, remediating these dependencies is often a manual and time-consuming process. Security and development teams must identify affected repositories, determine a safe dependency version, create pull requests, and verify that the fix has been applied. During critical events such as zero-day vulnerabilities, this process can delay remediation across multiple repositories.

To address this challenge, Harness continuously monitors and validates zero-day vulnerabilities, backed by our Security Research Team. Once a zero-day vulnerability is confirmed, the Zero Day Agent automatically identifies affected assets and triggers the remediation workflow. This accelerates the remediation process across your software supply chain and reduces the time and manual effort required to respond to critical security events.

{% hint style="info" %}
The Zero Day Agent is not a worker agent. It is a pre-built SCS agent designed specifically to respond to zero-day vulnerabilities.
{% endhint %}

***

### What will you learn in this topic? <a href="#what-will-you-learn-in-this-topic" id="what-will-you-learn-in-this-topic"></a>

By the end of this topic, you will be able to:

* Understand how the Zero Day Agent works in SCS.
* Configure the agent’s notifications, connectors, and severity filter.
* Choose the infrastructure on which the agent runs.
* View agent executions and findings details.
* Create remediation pull requests for affected repositories.

***

### Before you begin <a href="#before-you-begin" id="before-you-begin"></a>

Make a note of the following before you proceed with Zero Day agent configuration:

* Ensure your SCM provider is integrated with the platform to generate SBOMs for your code repositories. You can do this in one of the following ways:
  * Repository onboarding through RSPM currently supports GitHub and Bitbucket:
    * To integrate your GitHub account and onboard the repositories, go to [Onboard GitHub Repositories](../open-source-management/integrations/github.md).
    * To integrate your Bitbucket account and onboard the repositories, go to [Onboard Bitbucket Repositories](../open-source-management/integrations/bitbucket.md).
  * SBOMs can be generated through pipeline execution. Go to [Generate SBOM for Repositories](../open-source-management/generate-sbom-for-repositories.md) to generate SBOM through pipeline execution.
* Ensure that SBOMs are generated for your artifacts. Go to [Generate SBOM for artifacts](../open-source-management/generate-sbom-for-artifacts.md) to generate them.

***

### Understand the Zero Day Agent <a href="#understand-the-zero-day-agent" id="understand-the-zero-day-agent"></a>

The Zero Day Agent is a pre-built SCS agent that automatically responds to zero-day vulnerabilities identified by the Harness Security Research Team. Once enabled at the Account scope, the agent analyzes the affected component against the SBOMs generated for your repositories and artifacts to determine its impact across your software supply chain. It identifies affected repositories and artifacts. The agent can also discover newly detected vulnerabilities that may not yet be identified by traditional SCA scanners, helping you respond to vulnerabilities that could otherwise be detected later. For eligible repositories, the agent provides the recommended dependency update and lets you create a remediation pull request.

The agent also provides visibility throughout the remediation workflow. Each run records the packages analyzed, affected repositories and artifacts, and remediation status. The agent performs component analysis to provide details about the affected packages and their security relevance. It also sends notifications with information about the detected vulnerability, affected packages, impacted targets, and available remediation actions. This gives security and development teams visibility into the impact of a zero-day vulnerability and the progress of its remediation.

The following table provides a structured overview of the Zero Day Agent, when to use it, and how it helps automate the response to zero-day vulnerabilities across your software supply chain.

| Why use it?                                                                                                                                                                                                                                        | When to use?                                                                                                                                                                                                                                                                                                                  | How can you leverage it?                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>1. Automate dependency remediation across repositories.<br>2. Reduce the time required to respond to security events.<br>3. Standardize remediation across your software supply chain.<br>4. Improve remediation visibility and governance.</p> | <p>1. When you need to remediate vulnerable or risky OSS dependencies.<br>2. During zero-day vulnerabilities or newly disclosed security issues.<br>3. When managing applications distributed across multiple repositories and artifacts.<br>4. When security and development teams need to monitor remediation progress.</p> | <p>1. Detects affected repositories, creates remediation pull requests, and validates the applied fixes.<br>2. Automatically identifies impacted dependencies and provides the dependency updates specified in the security advisory for remediation when the agent is enabled after configuration.<br>3. Provides a consistent remediation workflow and provides centralized execution tracking.<br>4. Records execution details, generated pull requests, and remediation status for every run.</p> |

***

### Configure the Zero Day Agent <a href="#configure-the-zero-day-agent" id="configure-the-zero-day-agent"></a>

Configuring the Zero Day agent lets you control how it notifies you, which severities it acts on, and where its executions run. It is available only at the **Account** scope. To configure the Zero Day Agent in SCS, complete the following steps:

1. [Configure the notifications](zero-day-agent.md#step-1---configure-the-notifications)
2. [Set up the infrastructure](zero-day-agent.md#step-2---set-up-the-infrastructure)

#### Step 1 - Configure the notifications <a href="#step-1-configure-the-notifications" id="step-1-configure-the-notifications"></a>

Configuring notifications within the agent can display an in-app banner and send notifications to your messaging tools when it detects risky dependencies. To configure notifications, complete the following steps:

1. Navigate to the **Agents** page from the sidebar navigation of your SCS account. The page lists the prebuilt agents available in your account. Each agent is displayed as a card.
2.  Click the **Zero Day Agent** card to open the agent. The **Details** tab opens by default.

    <figure><img src="../../.gitbook/assets/zero-day-remediation-agent-details.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
3. Select the **Configurations** tab to open the **Notifications** and **Infrastructure** configuration panels.
4. Within the **Notifications** configuration panel, under **Connectors**, click **+ Add Connector** to open the `Select Notification Channel` dialog box.
5. Select your preferred notification channel for connector configuration. The available notification channel is **Slack**. Support for **Microsoft Teams** and **Google Chat** will be available in a future release.
6.  Click **Add** to add the notification channel and configure its connector.

    <div data-gb-custom-block data-tag="hint" data-style="info" class="hint hint-info"><p>You can add more than one connector for multiple notification channels.</p></div>
7. Click **Select** beside your selected notification channel to open the `Create or Select an Existing Connector` dialog.
8. Select your required connector from the list of existing connectors. You can search for your created connector or filter connectors by **Project**, **Organization**, and **Account**.
9. Alternatively, click **+ New Connector** to create a new connector for your selected notification channel.
10. Enter the Slack channel ID where you want the Zero Day Agent to send notifications.\
    You can enter multiple channel IDs.
11. Under **Severity Filter**, select one or multiple checkboxes beside the severities for which you want the agent to deliver notifications. The available options are **Critical**, **High**, **Medium**, and **Low**.

    <figure><img src="../../.gitbook/assets/agent-notification-connectors-severity.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

#### Step 2 - Set up the infrastructure <a href="#step-2-set-up-the-infrastructure" id="step-2-set-up-the-infrastructure"></a>

The infrastructure setting determines where the agent's executions run. To configure the infrastructure, complete the following steps:

1. Within the **Infrastructure** configuration panel, select the execution environment where the agent runs. The available options are:
   * **Harness Cloud** - Select this option to run the agent in a Harness-hosted environment.
     * Linux is selected as the default operating system for running executions in Harness Cloud.
     * Select your preferred architecture from the dropdown to run the executions. The available options are **ARM64** and **AMD64**.
   * **Kubernetes** - Select this option to run the agent in a specific Kubernetes cluster and namespace.
     * Linux is selected as the default operating system for running the agent in Kubernetes.
     * Click **Select Kubernetes Cluster** under **Kubernetes Cluster** to open the `Create or Select an Existing Connector` dialog.
     * Select your required connector from the list of existing connectors. You can search for your created connector or filter connectors by **Project**, **Organization**, and **Account**.
     * Alternatively, click **+ New Connector** to create a new Kubernetes cluster connector for connecting your existing Kubernetes clusters with Harness. Go to [Add a Kubernetes cluster connector](https://app.gitbook.com/s/3F2TpHXhur2QtQnORSM9/use-harness-platform/connectors/cloud-providers/add-a-kubernetes-cluster-connector#add-a-kubernetes-cluster-connector) to create one.
     * Enter the namespace in your Kubernetes cluster where you want the agent to run.
2.  After verifying the details, click **Save** on the top right corner.\
    Once saved, you can view the **Configuration saved successfully** toaster message at the top, indicating that the agent notification and infrastructure settings have been configured successfully.

    <figure><img src="../../.gitbook/assets/agent-infrastructure.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

***

### Enable the Zero Day Agent <a href="#enable-the-zero-day-agent" id="enable-the-zero-day-agent"></a>

The Zero Day Agent is enabled at the **Account** scope and automatically responds to zero-day vulnerabilities. When a new zero-day vulnerability is identified, the agent analyzes the affected component, identifies all affected repositories and artifacts, and sends notifications through the configured notification channel before initiating the remediation workflow.

{% hint style="info" %}
When you enable the Zero Day Agent, SCS automatically creates a template pipeline named **ZERO\_DAY** in the **default\_project** under the **default organization**. The agent uses this pipeline to run its executions.
{% endhint %}

To enable the agent, complete the following steps:

1. Navigate to the **Agents** page from the sidebar navigation of your SCS account.
2. Enable the agent using the toggle on the agent card or the toggle in the upper-right corner of the agent page.\
   Once enabled, the agent automatically responds to zero-day vulnerabilities identified by the Harness Security Research Team and runs across your software supply chain without requiring any user intervention.
3. Once a run is detected, the agent sends an alert notification to the **Alerts** panel.\
   The agent also sends a notification through the configured notification channel.
4.  Select **Alerts** from the sidebar navigation to open the **Alerts** panel and view the alert notifications. Go to [Platform Alerts](https://app.gitbook.com/s/3F2TpHXhur2QtQnORSM9/use-harness-platform/notifications-alerts-and-banners/platform-alerts) to configure alert rules.

    <figure><img src="../../.gitbook/assets/agent-alert-notification.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
5.  Click any notification to view the notification details.\
    The notification includes the **severity of the detected zero-day vulnerability**, **the number and details of affected packages**, **total affected targets**, **organization**, **project**, and a **link** to view the corresponding **execution**.

    <figure><img src="../../.gitbook/assets/alert-details.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
6. Click **View Execution** to review the execution details for the detected zero-day vulnerability.

***

### View the agent executions <a href="#view-the-agent-executions" id="view-the-agent-executions"></a>

After the agent runs, each execution is recorded in the **Executions** tab. You can view the executions at both the **Account** and **Project** scopes, review the affected packages and repositories, and monitor the status of each execution. To view agent executions, complete the following steps:

1. Navigate to the **Agents** page under the **Manage** section from the sidebar navigation of your SCS account.
2. Click the **Zero Day Agent** card to open the agent. The **Details** tab opens by default.
3.  Select the **Executions** tab to open the table of affected packages.\
    The table lists each detected package with its **Ecosystem**, **Severity**, **Affected Version**, **Fixed Version**, **Targets Impacted**, **Discovered At**, and **Agent Status**. Use the search bar to find a specific package.

    <figure><img src="../../.gitbook/assets/agent-executions.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
4. Click a package row to open the **Execution Details** side panel. The **Findings** tab opens by default.
5. In the **Details** section, review the package details including the name, ecosystem, affected versions, severity, description with the associated CVE, and detection time.
6.  In the **Affected Targets** section, review the repositories and artifacts where the package was detected, along with the **detected version**, **project**, **deployed environment**, and the **current remediation status**.

    <div data-gb-custom-block data-tag="hint" data-style="info" class="hint hint-info"><p>The Zero Day Agent identifies packages across your account. At the <strong>Account</strong> scope, the <strong>Affected Targets</strong> section displays all affected repositories and artifacts. At the <strong>Project</strong> scope, it displays only the repositories and artifacts associated with the selected project.</p></div>

    <figure><img src="../../.gitbook/assets/execution-details.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
7.  Select the **Execution Logs** tab to view the step-by-step run details, including repository discovery, component analysis, and notification steps.\
    Expand each step to view the actions performed by the agent and the output generated during the execution.

    * Expand the **Step 1: Batch Repository Discovery** card to review the repositories and artifacts where the affected packages were detected.
    * Expand the **Step 2: Batch Component Analysis** card to review the analysis for each affected package, including package details, dependency footprint, security relevance, and a summary of the analyzed findings.
    * Expand the **Step 3: Blast Radius Notification** card to review the notification generated by the agent, including the affected packages, impacted repositories and artifacts, severity, and notification details.

    <figure><img src="../../.gitbook/assets/agent-execution-logs.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

***

### Remediate an affected package <a href="#remediate-an-affected-package" id="remediate-an-affected-package"></a>

After reviewing the affected packages, you can remediate a package by creating a pull request with the recommended dependency update for an affected repository.

{% hint style="info" %}
Creating remediation pull requests is currently supported only for **direct dependencies**. Remediation pull requests are not supported for indirect dependencies or artifacts.
{% endhint %}

To create remediation pull requests, complete the following steps:

1. Navigate to the **Agents** page under the **Manage** section from the sidebar navigation of your SCS account.
2. Click the **Zero-Day Agent** card to open the agent. The **Details** tab opens by default.
3. Select the **Executions** tab to open the table of affected packages.
4. Click a package row to open the **Execution Details** side panel. The **Findings** tab opens by default.
5. In the **Affected Targets** section, review the repositories where the detected package can be remediated.
6. In the **Remediation** column, click the `Create PR` button to open a remediation pull request that updates the affected package manifest to the fixed version.
   * Targets with an open pull request show its status (for example, _IN PROGRESS_) and a link to the pull request.
   *   Targets with a merged pull request show its status (for example, _REMEDIATED_) and a link to the pull request.

       <figure><img src="../../.gitbook/assets/agent-remediation.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
7.  Open the generated pull request from the **Remediation** column to review the change before merging.

    <figure><img src="../../.gitbook/assets/zero-day-agent-pr.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

***

### Next steps <a href="#next-steps" id="next-steps"></a>

* [OSS Risks Remediation](../open-source-management/oss-risks-remediation.md) - Remediate detected open-source software (OSS) risks across your repositories and artifacts to strengthen your software supply chain security.
* [Direct/Indirect Dependency](../open-source-management/direct-indirect-dependency.md) - Learn how direct and indirect dependencies affect OSS risk remediation and how to filter SBOM dependencies by dependency type.
