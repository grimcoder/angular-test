### 2. Routing & Navigation:

Angular’s Router enables navigation between views or components as users perform tasks. It’s one of the pivotal aspects to understand for building a Single Page Application (SPA) like our Online Learning Platform.

#### 2.1. **Setting up routing for a single-page application**

**Step-by-step instructions**:

1. **Setting up Angular Router**:

    a. If not already added during project creation, install Angular Router:
    ```
    ng add @angular/router
    ```

    b. In your `AppModule` (typically `app.module.ts`), ensure the `RouterModule` is imported.

2. **Creating Components for Routes**:

    Before setting up routes, ensure you have the components you want to navigate to. For our platform, let's consider:
    - HomeComponent
    - CoursesComponent
    - CourseDetailComponent
    - UserProfileComponent
    - AdminComponent

    Create them using Angular CLI:
    ```
    ng generate component home
    ng generate component courses
    ng generate component course-detail
    ng generate component user-profile
    ng generate component admin
    ```

3. **Defining Routes**:

    In your main application module or a dedicated routing module, define the routes:

    ```typescript
    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'courses', component: CoursesComponent },
      { path: 'courses/:id', component: CourseDetailComponent },
      { path: 'profile', component: UserProfileComponent },
      { path: 'admin', component: AdminComponent }
    ];
    ```

    - The empty path (`''`) typically leads to your homepage.
    - `:id` is a dynamic segment, allowing you to navigate to details of different courses based on their ID.

4. **Registering Routes**:

    Register your routes using `RouterModule.forRoot()` in your main application module:

    ```typescript
    @NgModule({
      imports: [RouterModule.forRoot(routes)],
      exports: [RouterModule]
    })
    export class AppModule { }
    ```

5. **Router Outlet**:

    In your main app component template (typically `app.component.html`), add the `<router-outlet></router-outlet>` directive. This directive tells Angular where to display the routed views.

    ```html
    <router-outlet></router-outlet>
    ```

6. **Linking to Routes**:

    In your components or templates, use the `RouterLink` directive to allow users to navigate to different parts of the app.

    ```html
    <a routerLink="/courses">View All Courses</a>
    ```

#### 2.2. **Navigation paths, route guards, and lazy loading**

1. **Navigation Paths with Parameters**:

    For dynamic routes, like viewing a specific course:

    ```html
    <a [routerLink]="['/courses', course.id]">{{ course.title }}</a>
    ```

2. **Route Guards**:

    These are used to control whether the user can navigate to or navigate away from a given route. Perfect for our `AdminComponent`.

    a. Generate a guard:
    ```
    ng generate guard auth
    ```

    b. Implement the `CanActivate` method in `auth.guard.ts` to decide if the route should be activated:

    ```typescript
    canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): boolean {
        // Logic to check if user is authenticated
        if (this.authService.isAuthenticated()) {
            return true;
        } else {
            this.router.navigate(['/login']);
            return false;
        }
    }
    ```

    c. Add the guard to the route:

    ```typescript
    { path: 'admin', component: AdminComponent, canActivate: [AuthGuard] }
    ```

3. **Lazy Loading**:

    For large applications, you might want modules (like `AdminModule`) to load lazily.

    a. Ensure your feature module has a default route:

    ```typescript
    const routes: Routes = [
      { path: '', component: AdminComponent }
    ];
    ```

    b. Update the main route configuration to use `loadChildren`:

    ```typescript
    { path: 'admin', loadChildren: () => import('./admin/admin.module').then(m => m.AdminModule) }
    ```