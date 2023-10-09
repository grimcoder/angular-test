### 4. Directives for Online Learning Platform:

Angular Directives are classes that add behaviors or transforms the DOM. They are a powerful feature that allows you to create reusable DOM-manipulating components without an associated template. There are three types of directives in Angular:

1. **Component Directives**: These are the most common directives and are associated with a view (template). Every Angular component is essentially a directive with a template.
2. **Attribute Directives**: These change the behavior, appearance, or layout of an element, component, or another directive.
3. **Structural Directives**: These change the DOM layout by adding/removing elements.

#### 4.1. Using Built-in Directives:

**Structural Directives**:

Let's say our Online Learning Platform displays a list of courses. To conditionally show/hide courses based on a user's subscription status:

```html
<!-- Using *ngIf -->
<div *ngIf="user.isSubscribed">
    <app-course-list></app-course-list>
</div>
```

Or to loop through the courses:

```html
<!-- Using *ngFor -->
<div *ngFor="let course of courses">
    <app-course-card [course]="course"></app-course-card>
</div>
```

**Attribute Directives**:

To change the styling of a course that's marked as "featured":

```html
<!-- Using ngClass -->
<div [ngClass]="{'featured': course.isFeatured}">
    {{ course.name }}
</div>
```

#### 4.2. Creating Custom Directives:

Imagine you want a directive to highlight courses that have been recently added.

1. **Generate Directive**:

   ```bash
   ng generate directive highlightNew
   ```

2. **Implement the Directive**:

   `highlight-new.directive.ts`:

   ```typescript
   import { Directive, ElementRef, Input, Renderer2, OnInit } from '@angular/core';

   @Directive({
     selector: '[appHighlightNew]'
   })
   export class HighlightNewDirective implements OnInit {

     @Input() creationDate: Date;

     constructor(private el: ElementRef, private renderer: Renderer2) {}

     ngOnInit(): void {
       const currentDate = new Date();
       const daysDifference = (currentDate.getTime() - new Date(this.creationDate).getTime()) / (1000 * 3600 * 24);
       
       if (daysDifference <= 7) {  // if the course is less than 7 days old
         this.renderer.setStyle(this.el.nativeElement, 'background-color', 'yellow');
       }
     }
   }
   ```

3. **Usage**:

   In the course list:

   ```html
   <div *ngFor="let course of courses" [appHighlightNew]="course.creationDate">
      {{ course.name }}
   </div>
   ```

#### 4.3. HostListener and HostBinding:

**HostListener** lets you listen to events of the host element. For instance, if you want to listen to mouse events to show additional information about a course:

```typescript
@HostListener('mouseenter') onMouseEnter() {
  // Show more info about the course
}

@HostListener('mouseleave') onMouseLeave() {
  // Hide info
}
```

**HostBinding** lets you bind properties of the host element. For example, to bind a class based on a condition:

```typescript
@HostBinding('class.featured') isFeatured = true;  // If 'isFeatured' is true, the 'featured' class will be added to the host element.
```
