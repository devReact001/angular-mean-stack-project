# ⚡ Angular MEAN Stack - Quick Reference (2 Minutes)

## 🎯 What This App Does

A **project management application** where users can:
- Sign up / Login
- Create projects
- Add tasks to projects
- Manage team members
- Track progress

---

## 🔌 API Endpoints (Backend)

```
POST   /api/auth/register          Create new account
POST   /api/auth/login             Login to account
POST   /api/auth/logout            Logout

GET    /api/projects               Get all user's projects
POST   /api/projects               Create new project
PUT    /api/projects/:id           Update project
DELETE /api/projects/:id           Delete project

GET    /api/tasks                  Get all tasks
POST   /api/tasks                  Create task
PUT    /api/tasks/:id              Update task
DELETE /api/tasks/:id              Delete task

GET    /api/users/:id              Get user info
```

---

## 🛠️ Tech Stack (Simplified)

### What Runs Where?

```
Your Browser (Frontend)
├─ Runs on: localhost:4200
├─ Technology: Angular (JavaScript framework)
├─ Shows: Pages, buttons, forms
└─ Does: Makes API calls to backend

↓ (HTTP Requests)

Your Computer/Server (Backend)
├─ Runs on: localhost:3000
├─ Technology: Express.js (JavaScript framework)
├─ Handles: API requests, authentication, database
└─ Does: Processes data, talks to database

↓ (SQL/Document Queries)

Database (Data Storage)
├─ Technology: MongoDB (NoSQL database)
├─ Stores: Users, projects, tasks
└─ Does: Keeps data persistent
```

---

## 🔐 Authentication Flow

```
1. User enters email & password in login form

2. Frontend sends request to backend:
   POST /api/auth/login
   { "email": "user@example.com", "password": "123" }

3. Backend:
   - Finds user in database
   - Checks if password is correct
   - Creates JWT token (secure token)
   - Sends back: { "token": "xyz123...", "user": {...} }

4. Frontend:
   - Saves token to localStorage
   - Adds token to all future requests

5. Every request includes token:
   GET /api/projects
   Headers: { "Authorization": "Bearer xyz123..." }

6. Backend checks:
   - Is token valid? (not expired, not tampered with)
   - If yes → Allow access
   - If no → Return error "Unauthorized"
```

---

## 📂 Key Files & What They Do

### Frontend (Angular)

```
src/app/
├── services/
│   ├── auth.service.ts          Makes login/logout API calls
│   ├── project.service.ts       Handles project API calls
│   └── task.service.ts          Handles task API calls
│
├── guards/
│   └── auth.guard.ts            Protects routes (checks login)
│
├── pages/
│   ├── login/                   Login page component
│   ├── dashboard/               Main dashboard
│   └── projects/                Projects list
│
└── models/
    ├── user.model.ts            User data shape
    ├── project.model.ts         Project data shape
    └── task.model.ts            Task data shape
```

### Backend (Express)

```
src/
├── models/                      MongoDB schemas
│   ├── User.ts
│   ├── Project.ts
│   └── Task.ts
│
├── controllers/                 Business logic
│   ├── authController.ts
│   ├── projectController.ts
│   └── taskController.ts
│
├── routes/                      API endpoints
│   ├── authRoutes.ts
│   ├── projectRoutes.ts
│   └── taskRoutes.ts
│
├── middleware/
│   └── authMiddleware.ts        Checks JWT token
│
└── config/
    └── db.ts                    Database connection
```

---

## 🔄 How Request/Response Works

### Example: Getting All Projects

**Frontend (Angular Component):**
```typescript
// 1. User clicks "Get Projects" button
// 2. Component calls service
this.projectService.getProjects().subscribe(projects => {
  this.projects = projects; // Store in component
  // 3. Page automatically updates with projects
});
```

**Service (Makes API call):**
```typescript
// Makes HTTP request to backend
getProjects() {
  return this.http.get('/api/projects');
}
```

**Backend (Express):**
```typescript
// 1. Route receives GET /api/projects
// 2. authMiddleware checks JWT token
// 3. Controller gets projects from database
// 4. Returns: [ { id: 1, name: "Project A" }, ... ]
```

**Browser Network:**
```
Request:
GET /api/projects
Headers: { Authorization: "Bearer token..." }

Response (200 OK):
[
  { id: "1", name: "Project A", createdAt: "2025-01-15" },
  { id: "2", name: "Project B", createdAt: "2025-01-20" }
]
```

---

## 🐛 Debugging Checklist

**If login fails:**
- [ ] Check backend is running (port 3000)
- [ ] Check credentials are correct
- [ ] Check network tab in browser DevTools
- [ ] Look for error message in browser console

**If API calls fail:**
- [ ] Check token is saved in localStorage
- [ ] Check token format (should be in Authorization header)
- [ ] Check backend server is running
- [ ] Check MongoDB connection

**If page doesn't update:**
- [ ] Check component is subscribing to service
- [ ] Check unsubscribe logic (no memory leaks)
- [ ] Check data is being assigned to component property
- [ ] Check template is using *ngFor or async pipe

**If page won't load:**
- [ ] Check auth guard is allowing access
- [ ] Check token is still valid (not expired)
- [ ] Check route path is correct in app routing

---

## 💡 Key Concepts at a Glance

### Component
A piece of UI (login page, project card, task list)
- Has HTML template
- Has TypeScript logic
- Can receive data (input)
- Can send data out (output)

### Service
Handles business logic (API calls, authentication)
- Injected into components
- Reusable across components
- Makes HTTP calls
- Manages data

### Guard
Protects routes (checks if user is logged in before showing page)
- Runs before component loads
- Can allow or deny access
- Checks JWT token

### Observable
Represents data that arrives in the future
- Used for API calls
- Used for user input
- Subscribe to get data when ready

### Dependency Injection
Angular automatically provides services where needed
- Don't create services with `new`
- Ask for it in constructor
- Angular handles the rest

---

## 🎯 Data Flow Diagram

```
┌─────────────────────────────────────────────────────────┐
│                    USER BROWSER                          │
│  ┌────────────────────────────────────────────────────┐ │
│  │            Angular Frontend (Port 4200)            │ │
│  │                                                    │ │
│  │  Component → Service → HTTP Request               │ │
│  │                                                    │ │
│  │  Stores token: localStorage                       │ │
│  │  Updates page: Template bindings                  │ │
│  └────────────────────────────────────────────────────┘ │
└─────────────┬──────────────────────────────────────────┘
              │ HTTP (JSON)
              ↓
┌─────────────────────────────────────────────────────────┐
│                  YOUR SERVER                            │
│  ┌────────────────────────────────────────────────────┐ │
│  │            Express Backend (Port 3000)             │ │
│  │                                                    │ │
│  │  Route → Middleware (check token)                 │ │
│  │       → Controller (business logic)               │ │
│  │       → Model (database query)                    │ │
│  │       → Response (JSON data)                      │ │
│  └────────────────────────────────────────────────────┘ │
└─────────────┬──────────────────────────────────────────┘
              │ Mongoose Query
              ↓
┌─────────────────────────────────────────────────────────┐
│                  MongoDB DATABASE                       │
│                                                         │
│  Collections: users, projects, tasks                   │
│  Stores: All application data                          │
│  Persists: Even after server restarts                  │
└─────────────────────────────────────────────────────────┘
```

---

## ✅ Common Beginner Mistakes

| Mistake | Wrong | Right |
|---------|-------|-------|
| Not subscribing to Observable | `const data = this.service.getProjects();` | `this.service.getProjects().subscribe(data => { ... });` |
| Accessing data before it loads | `console.log(this.projects[0].name);` inside ngOnInit before subscription | `console.log(this.projects[0].name);` inside subscribe callback |
| Forgetting to inject service | No constructor parameter | `constructor(private service: AuthService) {}` |
| Not checking localStorage | Assume token exists | `const token = localStorage.getItem('token');` |
| Wrong HTTP method | `get()` for creating | `post()` for POST requests |

---

## 📚 File Reading Order (Quick Path)

1. **This file** ← You're here (2 min)
2. `ANGULAR_CONCEPTS_EXPLAINED.md` (20 min)
3. `commented_files/server/db.ts.commented` (10 min)
4. `commented_files/client/services/auth.service.ts.commented` (15 min)
5. You're ready to code!

---

## 🚀 Quick Commands (When Setup Complete)

```bash
# Terminal 1: Start backend
cd server
npm install
npm start

# Terminal 2: Start frontend
cd client
npm install
ng serve

# Open in browser
# Frontend: http://localhost:4200
# Backend API: http://localhost:3000
```

---

## 💬 Questions?

**Q: Which port does what?**
- Port 4200: Angular frontend (what you see in browser)
- Port 3000: Express backend (where API runs)

**Q: Where does data come from?**
- Frontend asks backend → Backend asks MongoDB → Data flows back

**Q: What's a JWT token?**
- A secure string that proves you're logged in
- Backend checks it on every request
- Expires after time (for security)

**Q: Can I use this without understanding all the code?**
- Yes! But understanding makes you dangerous (in a good way)
- Start simple, then dive deeper into commented files

---

## 🎓 Next Steps

✅ You've got the overview!

**Next:** Read [ANGULAR_CONCEPTS_EXPLAINED.md](ANGULAR_CONCEPTS_EXPLAINED.md) to understand each piece deeply.
