#### General concepts

Stands for *Continuous Integration, Continuous Delivery* - not continuous deployment

After fully implementing the software will be
- Higher quality
- Faster to deliver
- Lower cost
- More flexible

---

#### Continuous Integration
Definition: A development practice that requires developers to integrate code into a **shared repository one or more times** a day
- You need to have a single place where all the code lives.
- Everyone commits to the mainline every day.
- Automate the build (and test) process - this forces devs to fix broken builds immediately, and make and keep the build fast.
- Every commit should trigger a build.
- Everyone needs access to the latest build result, and can see everything going on.

As a result:
- Integration takes less effort
- Issues will surface earlier
- Automation means less issues
- The process is more visible
- Improves team communication
- Short integration iterations mean more flexibility to respond to changes in requirements
- The code is ready to be delivered more often.

---

#### Continuous Delivery
Definition: A software development principle where software can be released to production at any time.

A pipeline is used to deploy a provided build.  Separate pipelines typically exist for builds and releases.

Principles:
- You need CI in place
- Development (make new things) and Operations (keep things running) should work well together
- Automate the release process (powershell scripts etc) inc. tests
- Include release to definition of done
- Releasing should be on-demand.  Things can build to dev, but not prod.
- Everyone should be able to see and access everything

Benefits:

- Releasing takes less effort
- Releasing is more reliable and repeatable
- Puts control of release in the hands of the business
- Release more often
- Get feedback earlier

---

#### Technical Implementation

### CI:

1. **All code needs to get to a single source control repository.**  This will require a branching strategy.  Typically everybody will work on one branch off main.  Avoid the pitfall of having too many branches for different features/versions - this detracts from the whole point of being centralized. Merge to a single repository.

2. **Make sure all departments/users collaborate.**  Everybody needs to know what everybody else is working on (Planner kanban board/daily Teams stand up?).

3. **Create an automated build process.**  Set up a dedicated server & build engine (Azure DevOps goes here).  Create build definitions, and store them in source control too.

4. **Create an Automated Test Process.**  Run the static tests (Unit tests etc) with the build, deploy it to a (temporary) environment, and run UI tests.

### CD:

1. Have CI in place
2. Unite operations and developers
3. Create a release pipeline (and optional IaC - Infrastructure as Code - to script configuration)