Services are essential for the 'Single Responsibility Principle'.

Decorate a service with:
```
@Injectable({ providedIn: 'root' })
```

For module based apps: if you need multiple separate instances, in which case you can create an interface and use tokens (see section 5 of "Angular Services" course).  They also have to be lazy-loaded.

For standalone component apps, define these as additional arguments for the routes.  If services need to be shared use child components to share parent services.
