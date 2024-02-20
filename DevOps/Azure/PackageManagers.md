Software packages are hosted in a "Package Manager" (think NPM for Node.JS), Azure Artifacts is the built in offering. From a DevOps perspective they are an upstream dependency, and the Package Hosting Service is the Package/Artifact source of truth.

---

#### Azure Artifacts

Pipelines publish artifacts/packages to feeds in Azure Artifacts.  Feeds host your packages and let you control permissions.  You can control visibility, Upstream sources, and Scope (Project or Organization).  These can be accessed as NuGet packages in Visual Studio.

The workflow is:
```
                                    Auth & Connect
Build Pipeline → Azure Artifacts Feed ←-------→ Developer working with upstream packages
```

---
#### Creating a Versioning Strategy for Artifacts

As the application develops, multiple versions of artifacts/packages will be created.  A well-developed versioning strategy = better management.  **Packages are immutable** - they cannot be changed after creation, and the version numbers are permanently assigned and cannot be reused.

**Versioning Recommendations - "Semantic Versioning + Quality of Change"**

Format breakdown:
```

|       2.1.3      |     prerelease    |
| nature of change | quality of change |

 {major}.{minor}.{patch}.{patch}-{tag}
```

**Feed Views (accessed through settings icon)**
Allows you to set access for different users to different versions (separate for development/tested).  @local view is the default view, it cannot be renamed or deleted.  It contains packages published directly to the feed, then packages will be promoted to other views for consumption when ready.