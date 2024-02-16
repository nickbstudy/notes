 Managed repositories inside a DevOps organization, assosciated with a project.

 You can start them from scratch, import an external repo, or push a local code base.
 
 They use standard git commands (`git add .` `git commit -m "message"` `git push`, or to pull the branch ==(clarification needed?)== `git pull`)

 ---

 #### Submodules

 These allow you to incorporate resources from a different Git project in your current repo (e.g. 3rd party libraries).  Useful when you need to treat external resources as separate entities, yet seamlessly include them within your project.

 The command you use to incorporate them in the repo is `git submodule add https://github.com/account/added-package` and this is a reference to an outside package - you can't edit it, and it will be updated as the package is.  Extra steps are required when cloning projects with submodules locally, you must run `git submodule init` then `git submodule update`

 If they are to be included in a pipeline, they must be unauthenticated, or authenticated with your main repositorys account, and registered via HTTP not SSH.

 ---

 #### Incorporating changelogs

 `git log` gives full details of all commits (press Q to exit at end)
 `git log --oneline` gives a more concise list
 `git log --pretty="- %s"` Removes everything but the message (represented by the `%s` and can be customized, this example starts each line with a dash)