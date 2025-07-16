# CircleCI-Integration-into-JIRA
This is a guide on how to integrate CircleCI into JIRA 

# CircleCI Integration with Jira

This guide provides a comprehensive, step-by-step walkthrough to integrate **CircleCI** with your **Jira Ticket Boards**. Enhance your development workflow by visualizing CI/CD statuses directly within Jira.

---

## Why Integrate CircleCI with Jira?

Connecting CircleCI with Jira offers significant benefits:

* **Centralized Visibility**: See the build, deployment, and test statuses of your CircleCI pipelines directly within the relevant Jira Ticket dashboard.
* **Streamlined Workflow**: Gain a unified view of your development progress, enabling quicker decision-making and better team coordination.
* **Enhanced Tracking**: Easily track the journey of a feature or bug fix from code commit to deployment within your project management tool.

---

## Understanding the Tools

Before we dive in, let's briefly define the key players:

* ### CircleCI
    A powerful **CI/CD (Continuous Integration/Continuous Delivery)** platform that automates the building, testing, and deploying of software. It integrates seamlessly with popular version control systems like GitHub to streamline development.

* ### Jira
    An industry-leading **project management and issue-tracking tool** developed by Atlassian. Jira helps teams plan, track, and manage tasks, bugs, and releases, commonly used in agile environments.

---

## Integration Steps: A Detailed Guide

Follow these steps carefully to successfully link your CircleCI pipelines to Jira tickets.

### 1. Setting Up CircleCI

Ensure your CircleCI account is ready:

* **Sign up/Log in** to your CircleCI account: [CircleCI Signup/Login](https://circleci.com/)
* **Connect your GitHub profile** (personal or work) to your CircleCI account.
* For a more detailed setup guide, refer to the official documentation: [Setting up CircleCI](https://circleci.com/docs/2.0/getting-started/)
* **Opt into your relevant organization** or create a new one. For GitHub-backed CI/CD projects, your organization name typically matches your GitHub organization/account name.
* You'll be redirected to your organizational homepage within CircleCI.

### 2. Setting Up Jira

Prepare your Jira environment for the integration:

* **Sign up/Log in** to your Jira account: [Jira Signup/Login](https://www.atlassian.com/software/jira)
* Find a more detailed setup guide here: [Getting Started with Jira Software](https://confluence.atlassian.com/jirasoftwarecloud/getting-started-with-jira-software-967523910.html)
* **Create the relevant project** based on your desired template and invite your team members.
* You'll land on your Jira homepage, from which you can navigate to projects, dashboards, and create issues/tickets.

### 3. Connecting CircleCI and Jira

This is the core of the integration. Pay close attention to each sub-step:

1.  In your Jira instance, navigate to the **Apps** section and select **Explore more apps**.
2.  Search for "**GitHub for Jira**" and add it to your Jira account. **Crucially, select the app provided by Atlassian.**
3.  **Link GitHub for Jira** by authorizing your Atlassian account to access your GitHub account/organization.
    * For more information on this specific integration, see: [Jira Integration with GitHub](https://support.atlassian.com/jira-cloud-administration/docs/integrate-jira-with-github/)
4.  Return to the **Explore apps** page and search for "**CircleCI**". Add the CircleCI app developed by CircleCI.
5.  Click on the **Configure CircleCI** option. You will need to input your **CircleCI Organization ID**.
6.  To locate your CircleCI Organization ID:
    * Open your CircleCI web UI.
    * Go to **Organization Settings**.
    * From the **Overview** side-tab, simply copy the **Organization ID**.
7.  Paste this **Organization ID** into the CircleCI configuration page in Jira.
8.  **Submit and authorize** Atlassian to establish the connection with your CircleCI account.
9.  Jira will automatically generate a **web trigger URL**. **Copy this URL** as you'll need it shortly.
10. Now, switch back to **CircleCI**. Go to your **Organization Settings** and then the **Contexts** side-tab.
11. Create a **new context**. Give it a descriptive name, for example, "Automation" or "JiraIntegration".
12. Within this new context, select **Add Environment Variable**.
13. Create a new environment variable named `JIRA_WEBHOOK_URL` and paste the webhook URL you copied from Jira (in step 9) as its value. Click **Add environment variable**. This creates a global environment variable accessible by your CircleCI pipelines.
14. In your GitHub project, navigate to the `.circleci` folder and open the `config.yml` file.
15. **Insert Jira Orbs** into your `config.yml`. This typically involves adding the following lines at the top of your config file:
    ```yaml
    version: 2.1

    orbs:
      jira: circleci/jira@x.y.z # Use the latest version available
    ```
    (Replace `x.y.z` with the latest stable version of the Jira orb, which you can find on the CircleCI Orb Registry.)
16. Add a `jira/notify` block at the end of your `run` commands within the `steps` block of each job you want to report to Jira. Customize the parameters as needed:

    ```yaml
    # Example job snippet in your config.yml
    jobs:
      build-and-test:
        docker:
          - image: cimg/node:18.10.0
        steps:
          - checkout
          - run: npm install
          - run: npm test
          - jira/notify:
              environment-name: "Development"
              environment-type: "development"
              job-type: "build"
              pipeline-id: "${CIRCLE_WORKFLOW_ID}" # CircleCI dynamic variable
              pipeline-number: "${CIRCLE_BUILD_NUM}" # CircleCI dynamic variable
              status: "success" # or "failure" based on condition
    ```

    * **`environment-name`**: (e.g., `"Staging"`, `"Production"`, `"Development"`) for Jira visibility.
    * **`environment-type`**: (e.g., `"testing"`, `"production"`, `"development"`) based on your use case.
    * **`job-type`**: (e.g., `"build"`, `"deployment"`, `"test"`) helps categorize the event in Jira.
    * **`pipeline-id`** and **`pipeline-number`**: These are dynamically picked by CircleCI using environment variables (`CIRCLE_WORKFLOW_ID` and `CIRCLE_BUILD_NUM`).
    * You can verify `pipeline-id` and `pipeline_number` by going to the **Pipelines** side-tab in CircleCI.

17. To ensure your workflows use the `JIRA_WEBHOOK_URL` context, add the following snippet under every `workflow` job where you want Jira notifications:

    ```yaml
    workflows:
      your-workflow:
        jobs:
          - build-and-test:
              context: jira-webhook-context # Use the name of your created context (e.g., 'Automation' or 'jira-webhook-context')
    ```
    Replace `jira-webhook-context` with the actual name you gave your context in step 11.

### 4. Linking Commits to Jira Tickets

CircleCI now knows which Jira account to notify. To tell it *which specific ticket* to update, simply:

* **Include the Jira ticket tag** (e.g., `PROJ-123`, `BUG-456`) directly in your **commit message title**.
    * Example: `git commit -m "PROJ-123: Implemented user authentication"`

CircleCI will automatically pick up this tag and send the build/test/deployment status details to the corresponding Jira ticket dashboard.

---

## Verifying the Integration

To confirm everything is working seamlessly:

1.  Navigate to the relevant Jira ticket dashboard (e.g., `PROJ-123`).
2.  Look for options like **"Builds"** or **"Deployments"** within the ticket's sidebar or detail view.
3.  You should now see the associated CircleCI build status and deployment status directly in Jira.
    * The **Latest Build number** in Jira should match the build number in the corresponding CircleCI pipeline.
    * The **Environment name** (e.g., "Development", "Staging") and **Environment Type** (e.g., "development", "testing") should match what you configured in your `config.yml`.

---

## Additional Resources

* For more official information on CircleCI's Jira integration: [Integrate CircleCI with Jira](https://circleci.com/docs/jira-integration/)
* Explore more code examples and advanced usage for the Jira Orb: [CircleCI Jira Orb Usage Examples](https://github.com/CircleCI-Public/jira-orb/blob/main/src/examples/example.yml)

---
