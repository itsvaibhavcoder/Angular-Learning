
# Template-driven Forms in Angular

**Template-driven forms** in Angular are a simple and declarative way to create and manage forms. They rely on Angular's two-way data binding and directives in the HTML template to handle form inputs and validation. This approach is best suited for small to medium-sized forms where simplicity is a priority.

## Key Features of Template-driven Forms:
1. **Declarative Syntax**: Forms are created primarily using HTML and Angular directives like `[(ngModel)]`, `#templateRef`, and `ngForm`.
2. **Two-way Data Binding**: Uses Angular's `[(ngModel)]` directive for real-time synchronization of form inputs with the underlying model.
3. **Automatic Form Control Management**: Angular creates `FormControl` objects behind the scenes for each input with `ngModel`.
4. **Simplicity**: Suitable for straightforward forms without complex validation or interactions.

---

## Steps to Create a Template-driven Form

### 1. Import the Required Modules
Ensure that the **FormsModule** is imported into your Angular module:

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, FormsModule],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

---

### 2. Define the Model
In the component's TypeScript file, create a model to bind the form data:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  user = {
    name: '',
    email: '',
    password: ''
  };

  onSubmit() {
    console.log('Form submitted:', this.user);
  }
}
```

---

### 3. Create the HTML Template
Use Angular directives to bind the form and inputs:

```html
<form #userForm="ngForm" (ngSubmit)="onSubmit()">
  <div>
    <label for="name">Name:</label>
    <input
      type="text"
      id="name"
      name="name"
      [(ngModel)]="user.name"
      required
    />
  </div>

  <div>
    <label for="email">Email:</label>
    <input
      type="email"
      id="email"
      name="email"
      [(ngModel)]="user.email"
      required
    />
  </div>

  <div>
    <label for="password">Password:</label>
    <input
      type="password"
      id="password"
      name="password"
      [(ngModel)]="user.password"
      required
      minlength="6"
    />
  </div>

  <button type="submit" [disabled]="userForm.invalid">Submit</button>
</form>
```

---

## Key Directives Used
1. **`[(ngModel)]`**: Binds form input fields to the component model for two-way data binding.
2. **`#userForm="ngForm"`**: References the form object for validations and status checks.
3. **`ngSubmit`**: Handles form submission.
4. **Validation Directives**:
   - `required`: Marks a field as required.
   - `minlength` and `maxlength`: Set length constraints.
   - `pattern`: Validates the input against a regex.

---

## Advantages of Template-driven Forms
1. Easy to implement for simple use cases.
2. Minimal setup compared to reactive forms.
3. Built-in validation directives make validation straightforward.
4. Cleaner and less boilerplate code.

---

## Limitations
1. Less scalable for large forms or complex validation logic.
2. Harder to test programmatically compared to reactive forms.
3. Limited flexibility for dynamic form controls.

For more complex scenarios, consider using **Reactive Forms**, which provide more control over the form structure and validation.
