# Routing Practice(generated using Angular CLI version 6.0.8)

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
### Add the router-outlet tag in app.component.html to enable Angular to work with the configured routes:

```html
    <div style="text-align:center">
      <h1>
        Welcome to {{ title }}!
      </h1>
    </div>
    <router-outlet></router-outlet>
```

With the above change in place Angular will inject the selector for the active component based on the URL we are currently on _after_ the _router-outlet_ tag in the rendered page.

Now if we run the application by using:

ng serve(for local machine) 
OR 
ng serve --host 0.0.0.0 --port 8080 --public-host $C9_HOSTNAME(when using Cloud9 online IDE)
we _should_ be able to access content specific to the routes configured

http://localhost:4200 or http://localhost:4200/home should display content provided by the HomeComponent.

http://localhost:4200/about should display the content of AboutComponent.

http://localhost:4200/contact should display the content of ContactComponent.

Along with typing the different route URLs in the browser's address bar 
we can add a navigation bar to the Home page with links to these routes so that it is closer to how a real-world application works.
For this we need to add a nav element within app.component.html which contains anchor tags for the different routes within it.
The updated app.component.html is given below:

```html

	<div style="text-align:center">
	  <h1>
		Welcome to {{ title }}!
	  </h1>
	</div>
	<nav>
	  <a href="/">Home</a>&nbsp;
	  <a href="/about">About</a>&nbsp;
	  <a href="/contact">Contact</a>
	</nav>
	<router-outlet></router-outlet>

```

With the above change we would see three links 'Home', 'About', 'Contact' but when we click on any one of them the complete page will get refreshed which defeats the purpose of creating a Single Page Application(SPA) using Angular.
To give the control to Angular when we click on these links and avoid full page reload we need to replace 'href' with 'routerLink' property.

```html

	<div style="text-align:center">
	  <h1>
		Welcome to {{ title }}!
	  </h1>
	</div>
	<nav>
	  <a routerLink="/">Home</a>&nbsp;
	  <a routerLink="/about">About</a>&nbsp;
	  <a routerLink="/contact">Contact</a>
	</nav>
	<router-outlet></router-outlet>

```

We can now add some style to these links to display the active/current link with a different background color.

```css

  .active {
      background-color: #E3E3E3;
  }
  
```

This class can now be added to all the links using the `routerLinkActive` directive. 
Updated app.component.html:

```html

	<div style="text-align:center">
	  <h1>
		Welcome to {{ title }}!
	  </h1>
	</div>
	<nav>
	  <a routerLink="/" routerLinkActive="active">Home</a>&nbsp;
	  <a routerLink="/about" routerLinkActive="active">About</a>&nbsp;
	  <a routerLink="/contact" routerLinkActive="active">Contact</a>
	</nav>
	<router-outlet></router-outlet>

```
With above update we see the link we are currently at gets a grey background but there is a minor issue.
The 'home' link is _always_ highlighted. This is because if we are on /about or /contact it *includes* the '/'(root route).
To resolve this we can add another directive `routerLinkActiveOptions` with the exact match set to true.
The exact:true value makes sure that the route should exactly match for it to be styled as active.(inclusive routes are _not_ allowed)

```html

	<div style="text-align:center">
	  <h1>
		Welcome to {{ title }}!
	  </h1>
	</div>
	<nav>
	  <a routerLink="/" routerLinkActive="active" [routerLinkActiveOptions]="{exact:true}">Home</a>&nbsp;
	  <a routerLink="/about" routerLinkActive="active" [routerLinkActiveOptions]="{exact:true}">About</a>&nbsp;
	  <a routerLink="/contact" routerLinkActive="active" [routerLinkActiveOptions]="{exact:true}">Contact</a>
	</nav>
	<router-outlet></router-outlet>

```

Along with the above routes we can also add a route that acts as a "catch-all" in case the user enters/navigates to an invalid route. 
We can create a "not-found" component which provides us the HTML to be displayed for the above cases.

> ng generate component not-found

With the not-found component created we can configure the route corresponding to it as the last route in the app.module.ts file:

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
      },
      {
        path: '**',
        component: NotFoundComponent
      }
    ];
```

The order of routes within the appRoutes array is _very_ important. 
As a general rule we should add routes that are more specific followed by the more generic ones.
The route that acts as a catch-all should be added at the end with the path equal to "**" wildcard characters which denotes any URL value will match it.
