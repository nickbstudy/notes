#### Exporting
```
export interface Person { }
export function hireDeveloper(): void { }
export default class Employee { } 
// ↑ If importing from this file this will be given
```
Or if you have lots to export (or want to rename module, note `Employee` below):
`export { Person, hireDeveloper, Employee as StaffMember };`


#### Importing
```
import { Person, hireDeveloper } from './person';
let human: Person;

import Worker from './person';
// ↑ This just imports the default export, can name Worker anything

import { StaffMember as CoWorker } from './person';

import * as HR from './person';
HR.hireDeveloper();
```

#### Relative vs Non-Relative
`'/filepath'` is root directory, `'./filepath'` is current folder `'../filepath'` is one below.  File extentions are unnecessary.  Generally use relative paths for your own modules, and non-relative (no `.` or `/`) for 3rd party modules.