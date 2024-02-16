>*"DevOps represents a change in IT culture, focusing on rapid IT service delivery through the adoption of agile, lean practices in the context of a system-oriented approach."
*\- Gartner Research*

>"DevOps is the combination of cultural philosophies, practices, and tools that increases an organizations ability to delivery applications and services at high velocity."
*\- AWS*

>"DevOps is a set of practices that works to automate and integrate the processes between softwaare development and IT teams, so they can build, test, and release software faster and more reliably."
*\- Atlassian*

>In science and technology, we grossly underestimate the value of certainty
*\- Chris Behrens*

Deployment (of a correct, secure product) is always the top priority.

Aim to identify and deliver small batches of work.

Secrets & Security don't belong in version control - they should be environment variables on a local machines, or secret variables in ADO.

Set up an automatic build to fail if tests don't all pass.

Pull requests should go through a senior developer before being merged into main/master.

Make work (items, builds, progress, metrics) visible - kanban boards, planner, etc.

**Version Control** - Trunk based development is preferred to feature branches.  People should be checking into a main branch frequently.  If people are doing big chunks of build on their own you will run into more merge issues.  Also use version control for a wide set of assets, not just code but the build scripts etc.

Create templates for key activites (builds etc)

Shift left on security - integrate it sooner in the process.