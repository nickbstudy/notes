This model method is run before the template is built.  The class must implement it.  Usage:

```
export class SiteHeaderComponent implements OnInit{

  constructor() {}

  ngOnInit(): void {
    // do stuff
  }
}
```