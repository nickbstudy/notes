MVC = Model View Controller.  

**Model:**
- Domain data & logic to manage data
- Simple API
- Hides details of managing the data

**View**

Razor page (C#/HTML) incorporating tag helpers (such as `asp-something`)

**Controller**

Models are like the 'nouns'.  Controller controls where it enters but not what is being seen.  Views are based off models
Keep in mind the distinction of 'list pages' and 'detail pages' - compared to a facebook feed vs a profile page`

---

#### Migrations

When a model changes or is added/removed and the database needs to be updated to reflect that, a migration must be run.  Go to package manager and run `add-migration WhateverIsHappening` then `update-database` - the first command prepares the migration file and the second executes it.