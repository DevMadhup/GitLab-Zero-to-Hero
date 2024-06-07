# Gitlab Runners

## What are runners in Gitlab?
- Runners are the agents that run the CI/CD jobs that come from GitLab. <a href="https://docs.gitlab.com/runner/">Runners in Gitlab</b>
- When you register a runner, you are setting up communication between your GitLab instance and the machine where GitLab Runner is installed.
- Runners usually process jobs on the same machine where you installed GitLab Runner. However, you can also have a runner process jobs in a container, in a Kubernetes cluster, or in auto-scaled instances in the cloud.
- There are two types of runners:
  - Gitlab SaaS runners/Shared runners (Managed by Gitlab)
  - Gitlab self-managed runners   

## Executors:
- When you register a runner, you must choose an executor.
- An executor determines the environment each job runs in.

### For example:

- If you want your CI/CD job to run PowerShell commands, you might install GitLab Runner on a Windows server and then register a runner that uses the shell executor.
- If you want your CI/CD job to run commands in a custom Docker container, you might install GitLab Runner on a Linux server and register a runner that uses the Docker executor.

#
<b>Check the below screen of CI/CD pipeline where job ran if we not configure personal runner</b>
<img src="https://github.com/DevMadhup/GitLab-Zero-to-Hero/blob/main/Assets/Shared-runner.png" />

> To list all the runners in gitlab
- Project --> Settings --> CI/CD --> Runners
<img src="https://github.com/DevMadhup/GitLab-Zero-to-Hero/blob/main/Assets/No-of-SharedRunners.png" />

#
## How to add runners in Gitlab:
- Go to any cloud provider and create one ubuntu linux machine

#
- Connect to instance

#
- Navigate to Gitlab, Project --> Settings --> CI/CD --> Runners --> Expand and click on three dots as shown in the screenshot below
<img src="https://github.com/DevMadhup/GitLab-Zero-to-Hero/blob/main/Assets/RegisterRunner-1.png" />
<img src="https://github.com/DevMadhup/GitLab-Zero-to-Hero/blob/main/Assets/RegisterRunner-commands.png" />

#
- Copy the commands and create a script in your linux machine and run the script.
<img src="https://github.com/DevMadhup/GitLab-Zero-to-Hero/blob/main/Assets/Runner-Script.png" />

#
- Copy the next command to register runner from the gitlab.
  - Enter the GitLab instance URL (for example, https://gitlab.com/): Press Enter
  - Enter the registration token: Press Enter
  - Enter a description for the runner: Nodeapp Runner
  - Enter tags for the runner (comma-separated): Node, ubuntu (You can add any tags, these tags are very important because we need these tags in CI/CD)
  - Enter an executor: Shell

<img src="https://github.com/DevMadhup/GitLab-Zero-to-Hero/blob/main/Assets/Runner-registered%20successfully.png" />
<img src="https://github.com/DevMadhup/GitLab-Zero-to-Hero/blob/main/Assets/Runner-web.png" />

#
## How to run CI/CD on our personal runner:
- Go to project and create a <b>.gitlab-ci.yml</b>

#
- Add the below code
```bash
stages:
  - build

build_job:
  stage: build
  script:
    - echo "Building project $CI_PROJECT_NAME"
  tags:
    - Node
    - ubuntu
```
- If your pipeline fails with error like below
> ERROR: Job failed: prepare environment: exit status 1. Check https://docs.gitlab.com/runner/shells/index.html#shell-profile-loading for more informatio

- To troubleshoot this error, check <b>/home/gitlab-runner/.bash_logout</b>. For example, if the .bash_logout file has a script section like the following, comment it out and restart the pipeline:
```bash
if [ "$SHLVL" = 1 ]; then
    [ -x /usr/bin/clear_console ] && /usr/bin/clear_console -q
fi
```

#
- Check CI/CD pipeline logs to see where project ran:
<img src="https://github.com/DevMadhup/GitLab-Zero-to-Hero/blob/main/Assets/Ran-onRunner.png" />

#
