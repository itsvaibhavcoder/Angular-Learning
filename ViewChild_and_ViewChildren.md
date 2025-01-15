
# `@ViewChild` and `@ViewChildren` in Angular

## What are `@ViewChild` and `@ViewChildren`?

- **`@ViewChild`**: Used to get a reference to a single DOM element or a single component in the component's view (HTML template).
- **`@ViewChildren`**: Used to get references to multiple DOM elements or components (i.e., a collection) in the component's view.

---

## How They Work

1. **`@ViewChild`**:
   - Retrieves the first matching element/component in the template.
   - Typically used when you want to interact with or manipulate a specific element/component.

2. **`@ViewChildren`**:
   - Retrieves all matching elements/components in the template as a `QueryList`.
   - Useful when you need to perform operations on a group of elements/components.

---

## Examples

### 1. Using `@ViewChild`
Suppose we want to manipulate an input box (e.g., setting focus):

**Template (HTML):**
```html
<input #inputBox type="text" placeholder="Enter your name" />
<button (click)="focusInput()">Focus Input</button>
```

**Component (TypeScript):**
```typescript
import { Component, ViewChild, ElementRef } from '@angular/core';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
})
export class ExampleComponent {
  @ViewChild('inputBox') inputBox!: ElementRef;

  focusInput() {
    this.inputBox.nativeElement.focus(); // Sets focus on the input box
  }
}
```

---

### 2. Using `@ViewChildren`
Suppose we have multiple buttons, and we want to change their text.

**Template (HTML):**
```html
<button #buttonElement>Button 1</button>
<button #buttonElement>Button 2</button>
<button #buttonElement>Button 3</button>
<button (click)="changeButtonText()">Change Button Text</button>
```

**Component (TypeScript):**
```typescript
import { Component, ViewChildren, QueryList, ElementRef } from '@angular/core';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
})
export class ExampleComponent {
  @ViewChildren('buttonElement') buttons!: QueryList<ElementRef>;

  changeButtonText() {
    this.buttons.forEach((button, index) => {
      button.nativeElement.textContent = `New Text ${index + 1}`;
    });
  }
}
```

---

## Use Cases

### `@ViewChild` Use Cases:
1. Accessing a single DOM element to:
   - Set focus.
   - Change styles or properties dynamically.
   - Call methods on a child component.

### `@ViewChildren` Use Cases:
1. Working with multiple similar elements/components:
   - Iterating over them to apply changes.
   - Dynamically managing a collection of child components.

---

## Key Points

1. **Timing**:
   - The query (`@ViewChild` or `@ViewChildren`) is resolved after the view is initialized.
   - Access these references in the `ngAfterViewInit` lifecycle hook to ensure they are available.

2. **Accessing Child Components**:
   - You can also use `@ViewChild` and `@ViewChildren` to interact with child components directly, allowing you to call their methods or access their properties.

3. **Use with Services**:
   - Often used for interacting with child components in cases where service-based communication isn't necessary.

---

This makes `@ViewChild` and `@ViewChildren` powerful tools for manipulating the DOM and components declaratively in Angular.
