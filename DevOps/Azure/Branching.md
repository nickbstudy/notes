In order to make changes, you create a branch of the master repository, commit your work to that, then create a pull request to merge that back into the main branch.

- `push` pushes your code onto the branch
- `pull` overwrites your code with the source code
- `fetch` grabs the differences between your current code and any differences while you've been working on the branch

#### Branch management

- **Branch Policies** - Requiring approval from a specified number of reviewers on pull requests, checking for linked work items on PRs, check to see that all comments have been resolved, limiting merge types.

- **Branch Restrictions** - Validate code by pre-merging and building PR changes, require other services to post successful statuses to complete PRs, restrict who can push to the branch.

- **Branch Protections** - Prevent deletion & overwriting commit history (with force push)

---

### Branching strategies

Having a solid strategy optimizes productivity, better enables parallel development, helps to plan sets of structured releases, promotes paths for software changes through production, delivers changes quickly, and is required to support multiple versions of software and patches.

**3 Main types of branch strategy:**

- **Trunk-Based** - Really, really quick branches
- **Feature (or Task)** - Branch per user story
- **Release** - All applicable user stories and merges them all at once.

**Trunk-Based**

Developers push directly to the main code line every single time they push.

Pros: 
- Easy for a really small number of developers (esp. 1)

Cons:
- Large code review process, difficult to undo.  The more people with those branches the more conflicts can occur.

**Feature (or Task)**

A separate development branch is created off main, then feature branches stem from that.  Once all features or tasks have been completed it is merged back to main.

Pros:
- Enabled independent and experimental innovation
- Easy to segment
- Easy to implement CI/CD workflows

Cons:
- The longer it takes, the more difficult to merge.  This can be made easier with feature flags (use configuration options to enable/disable features within the code).  Don't forget to remove feature flags once the feature is deemed stable.

**Release**

Branches are created for all features per release.  A branch for 'V1.0.x' might be created off main, off which a development branch is made, which contains feature branches.

Pros:
- **Only way to support multiple versions in parallel**
- **Allows for customizations for a specific customer**
- Focus on specific issues per patch or release

Cons: 
- Difficult to maintain
- Can't have many changes or contributors
- Potentially create more work for teams per version

---

### Pull request workflow

Pull requests encourage collaboration and verification of valid code.  They contain the "What/Why/How" of the changes", and are used on a branch prior to merging.

Pulls requests should contain:

- What - An explanation of the changes that you made
- Why - The business or technical goal of the changes
- How - Design decisions and rationale on code change approaches
- Tests - Verification of tests performed and all results
- References -  Work Items, screenshots, links, documentation, etc
- The Rest - Challenges, improvements, optimizations, budget requirements

A typical workflow example:

```

                          ╭--------------------- [Make Changes]
                          ↓                            ↑
[Assign request] → [Review Code] → [Good?] →NO [Request Changes]
                                    ↓YES
                              [Approve Request]
                                     ↓
                                   Merge
```

---
### Relating Work Items

Any changes you make to code should be relatable to a work item.  Use # to add work item references (supported in GitHub, can be enforced in Azure).  It's possible to close items with work requests using keywords.