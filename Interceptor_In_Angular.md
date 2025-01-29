# Angular HTTP Interceptor - Deep Dive

## What is an Angular Interceptor?
An **Interceptor** in Angular is a middleware that intercepts HTTP requests and responses before they reach the server or the application. It is useful for:
- Adding authentication tokens (JWT)
- Logging requests and responses
- Error handling
- Modifying requests or responses globally

## How Does an Interceptor Work?
Interceptors implement the `HttpInterceptor` interface provided by `@angular/common/http`. The `intercept` method is triggered for each HTTP request made by the application, allowing us to modify it before it is sent to the backend.

---

## Implementing an HTTP Interceptor in Angular

### Step 1: Generate an Interceptor Service
Run the following Angular CLI command:
```bash
ng generate service interceptors/auth-interceptor
```

### Step 2: Implement the Interceptor
Modify `auth-interceptor.service.ts` as follows:

```typescript
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent } from '@angular/common/http';
import { Observable } from 'rxjs';
import { AuthService } from '../services/auth.service';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  constructor(private authService: AuthService) {}

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const authToken = this.authService.getToken();
    const authReq = req.clone({
      setHeaders: {
        Authorization: `Bearer ${authToken}`
      }
    });
    return next.handle(authReq);
  }
}
```

### Step 3: Provide the Interceptor in `app.module.ts`
Modify `app.module.ts` to register the interceptor:

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { HttpClientModule, HTTP_INTERCEPTORS } from '@angular/common/http';
import { AuthInterceptor } from './interceptors/auth-interceptor.service';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, HttpClientModule],
  providers: [
    { provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true }
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### Step 4: Creating an AuthService
Modify `auth.service.ts` to include token retrieval:

```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  getToken(): string {
    return localStorage.getItem('token') || '';
  }
}
```

### Step 5: Making an HTTP Request
Use Angular's `HttpClient` service to make an API call:

```typescript
import { HttpClient } from '@angular/common/http';
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  constructor(private http: HttpClient) {}

  ngOnInit(): void {
    this.http.get('https://jsonplaceholder.typicode.com/posts').subscribe(response => {
      console.log(response);
    });
  }
}
```

### Step 6: Adding Error Handling to the Interceptor
Enhance `auth-interceptor.service.ts` to catch errors:

```typescript
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    return next.handle(req).pipe(
      catchError((error: HttpErrorResponse) => {
        console.error('HTTP Error:', error);
        return throwError(() => new Error(error.message));
      })
    );
  }
}
```

---

## Summary
✅ Interceptors modify HTTP requests and responses globally.
✅ We implemented an `AuthInterceptor` to attach authentication tokens.
✅ Registered the interceptor in `app.module.ts`.
✅ Implemented error handling within the interceptor.

Now, all outgoing HTTP requests will automatically include the `Authorization` header, ensuring secure API communication!

