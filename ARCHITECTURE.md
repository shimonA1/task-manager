# Task Manager - Technical Architecture

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Frontend (React)                     │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │   Pages      │  │  Components  │  │   Services   │  │
│  │ - Login      │  │ - TaskForm   │  │ - API calls  │  │
│  │ - Register   │  │ - TaskItem   │  │ - Auth       │  │
│  │ - Tasks      │  │              │  │              │  │
│  └──────────────┘  └──────────────┘  └──────────────┘  │
│            │                 │                │          │
│            └─────────────────┴────────────────┘          │
│                           │                              │
└───────────────────────────┼──────────────────────────────┘
                            │ HTTP/REST
                            ▼
┌─────────────────────────────────────────────────────────┐
│                  Backend (Node.js/Express)              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │   Routes     │  │ Controllers  │  │  Middleware  │  │
│  │ - Auth       │  │ - Auth       │  │ - JWT Auth   │  │
│  │ - Tasks      │  │ - Tasks      │  │ - CORS       │  │
│  └──────────────┘  └──────────────┘  └──────────────┘  │
│            │                 │                │          │
│            └─────────────────┴────────────────┘          │
│                           │                              │
└───────────────────────────┼──────────────────────────────┘
                            │ Mongoose ODM
                            ▼
┌─────────────────────────────────────────────────────────┐
│                    MongoDB Database                     │
│  ┌──────────────┐              ┌──────────────┐         │
│  │    Users     │              │    Tasks     │         │
│  │ Collection   │              │ Collection   │         │
│  └──────────────┘              └──────────────┘         │
└─────────────────────────────────────────────────────────┘
```

## 📊 Data Flow

### User Registration Flow
```
1. User fills registration form
   ↓
2. Frontend validates input
   ↓
3. POST /api/auth/register
   ↓
4. Backend validates data
   ↓
5. Hash password (bcrypt)
   ↓
6. Save user to MongoDB
   ↓
7. Generate JWT token
   ↓
8. Return token + user data
   ↓
9. Frontend stores token in localStorage
   ↓
10. Redirect to tasks page
```

### Task Creation Flow
```
1. User clicks "New Task"
   ↓
2. Fill task form
   ↓
3. POST /api/tasks with JWT token
   ↓
4. Middleware verifies token
   ↓
5. Extract user ID from token
   ↓
6. Validate task data
   ↓
7. Create task with user reference
   ↓
8. Save to MongoDB
   ↓
9. Return created task
   ↓
10. Frontend updates UI
```

## 🔐 Security Implementation

### Authentication Flow

1. **Password Security**
   - Passwords hashed using bcrypt (10 salt rounds)
   - Never stored in plain text
   - Password field excluded from default queries

2. **JWT Token**
   - Generated on login/register
   - Contains user ID
   - 30-day expiration
   - Stored in localStorage
   - Sent in Authorization header

3. **Protected Routes**
   - Middleware checks for valid token
   - Extracts user from token
   - Attaches user to request object
   - All task routes require authentication

### Authorization

- Users can only access their own tasks
- Task ownership verified on every operation
- User ID comparison: `task.user === req.user._id`

## 📦 Database Schema

### User Schema
```javascript
{
  username: String (unique, required, min: 3)
  email: String (unique, required, lowercase)
  password: String (required, hashed, min: 6)
  createdAt: Date (auto)
}
```

### Task Schema
```javascript
{
  title: String (required, max: 100)
  description: String (max: 500)
  status: Enum ['pending', 'in-progress', 'completed']
  priority: Enum ['low', 'medium', 'high']
  dueDate: Date (optional)
  user: ObjectId (ref: User, required)
  createdAt: Date (auto)
  updatedAt: Date (auto)
}
```

### Relationships
- One User → Many Tasks (1:N)
- Tasks reference User via ObjectId

## 🛣️ API Endpoints

### Authentication Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | /api/auth/register | Register new user | No |
| POST | /api/auth/login | Login user | No |
| GET | /api/auth/me | Get current user | Yes |

### Task Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | /api/tasks | Get all user tasks | Yes |
| GET | /api/tasks/:id | Get single task | Yes |
| POST | /api/tasks | Create new task | Yes |
| PUT | /api/tasks/:id | Update task | Yes |
| DELETE | /api/tasks/:id | Delete task | Yes |
| GET | /api/tasks/stats | Get task statistics | Yes |

### Query Parameters

**GET /api/tasks**
- `status`: Filter by status (pending, in-progress, completed)
- `priority`: Filter by priority (low, medium, high)
- `sort`: Sort order (dueDate, priority, createdAt)

## 🎨 Frontend Architecture

### Component Hierarchy
```
App
├── AuthProvider (Context)
│   ├── Login
│   ├── Register
│   └── Tasks
│       ├── TaskForm (Modal)
│       └── TaskItem (Multiple)
```

### State Management

1. **Global State (Context)**
   - User authentication state
   - Login/logout functions
   - Token management

2. **Local State (useState)**
   - Form inputs
   - Task list
   - Filters
   - Loading states
   - Error messages

3. **Persistent State**
   - JWT token in localStorage
   - Survives page refresh

### React Router

- `/` → Redirect to /tasks or /login
- `/login` → Login page (public)
- `/register` → Register page (public)
- `/tasks` → Task dashboard (private)

## 🔄 API Communication

### Axios Configuration

```javascript
// Base configuration
baseURL: /api
headers: { 'Content-Type': 'application/json' }

// Interceptor: Add token to all requests
Authorization: Bearer <token>
```

### Error Handling

1. **Backend Errors**
   - 400: Bad Request (validation errors)
   - 401: Unauthorized (invalid/missing token)
   - 404: Not Found
   - 500: Server Error

2. **Frontend Handling**
   - Try-catch blocks on API calls
   - Display error messages to user
   - Log errors to console
   - Graceful fallbacks

## 🐳 Docker Architecture

### Container Structure

```
docker-compose.yml
├── mongodb (service)
│   ├── Image: mongo:6.0
│   ├── Port: 27017
│   └── Volume: mongo-data
│
├── backend (service)
│   ├── Build: ./backend/Dockerfile
│   ├── Port: 5000
│   ├── Depends: mongodb
│   └── Network: task-manager-network
│
└── frontend (service)
    ├── Build: ./frontend/Dockerfile
    ├── Port: 3000 → 80
    ├── Depends: backend
    └── Network: task-manager-network
```

### Build Process

**Backend**
1. Node.js Alpine base image
2. Copy package files
3. Install dependencies
4. Copy source code
5. Expose port 5000
6. Run node server.js

**Frontend**
1. Build stage: Node.js Alpine
2. Install dependencies
3. Build React app
4. Production stage: Nginx Alpine
5. Copy build files to nginx
6. Configure nginx reverse proxy
7. Expose port 80

## 🚀 Performance Optimizations

### Backend
- Mongoose lean queries for read-only operations
- Indexed fields (email, username)
- Password field exclusion by default
- Connection pooling with MongoDB

### Frontend
- React.memo for component optimization
- Code splitting with React.lazy (can be added)
- Production build minification
- Nginx compression

## 🧪 Testing Strategy (Recommended)

### Backend Testing
```javascript
// Unit tests (Jest)
- Controller functions
- Middleware functions
- Model methods

// Integration tests
- API endpoints
- Database operations
- Authentication flow
```

### Frontend Testing
```javascript
// Unit tests (Jest + React Testing Library)
- Component rendering
- User interactions
- Form validation

// E2E tests (Cypress)
- Complete user flows
- Authentication
- CRUD operations
```

## 📈 Scalability Considerations

### Current Limitations
- Single server instance
- No caching layer
- No load balancing
- Monolithic architecture

### Future Improvements
1. **Horizontal Scaling**
   - Multiple backend instances
   - Load balancer (Nginx/HAProxy)
   - Session management (Redis)

2. **Database**
   - MongoDB replica set
   - Read replicas
   - Sharding for large datasets

3. **Caching**
   - Redis for session storage
   - Cache frequently accessed data
   - CDN for static assets

4. **Microservices**
   - Separate auth service
   - Separate task service
   - Message queue (RabbitMQ/Kafka)

## 🔍 Monitoring & Logging

### Recommended Tools
- **Backend**: Morgan (HTTP logging), Winston (app logging)
- **Frontend**: Sentry (error tracking)
- **Database**: MongoDB Atlas monitoring
- **Infrastructure**: Prometheus + Grafana

## 📚 Technology Stack Summary

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Frontend | React 18 | UI framework |
| Frontend | React Router | Navigation |
| Frontend | Axios | HTTP client |
| Backend | Node.js | Runtime |
| Backend | Express | Web framework |
| Backend | Mongoose | ODM |
| Database | MongoDB | Data storage |
| Auth | JWT | Authentication |
| Security | bcrypt | Password hashing |
| Container | Docker | Deployment |
| Reverse Proxy | Nginx | Production server |

---

This architecture provides a solid foundation for a production-ready task management application with room for future enhancements.
