Pipelines are the primary engine of CICD.  They automate the application building and deployment process with steps defined in YAML files.

**CI (Continuous Integration)**
- Automatically builds and tests code
- Creates deployable artifacts

**CD (Continuous Delivery)**
- Automatically deploy to environments/end users

---

#### Pipeline Structure

##### (Trigger) → Stages → Jobs → Steps

Items are carried out by 'agents' (managed VMs)

---
#### Self Hosting

You will need a PAT (Personal Access Token) - go to organization, click person (User Settings on hover) next to profile pic then 'PATs', New Token, name it anything (e.g 'self-hosted-agent').  When creating, to give access to agent pools click 'Show All Scopes' then under 'Agent Pools' check Read & manage.  Copy & save token.

Go to Organization Settings, Pipelines → Agent pools → Default → New Agent, and follow the steps.

After installing the agent, `. .\config.cmd` will launch the setup wizard.
Config will prompt for server URL - this is `https://dev.azure.com/your-org-name`, then press enter for PAT, then paste it, then join desired pool (probably default)
If you don't set up service/autorun, start it with `. .\run.cmd`



**YAML Self-hosted pool assignment**
```
pool:
    name: 'Default'   // or 'AnotherPoolsNameHere' if using something else
```

---

#### Build trigger rules

Typically run on a commit to a new repository.  (Usually) defined in pipeline YAML.  4 main types are:

**1. CI Trigger - run when updating repo/branch**
```
trigger:
    branches:
        include:
        - master
        - releases/*
        - refs/tags/{tagname}
        exclude:
        - releases/old*
```

**2. Schedue Trigger - specified time**

Typically run nightly, triggered independent of repository, schedule defined in cron format, can choose to run only if targeted repo has changed
```
schedules:
-cron: <cron string here>
    displayName: my-schedule-build
    branches:
        include: new-feature
    always: true
```

**3. Pipeline Trigger**

Used when upstream component (library) changes, downstream dependencies must be rebuilt.  Include triggering pipeline and source, and optional branch/tag/stage filters for referenced pipeline

```
resources:
    pipelines:
    - pipeline: upstream-lib
      source: upstream-lib-ci
      project: your-project-name
      trigger: true  // Trigger filter to trigger pipeline when any version of the source pipeline completes
```

**4. Run on pull request**

Runs validation/testing upon pull request too.  Configuration depends on repo location - Azure Repos is configured in branch policy, all others in YAML.