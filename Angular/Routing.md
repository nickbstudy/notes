Routing should go in its own module.  This is consistent with what would be automatically generated.  The command to create `app-routing.module.ts` in the app folder for the main app is:
`ng g module app-routing --flat --module=app` 
with flat meaning at the base level.

Basic boilerplate for root routes looks like:
```
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

const routes: Routes = [];

@NgModule({
  declarations: [],
  imports: [
    RouterModule.forRoot(routes)   // note forRoot - child routing will be different
  ],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```
The HTML element to display routes is:
`<router-outlet></router-outlet>`

Fill the `routes` array with objects to define the addresses relative to root, and components (and optionally titles) for each as follows (components will need to be imported but intellisense should handle this):

```
const routes: Routes = [
  { path: 'home', component: HomeComponent, title: "Home - Joes Robot Shop" },
  { path: 'catalog', component: CatalogComponent, title: "Catalog - Joes Robot Shop" }
];
```

#### Matching ''

An important route to include is `''` representing the root of your site.  You should set this up as a redirect:
`{ path: '', redirectTo: '/home', pathMatch: 'full'}`
The `pathMatch` argument can be prefix or full, and specifies how much of the route should be checked.  Always leave this at full for the `''` route or it will interfere with outers.

#### Linking Routes

For hard coded routes simply swap `href` for `routerLink` and specify the component address:
`<a routerLink="/catalog">Catalog</a>`
There are a few ways you can do this with dynamic information too, either by using {{ }} and string concatenation, or the easiest way is to surrout `routerLink` with `[]` and pass an array of strings, e.g.
`<a [routerLink]="['user', user.id]">{{user.name}}</a>`

#### Navigating programmatically

Add to the constructor of the component `private router: Router` (with the relevant import), then where required use `this.router.navigate([])` with an array of string/s to the destination.

#### Route parameters

Add to the `app-routing.modules.ts` file at the end of your route `/:something` to indicate that will be a route parameter (you can have more than 1).  An example of a route with a filter parameter:
`{ path: 'catalog/:filter', component: CatalogComponent, title: "Catalog" },`
Then in the receiving component inject `private route: ActivatedRoute` and the relevant import.  You can then access the route through `this.route.snapshot.params['filter']`