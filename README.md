# Routing Practice(project generated using Angular CLI version 6.0.8)

Here the very basics of Angular routing are explained starting from how to include the router module and configure possible routes within an Angular application to how we can pass and retrieve path and query parameters via the different routes.

## Creating the components that we will need to understanding how routing can be configured and used in an Angular application

Use Angular CLI to generate 3 components: 'home', 'about' and 'contact'.
> ng generate component home
> ng generate component about
> ng generate component contact

Since we have used Angular CLI the newly created components will be automatically added to the declarations array in the app module file.

### Adding imports to include the router module in an Angular application

- Open app.module.ts and add the below imports to include 
    - Routes(an alias of Route[] which is an array of objects with each entry mapping a route URL to a component)
    - RouterModule which allows us to configure and use Angular's routing features.


```typescript
     import { Routes, RouterModule } from '@angular/router';
```

### Creating an array of routes with each route having at least two properties: the URL path and the component it is mapped to.

```typescript
    const appRoutes: Routes = [
      {
        path: '',
        component: HomeComponent
      },
      {
        path: 'home',
        redirectTo: ''
      },
      {
        path: 'about',
        component: AboutComponent
      },
      {
        path: 'contact',
        component: ContactComponent
      }
    ];
```

### Wire the routes created in the above step so that we can actually use them when we run the application

- In the 'imports' array within the app module file add the below statement after the BrowserModule import:

```typescript
      RouterModule.forRoot(appRoutes)
```
Now if we run the application by using:

ng serve(for local machine) 
OR 
ng serve --host 0.0.0.0 --port 8080 --public-host $C9_HOSTNAME(when using Cloud9 online IDE)
we _should_ be able to access content specific to the routes configured

http://localhost:4200 or http://localhost:4200/home should display content provided by the HomeComponent.
http://localhost:4200/about should display the content of AboutComponent.
http://localhost:4200/contact should display the content of ContactComponent.

