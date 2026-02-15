# 🎯 Task Manager - Full Stack Application

A modern, feature-rich task management application built with the MERN stack (MongoDB, Express, React, Node.js).

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Node](https://img.shields.io/badge/node-%3E%3D14.0.0-brightgreen)
![React](https://img.shields.io/badge/react-18.2.0-blue)

## ✨ Key Features

### 🔐 Authentication & Security
- User registration and login with JWT authentication
- Secure password hashing with bcrypt
- Protected routes and API endpoints
- Token-based session management

### 📋 Task Management
- **Create** tasks with title, description, priority, and due dates
- **Update** task status (Pending → In Progress → Completed)
- **Delete** tasks you no longer need
- **Filter** tasks by status
- **Priority levels**: Low, Medium, High
- **Statistics dashboard** showing task breakdown

### 🎨 User Interface
- Clean, modern, and responsive design
- Intuitive task creation modal
- Real-time task updates
- Color-coded priority badges
- Status indicators for quick overview

### 🚀 Technical Highlights
- RESTful API design
- MVC architecture
- JWT authentication
- MongoDB database with Mongoose ODM
- React Context for state management
- Axios for HTTP requests

## 📸 Screenshots

### Dashboard
```
┌─────────────────────────────────────────────────────┐
│  My Tasks                              [Logout]     │
│  Welcome, Username!                                 │
├─────────────────────────────────────────────────────┤
│  ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐      │
│  │  12    │ │   5    │ │   3    │ │   4    │      │
│  │ Total  │ │Pending │ │Progress│ │Complete│      │
│  └────────┘ └────────┘ └────────┘ └────────┘      │
├─────────────────────────────────────────────────────┤
│  [+ New Task]     [All] [Pending] [Progress] [✓]   │
├─────────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────────┐   │
│  │ Complete Project Documentation      [HIGH]  │   │
│  │ Write comprehensive docs...                 │   │
│  │ Status: pending  |  Due: 2024-12-31        │   │
│  │                         [Edit] [Delete]     │   │
│  └─────────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────────┐   │
│  │ Fix Login Bug                     [MEDIUM]  │   │
│  │ Resolve authentication issue                │   │
│  │ Status: in-progress  |  Due: 2024-12-15    │   │
│  │                         [Edit] [Delete]     │   │
│  └─────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────┘
```

### Login Page
```
┌─────────────────────────────────────┐
│                                     │
│         Task Manager                │
│            Login                    │
│                                     │
│  Email:                             │
│  [____________________]             │
│                                     │
│  Password:                          │
│  [____________________]             │
│                                     │
│       [Login Button]                │
│                                     │
│  Don't have an account? Register    │
│                                     │
└─────────────────────────────────────┘
```

## 🛠️ Tech Stack

### Backend
- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MongoDB
- **ODM**: Mongoose
- **Authentication**: JWT (jsonwebtoken)
- **Security**: bcrypt for password hashing
- **Validation**: express-validator

### Frontend
- **Framework**: React 18
- **Routing**: React Router v6
- **HTTP Client**: Axios
- **State Management**: React Context API
- **Styling**: Custom CSS

### Option 2: Manual Setup

**Prerequisites**: Node.js 14+, MongoDB

```bash
# Backend setup
cd backend
npm install
cp .env.example .env  # Edit .env with your settings
npm start

# Frontend setup (in new terminal)
cd frontend
npm install
npm start
```

Visit http://localhost:3000 to use the application.

## 📚 Documentation

- **[Setup Guide](SETUP_GUIDE.md)** - Detailed installation instructions
- **[Architecture](ARCHITECTURE.md)** - System design and technical details
- **[API Collection](Task-Manager-API.postman_collection.json)** - Postman collection for API testing

## 🧪 API Endpoints

### Authentication
```
POST   /api/auth/register    Register new user
POST   /api/auth/login       Login user
GET    /api/auth/me          Get current user (protected)
```

### Tasks
```
GET    /api/tasks            Get all tasks (protected)
GET    /api/tasks/:id        Get single task (protected)
POST   /api/tasks            Create task (protected)
PUT    /api/tasks/:id        Update task (protected)
DELETE /api/tasks/:id        Delete task (protected)
GET    /api/tasks/stats      Get statistics (protected)
```

## 🎓 Learning Outcomes

This project demonstrates proficiency in:

✅ **Full-Stack Development** - Complete MERN stack implementation  
✅ **RESTful API Design** - Proper HTTP methods and status codes  
✅ **Authentication & Authorization** - JWT-based security  
✅ **Database Design** - MongoDB schema and relationships  
✅ **State Management** - React Context API  
✅ **Responsive UI** - Modern CSS and component design   
✅ **Git** - Version control and collaboration  

## 📝 Code Quality

- **Modular Architecture**: Separation of concerns (MVC pattern)
- **Error Handling**: Comprehensive try-catch blocks
- **Input Validation**: Both client and server-side validation
- **Security Best Practices**: Password hashing, JWT tokens, protected routes
- **Clean Code**: Consistent naming, proper comments, readable structure

## 🔮 Future Enhancements

- [ ] Task categories and tags
- [ ] File attachments for tasks
- [ ] Task sharing and collaboration
- [ ] Email notifications
- [ ] Search functionality
- [ ] Dark mode
- [ ] Mobile app (React Native)
- [ ] Real-time updates with WebSockets
- [ ] Task templates
- [ ] Analytics dashboard

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👤 Author

**Shimon Abramov**
- GitHub: [@shimonA1](https://github.com/shimonA1)
- LinkedIn: [Shimon Abramov](https://linkedin.com/in/shimon-abramov-444b7933b/)
- Email: shimon789456a@gmail.com

## 🙏 Acknowledgments

- Built as a portfolio project to demonstrate full-stack development skills
- Inspired by modern task management applications
- Special thanks to the open-source community

---

⭐ If you found this project helpful, please give it a star!

**Note**: This is a demonstration project built for learning purposes. Feel free to use it as a starting point for your own projects!
