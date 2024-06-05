# Gitlab variables, stages and artifacts

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

#
# Stages in GitLab :
- In GitLab CI/CD, stages are used to <b>define the sequence in which jobs are executed</b>. Jobs in the same stage are run in parallel, whereas stages themselves run sequentially. This means that all jobs in a stage must complete successfully before the next stage starts.

## How to configure stages in .gitlab-ci.yml file :
- you need to define the stages and then assign jobs to those stages.
  
### Steps :
- <b>Define Stages</b>:
  - Specify the stages in your .gitlab-ci.yml file. The order in which stages are listed is the order in which they will be executed.

#
- <b>Define Jobs</b>:
  - Create jobs and assign them to the appropriate stages. Each job must specify a stage.

#
- <b>Specify Scripts</b>:
  - Define the script that each job will run.

#
### Example:
```bash
# Define stages
stages:
  - build
  - test
  - deploy

# Job definitions
# Build job
build_job:
  stage: build
  script:
    - echo "Building the project..."
    - # Add your build commands here

# Test job 1
test_job_1:
  stage: test
  script:
    - echo "Running tests..."
    - # Add your test commands here

# Test job 2
test_job_2:
  stage: test
  script:
    - echo "Running additional tests..."
    - # Add your test commands here

# Deploy job
deploy_job:
  stage: deploy
  script:
    - echo "Deploying the project..."
    - # Add your deploy commands here
```
## Explanation :
- <b>Define Stages</b>:
  - The stages keyword defines a list of stages in the order they should be executed.
  ```bash
  stages:
  - build
  - test
  - deploy
  ```

#
- <b>Define Jobs</b>:
  - Each job specifies a stage to indicate when it should run.
  - Jobs within the same stage will run in parallel.
  ```bash
  build_job:
  stage: build
  script:
    - echo "Building the project..."
  ```

#  
### Complete yaml file:
```bash
stages:
  - build
  - test
  - deploy

build_job:
  stage: build
  script:
    - echo "Building the project..."

test_job:
  stage: test
  script:
    - echo "Running tests..."

deploy_job:
  stage: deploy
  script:
    - echo "Deploying the project..."
```
#
# GitLab Artifacts
- In GitLab CI/CD, artifacts are files and directories created by jobs and stored by GitLab after the job finishes. They are typically used to share data between jobs or to keep build results. <a href="https://docs.gitlab.com/ee/ci/jobs/job_artifacts.html">Gitlab Artifacts</a>
- Artifacts can be configured to be kept for a certain duration, and they can be used for debugging, testing, or deployment.

#
## Configuring Artifacts in GitLab CI/CD
- To configure artifacts, we need to define them in your .gitlab-ci.yml file under the artifacts keyword within a job.
- We can specify the paths to the files or directories you want to save as artifacts.

### Example: 
```bash
stages:
  - build
  - test
  - deploy

build_job:
  stage: build
  script:
    - echo "Building project..."
    - mkdir -p build
    - echo "Compiled files" > build/compiled.txt
  artifacts:
    paths:
      - build/
    expire_in: 1 week

test_job:
  stage: test
  script:
    - cat "./build/compiled.txt"
```

### Explanation: 
- Build Job: 
  - Creates a build/ directory and a file named compiled.txt inside it.
  - The build/ directory is specified as an artifact path, so all its contents will be saved as artifacts.
  - The artifacts will expire in one week.

- Test Job:
  - This job will print the content of compiled.txt 
  
