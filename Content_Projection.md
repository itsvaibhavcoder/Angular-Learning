
# Content Projection in Angular

## What is Content Projection?

Content Projection is a powerful Angular feature that allows you to insert dynamic content into a component's template. It enables developers to pass HTML or other components into a child component and display them in predefined slots.

---

## Types of Content Projection

1. **Single-Slot Content Projection**:
   - Passes content into a single location in the child component.

2. **Multi-Slot Content Projection**:
   - Allows projecting different pieces of content into different slots using the `select` attribute.

3. **Conditional Content Projection**:
   - Dynamically projects content based on conditions using Angular directives like `*ngIf`.

---

## Examples

### 1. Single-Slot Content Projection

**Parent Component (HTML):**
```html
<app-child>
  <p>This is projected content.</p>
</app-child>
```

**Child Component (HTML):**
```html
<div>
  <ng-content></ng-content>
</div>
```

In this example, the `<p>` tag from the parent is projected into the `<ng-content>` tag of the child component.

---

### 2. Multi-Slot Content Projection

**Parent Component (HTML):**
```html
<app-child>
  <h1 slot="header">Header Content</h1>
  <p slot="body">Body Content</p>
</app-child>
```

**Child Component (HTML):**
```html
<div>
  <div class="header">
    <ng-content select="[slot='header']"></ng-content>
  </div>
  <div class="body">
    <ng-content select="[slot='body']"></ng-content>
  </div>
</div>
```

Here, the content with `slot="header"` is projected into the `ng-content` with `select="[slot='header']"`, and similarly for the body.

---

### 3. Conditional Content Projection

**Parent Component (HTML):**
```html
<app-child>
  <p *ngIf="show">This is conditionally projected content.</p>
</app-child>
```

**Child Component (HTML):**
```html
<div>
  <ng-content></ng-content>
</div>
```

The content will be projected only if the `*ngIf` condition in the parent component evaluates to `true`.

---

## Use Cases

1. **Reusable Components**:
   - Create components that accept dynamic content, like dialogs, tabs, or cards.

2. **Customization**:
   - Allow users to customize specific parts of a component without changing its structure.

3. **Dynamic Content**:
   - Insert runtime-determined content into components.

---

## Key Points

1. **ng-content**:
   - Acts as a placeholder for the projected content in the child component.

2. **Default Content**:
   - Default content can be included within the child component and will be displayed if no content is projected.

3. **Selectors**:
   - Use `select` in `ng-content` to specify which content to project into a specific slot.

4. **Lifecycle**:
   - The projected content is compiled and linked with the parent component's context, not the child component.

---

Content Projection enhances Angular's component-based architecture by enabling flexibility and reusability.
