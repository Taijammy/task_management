# TaskFlow — Task Management Dashboard

A full-stack Task Management Dashboard with role-based authentication built with React + Express + MongoDB.

![React](https://img.shields.io/badge/React-18-blue) ![Node](https://img.shields.io/badge/Node.js-20-green) ![MongoDB](https://img.shields.io/badge/MongoDB-7.0-green) ![Tailwind](https://img.shields.io/badge/Tailwind-3-blue)

---

## 🚀 Features

- **Authentication** — Register, login, logout with JWT tokens
- **Role-Based Access** — Admin and User roles with different permissions
- **Admin Panel** — Manage users, view platform stats, change roles
- **Dashboard** — Live stats: total, completed, in-progress, pending tasks
- **Task CRUD** — Create, read, update, delete tasks
- **Filters** — Filter by status, priority, search by keyword
- **Responsive** — Works on desktop and mobile
- **Protected Routes** — Frontend + backend route protection

---

## 🛠 Tech Stack

### Frontend
- React 18 + Vite
- Tailwind CSS
- React Router v6
- Axios
- Context API

### Backend
- Node.js + Express.js
- MongoDB + Mongoose
- JWT Authentication
- bcryptjs
- express-validator

---

## 📁 Project Structure

```
task_management/
├── src/                          ← React frontend
│   ├── api/
│   │   ├── axios.js              ← Axios instance + interceptors
│   │   ├── auth.api.js           ← Auth API calls
│   │   ├── tasks.api.js          ← Task API calls
│   │   └── admin.api.js          ← Admin API calls
│   ├── context/
│   │   └── AuthContext.jsx       ← Global auth state + role helpers
│   ├── hooks/
│   │   ├── useTasks.js           ← Task state + CRUD logic
│   │   └── useToast.js           ← Toast notifications
│   ├── components/
│   │   ├── layout/
│   │   │   ├── Layout.jsx        ← Sidebar + topbar wrapper
│   │   │   ├── Sidebar.jsx       ← Navigation with role badge
│   │   │   └── ProtectedRoute.jsx← Auth + AdminRoute guards
│   │   ├── tasks/
│   │   │   ├── TaskCard.jsx      ← Single task card
│   │   │   └── TaskForm.jsx      ← Create/edit form
│   │   └── ui/
│   │       ├── Badge.jsx         ← Priority + status badges
│   │       ├── Modal.jsx         ← Reusable modal
│   │       ├── Toast.jsx         ← Notifications
│   │       └── Spinner.jsx       ← Loading spinner
│   ├── pages/
│   │   ├── Login.jsx
│   │   ├── Register.jsx
│   │   ├── Dashboard.jsx
│   │   ├── Tasks.jsx
│   │   └── AdminPanel.jsx        ← Admin only
│   ├── App.jsx
│   ├── main.jsx
│   └── index.css
│
├── backend/                      ← Express API
│   ├── config/db.js              ← MongoDB connection
│   ├── models/
│   │   ├── user.model.js         ← User schema with roles
│   │   └── task.model.js         ← Task schema
│   ├── controllers/
│   │   ├── auth.controller.js
│   │   ├── task.controller.js
│   │   └── admin.controller.js
│   ├── routes/
│   │   ├── auth.routes.js
│   │   ├── task.routes.js
│   │   └── admin.routes.js
│   ├── middleware/
│   │   ├── auth.middleware.js    ← JWT + adminOnly + authorizeRoles
│   │   └── validate.middleware.js
│   ├── server.js
│   ├── package.json
│   └── .env.example
│
├── index.html
├── package.json
├── vite.config.js
├── tailwind.config.js
└── README.md
```

---

## ⚙️ Setup Instructions

### Prerequisites
- Node.js v18+
- MongoDB (local) or MongoDB Atlas (cloud)
- Git

---

### 1. Clone the repository
```bash
git clone https://github.com/Taijammy/task_management.git
cd task-management
```

---

### 2. Setup Backend

```bash
cd backend
npm install
cp .env.example .env
```

Open `backend/.env` and fill in your values:
```env
PORT=5000
NODE_ENV=development
MONGO_URI=mongodb://localhost:27017/taskflow
JWT_SECRET=your_long_random_secret_here_min_32_chars
JWT_EXPIRES_IN=7d
COOKIE_MAX_AGE_MS=604800000
CLIENT_ORIGIN=http://localhost:5173
```

---

### 3. Start MongoDB

```bash
# Linux
sudo systemctl start mongod

# macOS
brew services start mongodb-community

# Windows
net start MongoDB
```

---

### 4. Start Backend Server

```bash
cd backend
npm run dev
```

You should see:
```
🚀 Server running in development mode on port 5000
✅ MongoDB connected: localhost
```

---

### 5. Setup Frontend

Open a new terminal:

```bash
cd task-management
npm install
npm run dev
```

You should see:
```
➜ Local: http://localhost:5173/
```

---

### 6. Open in Browser

```
http://localhost:5173
```

---

### 7. Create Admin Account

Register your **first account** — it automatically becomes Admin:
```
http://localhost:5173/register
```

> The first registered user is automatically assigned the Admin role.
> All subsequent users get the User role.

---

## 🔌 API Endpoints

### Auth
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| POST | `/api/auth/register` | Public | Create account |
| POST | `/api/auth/login` | Public | Login, returns JWT |
| POST | `/api/auth/logout` | Public | Clear session |
| GET | `/api/auth/me` | Protected | Get current user |

### Tasks
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| GET | `/api/tasks` | Protected | Get tasks + stats |
| GET | `/api/tasks/:id` | Protected | Get single task |
| POST | `/api/tasks` | Protected | Create task |
| PUT | `/api/tasks/:id` | Protected | Update task |
| DELETE | `/api/tasks/:id` | Protected | Delete task |

### Admin
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| GET | `/api/admin/stats` | Admin only | Platform-wide stats |
| GET | `/api/admin/users` | Admin only | All users + task counts |
| PUT | `/api/admin/users/:id/role` | Admin only | Change user role |
| PUT | `/api/admin/users/:id/status` | Admin only | Activate/deactivate user |
| DELETE | `/api/admin/users/:id` | Admin only | Delete user + their tasks |

### Query Parameters (GET /api/tasks)
| Param | Example | Description |
|-------|---------|-------------|
| `status` | `?status=Pending` | Filter by status |
| `priority` | `?priority=High` | Filter by priority |
| `search` | `?search=design` | Search title/description |
| `sort` | `?sort=dueDate` | Sort results |

---

## 🔒 Role-Based Access

| Feature | User | Admin |
|---------|------|-------|
| View own tasks | ✅ | ✅ |
| Create/Edit/Delete own tasks | ✅ | ✅ |
| View ALL users tasks | ❌ | ✅ |
| Access Admin Panel | ❌ | ✅ |
| Manage user roles | ❌ | ✅ |
| View platform stats | ❌ | ✅ |
| Delete any user | ❌ | ✅ |

---

## 📦 API Response Format

```json
// Success
{
  "success": true,
  "message": "Task created successfully",
  "data": { "task": {} }
}

// Error
{
  "success": false,
  "error": "Validation failed",
  "details": [{ "field": "title", "message": "Title is required" }]
}
```

---

## 🧪 Test the API

```bash
# Health check
curl http://localhost:5000/api/health

# Register
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name":"John Doe","email":"john@example.com","password":"secret123"}'

# Login and save token
TOKEN=$(curl -s -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"john@example.com","password":"secret123"}' \
  | grep -o '"token":"[^"]*"' | cut -d'"' -f4)

# Create task
curl -X POST http://localhost:5000/api/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $TOKEN" \
  -d '{"title":"My task","priority":"High","status":"Pending","dueDate":"2026-06-01"}'

# Get all tasks
curl http://localhost:5000/api/tasks \
  -H "Authorization: Bearer $TOKEN"
```

---

## 🔐 Environment Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `PORT` | Server port | `5000` |
| `NODE_ENV` | Environment | `development` |
| `MONGO_URI` | MongoDB connection string | `mongodb://localhost:27017/taskflow` |
| `JWT_SECRET` | JWT signing secret (min 32 chars) | `long_random_string` |
| `JWT_EXPIRES_IN` | Token expiry | `7d` |
| `COOKIE_MAX_AGE_MS` | Cookie max age in ms | `604800000` |
| `CLIENT_ORIGIN` | Frontend URL for CORS | `http://localhost:5173` |

---
