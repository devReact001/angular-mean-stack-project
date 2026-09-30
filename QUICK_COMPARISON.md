# ⚖️ React vs Angular - Side by Side Comparison

This guide shows how to do the same thing in React (MERN) and Angular (MEAN). Perfect for React developers learning Angular!

---

## 1️⃣ Basic Component

### React

```jsx
// React component is just a function
function LoginPage() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  
  const handleLogin = async () => {
    const response = await fetch('/api/auth/login', {
      method: 'POST',
      body: JSON.stringify({ email, password })
    });
    const data = await response.json();
    localStorage.setItem('token', data.token);
  };
  
  return (
    <div>
      <input 
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Email"
      />
      <input 
        value={password}
        onChange={(e) => setPassword(e.target.value)}
        type="password"
        placeholder="Password"
      />
      <button onClick={handleLogin}>Login</button>
    </div>
  );
}
```

### Angular

```typescript
// Angular component is a class with decorator
@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent {
  email: string = '';
  password: string = '';
  
  constructor(private authService: AuthService) {}
  
  handleLogin() {
    this.authService.login(this.email, this.password)
      .subscribe(response => {
        localStorage.setItem('token', response.token);
      });
  }
}
```

```html
<!-- login.component.html -->
<div>
  <input 
    [(ngModel)]="email"
    placeholder="Email"
  />
  <input 
    [(ngModel)]="password"
    type="password"
    placeholder="Password"
  />
  <button (click)="handleLogin()">Login</button>
</div>
```

### Key Differences

| Aspect | React | Angular |
|--------|-------|---------|
| Syntax | JSX (HTML in JS) | Template + Component |
| Component Type | Function | Class |
| State | `useState` hook | Component properties |
| Data Binding | Manual (onChange) | Two-way (ngModel) |
| File Structure | One file | Two files (.ts + .html) |

---

## 2️⃣ State Management

### React (useState)

```jsx
function Counter() {
  const [count, setCount] = useState(0);
  
  const increment = () => setCount(count + 1);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>+</button>
    </div>
  );
}
```

### Angular (Component Property)

```typescript
@Component({
  selector: 'app-counter',
  template: `
    <div>
      <p>Count: {{ count }}</p>
      <button (click)="increment()">+</button>
    </div>
  `
})
export class CounterComponent {
  count: number = 0;
  
  increment() {
    this.count = this.count + 1;
  }
}
```

### Summary

| React | Angular |
|-------|---------|
| `useState(initialValue)` | `property: Type = initialValue;` |
| `setCount(newValue)` | `this.count = newValue;` |
| Trigger re-render | Automatic with Change Detection |

---

## 3️⃣ Handling User Input

### React (onChange)

```jsx
function SearchBox() {
  const [search, setSearch] = useState('');
  
  return (
    <input 
      value={search}
      onChange={(e) => setSearch(e.target.value)}
      placeholder="Search..."
    />
  );
}
```

### Angular (ngModel)

```typescript
@Component({
  template: `
    <input [(ngModel)]="search" placeholder="Search..." />
    <p>You typed: {{ search }}</p>
  `
})
export class SearchComponent {
  search: string = '';
}
```

### Key Difference

**React:** Manual two-way binding (onChange listener)
**Angular:** Automatic two-way binding ([(ngModel)])

---

## 4️⃣ Conditional Rendering

### React

```jsx
function Status() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  
  return (
    <div>
      {isLoggedIn ? (
        <p>Welcome back!</p>
      ) : (
        <p>Please log in</p>
      )}
    </div>
  );
}
```

### Angular

```typescript
@Component({
  template: `
    <div>
      <p *ngIf="isLoggedIn">Welcome back!</p>
      <p *ngIf="!isLoggedIn">Please log in</p>
    </div>
  `
})
export class StatusComponent {
  isLoggedIn: boolean = false;
}
```

### Comparison

| React | Angular |
|-------|---------|
| `{condition ? <A /> : <B />}` | `<A *ngIf="condition">` |
| Ternary operator | Structural directive |

---

## 5️⃣ Loops / Lists

### React

```jsx
function ProjectList() {
  const [projects, setProjects] = useState([]);
  
  return (
    <div>
      {projects.map(project => (
        <div key={project.id}>
          <h3>{project.name}</h3>
          <p>{project.description}</p>
        </div>
      ))}
    </div>
  );
}
```

### Angular

```typescript
@Component({
  template: `
    <div>
      <div *ngFor="let project of projects">
        <h3>{{ project.name }}</h3>
        <p>{{ project.description }}</p>
      </div>
    </div>
  `
})
export class ProjectListComponent {
  projects: any[] = [];
}
```

### Comparison

| React | Angular |
|-------|---------|
| `.map()` function | `*ngFor` directive |
| `key={id}` | Automatic |
| JavaScript loop | Template loop |

---

## 6️⃣ Making API Calls

### React (Fetch / Axios)

```jsx
function UserProfile() {
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    const fetchUser = async () => {
      const response = await fetch('/api/users/1');
      const data = await response.json();
      setUser(data);
    };
    
    fetchUser();
  }, []); // Empty dependency array = runs once
  
  return <div>{user?.name}</div>;
}
```

### Angular (Service + Observable)

```typescript
@Component({
  template: `<div>{{ user?.name }}</div>`
})
export class UserProfileComponent implements OnInit {
  user: any;
  
  constructor(private userService: UserService) {}
  
  ngOnInit() {
    this.userService.getUser(1).subscribe(data => {
      this.user = data;
    });
  }
}
```

```typescript
// userService.ts
@Injectable({
  providedIn: 'root'
})
export class UserService {
  constructor(private http: HttpClient) {}
  
  getUser(id: number) {
    return this.http.get(`/api/users/${id}`);
  }
}
```

### Comparison

| React | Angular |
|-------|---------|
| `useEffect` hook | `ngOnInit` lifecycle hook |
| `async/await` | `subscribe()` |
| Direct fetch | Service abstraction |
| State management | Observable stream |

---

## 7️⃣ Forms

### React (Manual)

```jsx
function LoginForm() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [errors, setErrors] = useState({});
  
  const handleSubmit = (e) => {
    e.preventDefault();
    
    const newErrors = {};
    if (!email) newErrors.email = 'Email required';
    if (!password) newErrors.password = 'Password required';
    
    if (Object.keys(newErrors).length > 0) {
      setErrors(newErrors);
      return;
    }
    
    // Submit form
    loginUser(email, password);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input 
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      {errors.email && <span>{errors.email}</span>}
      
      <input 
        value={password}
        onChange={(e) => setPassword(e.target.value)}
        type="password"
      />
      {errors.password && <span>{errors.password}</span>}
      
      <button type="submit">Login</button>
    </form>
  );
}
```

### Angular (Reactive Forms)

```typescript
@Component({
  template: `
    <form [formGroup]="loginForm" (ngSubmit)="handleSubmit()">
      <input formControlName="email" />
      <span *ngIf="email.invalid && email.touched">
        Email required
      </span>
      
      <input 
        formControlName="password" 
        type="password"
      />
      <span *ngIf="password.invalid && password.touched">
        Password required
      </span>
      
      <button type="submit">Login</button>
    </form>
  `
})
export class LoginFormComponent {
  loginForm: FormGroup;
  
  constructor(private fb: FormBuilder) {
    this.loginForm = this.fb.group({
      email: ['', Validators.required],
      password: ['', Validators.required]
    });
  }
  
  get email() {
    return this.loginForm.get('email');
  }
  
  get password() {
    return this.loginForm.get('password');
  }
  
  handleSubmit() {
    if (this.loginForm.valid) {
      const { email, password } = this.loginForm.value;
      this.loginUser(email, password);
    }
  }
}
```

### Comparison

| React | Angular |
|-------|---------|
| Manual state for each field | FormGroup manages all |
| Manual validation logic | Built-in Validators |
| Manual error display | Built-in touched/dirty states |
| More flexible | More structured |

---

## 8️⃣ Routing

### React Router

```jsx
import { BrowserRouter, Routes, Route, useNavigate } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/login" element={<LoginPage />} />
        <Route path="/dashboard" element={<Dashboard />} />
      </Routes>
    </BrowserRouter>
  );
}

function LoginPage() {
  const navigate = useNavigate();
  
  const handleLogin = async () => {
    await loginUser();
    navigate('/dashboard');
  };
  
  return <div>...</div>;
}
```

### Angular Router

```typescript
// app-routing.module.ts
const routes: Routes = [
  { path: 'login', component: LoginComponent },
  { path: 'dashboard', component: DashboardComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
```

```typescript
// login.component.ts
@Component({...})
export class LoginComponent {
  constructor(private router: Router) {}
  
  handleLogin() {
    this.authService.login().subscribe(() => {
      this.router.navigate(['/dashboard']);
    });
  }
}
```

### Comparison

| React | Angular |
|-------|---------|
| `<BrowserRouter>` wrapper | Separate routing module |
| `useNavigate` hook | Router injected service |
| Routes in JSX | Routes in config array |

---

## 9️⃣ Sharing Data (Props vs @Input)

### React (Props)

```jsx
// Parent component
function App() {
  const [userName, setUserName] = useState('John');
  
  return (
    <Greeting name={userName} />
  );
}

// Child component
function Greeting({ name }) {
  return <p>Hello, {name}!</p>;
}
```

### Angular (@Input)

```typescript
// Parent component
@Component({
  selector: 'app-root',
  template: `<app-greeting [name]="userName"></app-greeting>`
})
export class AppComponent {
  userName: string = 'John';
}

// Child component
@Component({
  selector: 'app-greeting',
  template: `<p>Hello, {{ name }}!</p>`
})
export class GreetingComponent {
  @Input() name: string;
}
```

### Comparison

| React | Angular |
|-------|---------|
| Function parameters | @Input decorator |
| Destructuring | Explicit property |
| Dynamic by default | One-way binding |

---

## 🔟 Sending Data to Parent

### React (Callbacks)

```jsx
// Parent
function App() {
  const handleChildClick = (message) => {
    console.log('Child said:', message);
  };
  
  return <Child onMessage={handleChildClick} />;
}

// Child
function Child({ onMessage }) {
  return (
    <button onClick={() => onMessage('Hello parent!')}>
      Click me
    </button>
  );
}
```

### Angular (@Output)

```typescript
// Parent
@Component({
  selector: 'app-root',
  template: `<app-child (onMessage)="handleChildClick($event)"></app-child>`
})
export class AppComponent {
  handleChildClick(message: string) {
    console.log('Child said:', message);
  }
}

// Child
@Component({
  selector: 'app-child',
  template: `<button (click)="sendMessage()">Click me</button>`
})
export class ChildComponent {
  @Output() onMessage = new EventEmitter<string>();
  
  sendMessage() {
    this.onMessage.emit('Hello parent!');
  }
}
```

### Comparison

| React | Angular |
|-------|---------|
| Function prop | @Output EventEmitter |
| Callback function | emit() method |
| Parameter is data | emit(data) |

---

## 1️⃣1️⃣ Custom Hooks vs Services

### React (Custom Hook)

```jsx
function useAuth() {
  const [user, setUser] = useState(null);
  const [isLoading, setIsLoading] = useState(false);
  
  const login = async (email, password) => {
    setIsLoading(true);
    const response = await fetch('/api/auth/login', {
      method: 'POST',
      body: JSON.stringify({ email, password })
    });
    const data = await response.json();
    setUser(data.user);
    setIsLoading(false);
  };
  
  return { user, isLoading, login };
}

// Usage in component
function LoginPage() {
  const { user, isLoading, login } = useAuth();
  
  return (
    <button onClick={() => login('email', 'password')}>
      {isLoading ? 'Loading...' : 'Login'}
    </button>
  );
}
```

### Angular (Service)

```typescript
@Injectable({
  providedIn: 'root'
})
export class AuthService {
  private user$ = new BehaviorSubject<User | null>(null);
  private isLoading$ = new BehaviorSubject<boolean>(false);
  
  constructor(private http: HttpClient) {}
  
  login(email: string, password: string) {
    this.isLoading$.next(true);
    return this.http.post('/api/auth/login', { email, password })
      .pipe(
        tap(response => {
          this.user$.next(response.user);
          this.isLoading$.next(false);
        })
      );
  }
  
  getUser() {
    return this.user$.asObservable();
  }
  
  isLoading() {
    return this.isLoading$.asObservable();
  }
}

// Usage in component
@Component({...})
export class LoginComponent {
  isLoading$ = this.authService.isLoading();
  
  constructor(private authService: AuthService) {}
  
  login() {
    this.authService.login('email', 'password').subscribe();
  }
}
```

### Comparison

| React | Angular |
|-------|---------|
| Custom hook function | Injectable service class |
| Multiple useState calls | Observables/BehaviorSubject |
| Direct return value | Observable stream |
| Used in components | Injected into components |

---

## 1️⃣2️⃣ Global State Management

### React (Context)

```jsx
const UserContext = createContext();

function UserProvider({ children }) {
  const [user, setUser] = useState(null);
  
  return (
    <UserContext.Provider value={{ user, setUser }}>
      {children}
    </UserContext.Provider>
  );
}

// Usage in component
function Profile() {
  const { user } = useContext(UserContext);
  return <p>{user?.name}</p>;
}
```

### Angular (Service + Subject)

```typescript
@Injectable({
  providedIn: 'root'
})
export class UserStore {
  private user$ = new BehaviorSubject<User | null>(null);
  
  getUser() {
    return this.user$.asObservable();
  }
  
  setUser(user: User) {
    this.user$.next(user);
  }
}

// Usage in component
@Component({...})
export class ProfileComponent {
  user$ = this.userStore.getUser();
  
  constructor(private userStore: UserStore) {}
}
```

### Comparison

| React | Angular |
|-------|---------|
| createContext() | Injectable service |
| Provider wrapper | Global service |
| useContext hook | Inject service |
| Context.Provider | BehaviorSubject |

---

## 📊 Feature Comparison Summary

| Feature | React | Angular |
|---------|-------|---------|
| Component | Function | Class |
| State | useState | Properties |
| Props | Parameters | @Input |
| Events | Callbacks | @Output |
| Async | async/await | Observables |
| Forms | Manual or libraries | Reactive Forms |
| Routing | React Router | Angular Router |
| Services | Custom hooks | @Injectable |
| Global state | Context | Services + Subjects |
| File structure | One file | Multiple files |
| Learning curve | Gentler | Steeper |

---

## 💡 Mental Model Translation

### React Mental Model → Angular Mental Model

**React:** "Everything is JavaScript, HTML lives inside JS"
**Angular:** "Separate concerns: Component logic (.ts) and template (.html)"

**React:** "Hooks do everything"
**Angular:** "Services handle logic, components handle UI"

**React:** "Promises and async/await"
**Angular:** "Observables and streams"

**React:** "Props and callbacks"
**Angular:** "@Input and @Output"

---

## 🎓 Transition Tips for React Developers

1. **Forget useState** - Use class properties instead
2. **Forget useEffect** - Use ngOnInit lifecycle hook
3. **Forget map()** - Use *ngFor in templates
4. **Forget .then()** - Use .subscribe() on Observables
5. **Forget Context** - Use Services with Subjects
6. **Embrace separating** - Template and logic go in different files
7. **Embrace TypeScript** - Use types everywhere
8. **Embrace RxJS** - Observables are powerful once you understand them

---

## 📚 Next Steps

1. Pick any React pattern above
2. Find the Angular equivalent
3. Look at commented Angular code to see it in action
4. Practice by building features

The concepts are similar—just different syntax! You've got this! 🚀

---

**Next:** Read [ANGULAR_PROJECT_STRUCTURE.md](ANGULAR_PROJECT_STRUCTURE.md) to understand how files are organized.
