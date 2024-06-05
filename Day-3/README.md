# Gitlab variables, runners, stages and artifacts

## What are variables in Gitlab? 
- In GitLab, variables are used to define values that can be used in the CI/CD pipeline configurations, making the pipelines more dynamic and flexible. They can store information like credentials, settings, or any other data that you want to reuse across multiple jobs and pipelines. <a href="https://docs.gitlab.com/ee/ci/variables/">Variables in GitLab</a>
- CI/CD variables are a type of environment variable. You can use them to:

  - Control the behavior of jobs and pipelines.
  - Store values you want to re-use.
  - Avoid hard-coding values in your .gitlab-ci.yml file.

#
### Types of variables in Gitlab :

- <b>Predefined Variables</b>: These are built-in variables provided by GitLab that give information about the GitLab environment, project, job, etc. <a href="https://docs.gitlab.com/ee/ci/variables/predefined_variables.html">Predefined CI/CD variables in GitLab</a>  
  #### Examples:
  - CI_PROJECT_NAME: The name of the project.
  - CI_COMMIT_REF_NAME: The branch or tag name for which the project is built.
  - CI_JOB_ID: The unique ID of the current job.


  ```bash
  job:
  script:
    - echo "Project name is $CI_PROJECT_NAME"
    - echo "Commit ref name is $CI_COMMIT_REF_NAME"
    - echo "Job ID is $CI_JOB_ID"
  ```

#
- <b>Custom Variables</b>: These are user-defined variables that you can create and use in your pipeline. You can define custom variables directly in your .gitlab-ci.yml file, in the GitLab UI at the project or group level, or in the instance level (for self-managed GitLab instances).
  ```bash
  variables:
    DEPLOY_ENV: "production"
    API_KEY: "your_api_key_here"

  deploy_job:
    script:
      - echo "Deploying to $DEPLOY_ENV"
      - echo "Using API key $API_KEY"
  ```

#
- <b>Secret Variables</b>: These are variables that are masked in job logs and can be used to store sensitive information like API tokens or passwords.
  ```bash
  stages:
  - deploy

  deploy_job:
    stage: deploy
    script:
      - echo "Deploying with API key $API_KEY"
  ```

### Creating Secret Variables
- Navigate to Your Project:

- Open GitLab and go to your project’s main page.
  - Go to Settings:

  - On the left sidebar, click on Settings.
    - Open CI/CD Settings:

  - Within the settings menu, click on CI/CD.
    - Expand Variables Section:

  - Scroll down to the Variables section and click on Expand.
    - Add a New Variable:

  - Click on the Add variable button.

- Variable Details:
  - Key: Enter the name of your variable (e.g., API_KEY).
  - Value: Enter the value for your variable (e.g., your_secret_api_key).
  - Type: By default, it’s set to Environment variable.
  - Protected: Unchecked
  - Masked: Checked, the variable’s value will be hidden in job logs.
  - Environment scope: By default, it applies to all environments. You can specify a particular environment if needed.

- Save the Variable:
- Click on the Add variable button to save your new secret variable.
