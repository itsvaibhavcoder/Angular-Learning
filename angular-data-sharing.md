
# Sharing Data in Angular

In Angular, sharing data between components is a common task that can be done in several ways depending on the relationship between the components. Here's an overview of the most common techniques for sharing data in Angular:

## 1. Using `@Input()` and `@Output()` for Parent-Child Communication
This is the most common way to share data between a **parent** and a **child** component.

### A. Parent to Child (via `@Input()`)
The parent component passes data to the child component using **property binding** and the `@Input()` decorator.

### Example:

**Parent Component (HTML):**
```html
<app-child [childMessage]="parentMessage"></app-child>
```

**Parent Component (TypeScript):**
```typescript
@Component({
  selector: 'app-parent',
  template: `
    <app-child [childMessage]="messageFromParent"></app-child>
  `
})
export class ParentComponent {
  messageFromParent = "Hello from Parent!";
}
```

**Child Component (TypeScript):**
```typescript
@Component({
  selector: 'app-child',
  template: `
    Say: {{ childMessage }}
  `
})
export class ChildComponent {
  @Input() childMessage!: string;  // Data passed from the parent
}
```

### B. Child to Parent (via `@Output()` and EventEmitter)
The child component can send data to the parent using **event binding** and the `@Output()` decorator with an `EventEmitter`.

### Example:

**Child Component (TypeScript):**
```typescript
import { Component, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
    <button (click)="sendMessage()">Send Message to Parent</button>
  `
})
export class ChildComponent {
  @Output() messageEvent = new EventEmitter<string>();

  sendMessage() {
    this.messageEvent.emit('Hello from Child!');
  }
}
```

**Parent Component (HTML):**
```html
<app-child (messageEvent)="receiveMessage($event)"></app-child>
<p>{{ messageFromChild }}</p>
```

**Parent Component (TypeScript):**
```typescript
@Component({
  selector: 'app-parent',
  template: `
    <app-child (messageEvent)="receiveMessage($event)"></app-child>
    <p>{{ messageFromChild }}</p>
  `
})
export class ParentComponent {
  messageFromChild = '';

  receiveMessage(message: string) {
    this.messageFromChild = message;
  }
}
```

---

## 2. Using a Service for Sibling or Unrelated Components

Services allow you to share data across components that may not have a parent-child relationship (e.g., sibling or unrelated components). You can achieve this by using Angular's **Dependency Injection (DI)**.

### Steps:
1. Create a **service** that will hold the shared data or facilitate communication.
2. Inject the service into the components that need to share data.

### Example:

**Data Service (TypeScript):**
```typescript
import { Injectable } from '@angular/core';
import { BehaviorSubject } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class DataService {
  private messageSource = new BehaviorSubject<string>('default message');
  currentMessage = this.messageSource.asObservable();

  changeMessage(message: string) {
    this.messageSource.next(message);
  }
}
```

**Component 1 (Sends data):**
```typescript
import { Component } from '@angular/core';
import { DataService } from './data.service';

@Component({
  selector: 'app-sender',
  template: `
    <button (click)="sendMessage()">Send Message</button>
  `
})
export class SenderComponent {
  constructor(private dataService: DataService) {}

  sendMessage() {
    this.dataService.changeMessage('Hello from Sender!');
  }
}
```

**Component 2 (Receives data):**
```typescript
import { Component, OnInit } from '@angular/core';
import { DataService } from './data.service';

@Component({
  selector: 'app-receiver',
  template: `
    <p>{{ message }}</p>
  `
})
export class ReceiverComponent implements OnInit {
  message: string;

  constructor(private dataService: DataService) {}

  ngOnInit() {
    this.dataService.currentMessage.subscribe(message => this.message = message);
  }
}
```

---

## 3. Using LocalStorage or SessionStorage
You can use `localStorage` or `sessionStorage` to store data globally and share it between components.

### Example:
```typescript
localStorage.setItem('key', 'value');  // Set data
const value = localStorage.getItem('key');  // Get data
```

This approach is more suitable for data persistence between page reloads or for application-wide settings.

---

## 4. Using `@ViewChild` for Child Component Interaction
If the parent component needs access to the child component’s properties or methods directly, you can use `@ViewChild()`.

### Example:

**Parent Component (TypeScript):**
```typescript
@Component({
  selector: 'app-parent',
  template: `<app-child></app-child><button (click)="accessChild()">Access Child</button>`
})
export class ParentComponent {
  @ViewChild(ChildComponent) childComponent!: ChildComponent;

  accessChild() {
    console.log(this.childComponent.childMessage);  // Access child property
    this.childComponent.someMethod();  // Call child method
  }
}
```

**Child Component (TypeScript):**
```typescript
@Component({
  selector: 'app-child',
  template: `Say: {{ childMessage }}`
})
export class ChildComponent {
  childMessage = 'Hello from Child!';
  
  someMethod() {
    console.log('Method in Child Component called');
  }
}
```

---

## Summary:
- **Parent to Child**: Use `@Input()`.
- **Child to Parent**: Use `@Output()` with `EventEmitter`.
- **Sibling or Unrelated Components**: Use a shared **service**.
- **Access Child Properties or Methods**: Use `@ViewChild()`.
- **Global or Persistent Data**: Use `localStorage` or `sessionStorage`.

