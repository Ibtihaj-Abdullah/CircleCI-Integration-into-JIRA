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
<img width="544" height="249" alt="image" src="https://github.com/user-attachments/assets/97ed15f3-eb84-42ab-b7f3-3c163efa7241" />

* You'll be redirected to your organizational homepage within CircleCI.
<img width="564" height="263" alt="image" src="https://github.com/user-attachments/assets/18fd8391-91c5-4168-8b69-f6b413bc3015" />


### 2. Setting Up Jira

Prepare your Jira environment for the integration:

* **Sign up/Log in** to your Jira account: [Jira Signup/Login](https://www.atlassian.com/software/jira)
* Find a more detailed setup guide here: [Getting Started with Jira Software](https://confluence.atlassian.com/jirasoftwarecloud/getting-started-with-jira-software-967523910.html)
* **Create the relevant project** based on your desired template and invite your team members.
* You'll land on your Jira homepage, from which you can navigate to projects, dashboards, and create issues/tickets.
<img width="618" height="271" alt="image" src="https://github.com/user-attachments/assets/694cf9d7-a649-4602-8b00-806308023cfb" />


### 3. Connecting CircleCI and Jira

This is the core of the integration. Pay close attention to each sub-step:

1.  In your Jira instance, navigate to the **Apps** section and select **Explore more apps**.
<img width="126" height="135" alt="image" src="https://github.com/user-attachments/assets/d69ab7dd-f2db-499d-ae11-999056a2d3b5" />

2.  Search for "**GitHub for Jira**" and add it to your Jira account. **Crucially, select the app provided by Atlassian.**
<img width="385" height="151" alt="image" src="https://github.com/user-attachments/assets/147fa77e-d4bc-4623-b147-b53a714f0d53" />

3.  **Link GitHub for Jira** by authorizing your Atlassian account to access your GitHub account/organization.
    * For more information on this specific integration, see: [Jira Integration with GitHub](https://support.atlassian.com/jira-cloud-administration/docs/integrate-jira-with-github/)
<img width="852" height="151" alt="image" src="https://github.com/user-attachments/assets/eb5db4ec-b532-4e7d-9140-5b9a202535e9" />

4.  Return to the **Explore apps** page and search for "**CircleCI**". Add the CircleCI app developed by CircleCI.
<img width="367" height="183" alt="image" src="https://github.com/user-attachments/assets/6fb6ad16-795d-464a-b609-b4a72f028595" />

5.  Click on the **Configure CircleCI** option. You will need to input your **CircleCI Organization ID**.
<img width="393" height="253" alt="image" src="https://github.com/user-attachments/assets/131e5bb7-6ffd-4594-a594-81ebbb9127aa" />

6.  To locate your CircleCI Organization ID:
    * Open your CircleCI web UI.
    * Go to **Organization Settings**.
    * From the **Overview** side-tab, simply copy the **Organization ID**.
<img width="210" height="287" alt="image" src="https://github.com/user-attachments/assets/dad599cc-9d38-4092-9444-95ba45f7459d" />
<img width="525" height="94" alt="image" src="https://github.com/user-attachments/assets/610fb410-78ba-4c6a-8e00-71e3f524e67e" />

7.  Paste this **Organization ID** into the CircleCI configuration page in Jira.
8.  **Submit and authorize** Atlassian to establish the connection with your CircleCI account.
9.  Jira will automatically generate a **web trigger URL**. **Copy this URL** as you'll need it shortly.
10. Now, switch back to **CircleCI**. Go to your **Organization Settings** and then the **Contexts** side-tab.
<img width="613" height="334" alt="image" src="https://github.com/user-attachments/assets/92644ea9-9628-4e34-a1d0-45f696ba11fe" />

11. Create a **new context**. Give it a descriptive name, for example, "Automation" or "JiraIntegration".
<img width="390" height="288" alt="image" src="https://github.com/user-attachments/assets/4c69b258-2a29-47c9-a79c-3af0bbc9c9ea" />

12. Within this new context, select **Add Environment Variable**.
<img width="618" height="580" alt="image" src="https://github.com/user-attachments/assets/1ac2bb0d-3751-4d69-b714-83261d9a25ad" />

13. Create a new environment variable named `JIRA_WEBHOOK_URL` and paste the webhook URL you copied from Jira (in step 9) as its value. Click **Add environment variable**. This creates a global environment variable accessible by your CircleCI pipelines.
<img width="402" height="332" alt="image" src="https://github.com/user-attachments/assets/39cc2771-f760-464d-92d5-fe484116820f" />

14. In your GitHub project, navigate to the `.circleci` folder and open the `config.yml` file.
<img width="502" height="67" alt="image" src="https://github.com/user-attachments/assets/8f3fdf85-b674-4b43-816e-d52ac21c68c8" />

15. **Insert Jira Orbs** into your `config.yml`. This typically involves adding the following lines at the top of your config file:
    ```yaml
    version: 2.1

    orbs:
      jira: circleci/jira@x.y.z # Use the latest version available
    ```
    (Replace `x.y.z` with the latest stable version of the Jira orb, which you can find on the CircleCI Orb Registry.)
<img width="275" height="100" alt="image" src="https://github.com/user-attachments/assets/ba1cb358-e2fd-4715-91a7-7310a680e79b" />

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

<img width="453" height="184" alt="image" src="https://github.com/user-attachments/assets/b641ea65-0552-43f1-b3df-6a42d609fbd2" />

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
<img width="269" height="192" alt="image" src="https://github.com/user-attachments/assets/ca2bc610-c9f3-4061-be10-1ca956a19641" />
<img width="285" height="279" alt="image" src="https://github.com/user-attachments/assets/984b9c7f-8646-4284-a3bc-f4276ca4c50b" />

---

## Verifying the Integration

To confirm everything is working seamlessly:

1.  Navigate to the relevant Jira ticket dashboard (e.g., `PROJ-123`).
<img width="650" height="336" alt="image" src="https://github.com/user-attachments/assets/54e3ec77-f6f1-4e09-b7b2-23e5a9134cb8" />

2.  Look for options like **"Builds"** or **"Deployments"** within the ticket's sidebar or detail view.
3.  You should now see the associated CircleCI build status and deployment status directly in Jira.
    * The **Latest Build number** in Jira should match the build number in the corresponding CircleCI pipeline.
    * The **Environment name** (e.g., "Development", "Staging") and **Environment Type** (e.g., "development", "testing") should match what you configured in your `config.yml`.
<img width="505" height="167" alt="image" src="https://github.com/user-attachments/assets/620cfed7-c3e5-45d1-bbb9-2faa34de1e79" />
<img width="818" height="104" alt="image" src="https://github.com/user-attachments/assets/a085a1f6-fece-4628-8d50-9b52c340942e" />
<img width="727" height="301" alt="image" src="https://github.com/user-attachments/assets/7ed1db27-9ec0-47e6-904b-1c85d7ab1346" />
<img width="700" height="240" alt="image" src="https://github.com/user-attachments/assets/bf53c747-d4bb-4ffd-8466-41df726467e2" />

---

## Additional Resources

* For more official information on CircleCI's Jira integration: [Integrate CircleCI with Jira](https://circleci.com/docs/jira-integration/)
* Explore more code examples and advanced usage for the Jira Orb: [CircleCI Jira Orb Usage Examples](https://github.com/CircleCI-Public/jira-orb/blob/main/src/examples/example.yml)

---
