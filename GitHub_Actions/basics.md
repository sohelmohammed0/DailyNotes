## What is GitHub Actions?
- GitHub Actions is a Githubs built in CI/CD and automation platform; you write workflows (YAML files) stored in you repo that runs automatically on events (push, PR, schedule etc) Workflows are composed of Jobs -> steps -> actions (reusable tasks) and run on runners (GitHub Hosted or Self Hosted)

## Key COncepts:

1. Workflow:
    - A complete automation pipeline defined in YAML file in .github/workflows/ that defines when and what to run
2. Event (trigger):
    - What starts a workflow (push, pull_request schedule, workflow_dispatch)
3. Job: 
    - Collection of steps that runs on the same runner (can run in parallel with other jobs)
4. Step: 
    - a single task inside a job either uses:... (an action) or run:... (shell commands)
5. Action:
    - reusable units (officical or community) you can use: an cation or write your own
6. Runner:
    - The machine that ececutes jobs (github-hosted runners are provided; you can also register self hosted runners)
7. Secrets/Environments:
    - secure storage for credentials and environment specific safegaurds (use them, dont hard code secrets)