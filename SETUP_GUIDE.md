# Task Manager - Setup Guide

## 🚀 Quick Start Guide

### Method 1: Docker (Recommended - Easiest)

If you have Docker and Docker Compose installed:

```bash
# Clone the repository
git clone https://github.com/yourusername/task-manager-fullstack.git
cd task-manager-fullstack

# Start all services
docker-compose up -d

# Access the application
# Frontend: http://localhost:3000
# Backend API: http://localhost:5000
```

That's it! The application is running with MongoDB, Backend, and Frontend.

### Method 2: Manual Installation

#### Prerequisites
- Node.js v14+ installed
- MongoDB installed and running
- npm or yarn

#### Step 1: Install Backend

```bash
cd backend
npm install
```

#### Step 2: Configure Backend

Create a `.env` file in the `backend` directory:

```
PORT=5000
MONGODB_URI=mongodb://localhost:27017/taskmanager
JWT_SECRET=your_super_secret_jwt_key_change_this_in_production
NODE_ENV=development
```

#### Step 3: Start MongoDB

Make sure MongoDB is running:
```bash
# On macOS with Homebrew
brew services start mongodb-community

# On Linux
sudo systemctl start mongod

# On Windows
# Start MongoDB from Services
```

#### Step 4: Start Backend

```bash
cd backend
npm start
```

Backend should now be running on http://localhost:5000

#### Step 5: Install Frontend

Open a new terminal:

```bash
cd frontend
npm install
```

#### Step 6: Start Frontend

```bash
cd frontend
npm start
```

Frontend should now be running on http://localhost:3000

## 📝 Testing the Application

### 1. Register a New User

- Open http://localhost:3000
- Click "Register"
- Fill in username, email, and password
- Submit the form

### 2. Create Tasks

- After logging in, click "New Task"
- Fill in task details
- Set priority and status
- Save the task

### 3. Test API Endpoints (Optional)

You can test the API directly using curl or Postman:

```bash
# Register a user
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username":"testuser","email":"test@example.com","password":"password123"}'

# Login
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password123"}'

# Create a task (replace YOUR_TOKEN with the token from login)
curl -X POST http://localhost:5000/api/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{"title":"My First Task","description":"Task description","priority":"high","status":"pending"}'

# Get all tasks
curl -X GET http://localhost:5000/api/tasks \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## 🔧 Troubleshooting

### MongoDB Connection Issues

**Error**: "MongooseServerSelectionError"
**Solution**: Make sure MongoDB is running:

```bash
# Check if MongoDB is running
mongosh

# If not running, start it
brew services start mongodb-community  # macOS
sudo systemctl start mongod           # Linux
```

### Port Already in Use

**Error**: "Port 3000/5000 is already in use"
**Solution**: 

```bash
# Find and kill the process
# macOS/Linux
lsof -ti:3000 | xargs kill -9
lsof -ti:5000 | xargs kill -9

# Or change the port in .env (backend) or package.json (frontend)
```

### CORS Issues

If you see CORS errors in the browser console:
- Make sure backend is running on port 5000
- Check that frontend proxy is configured correctly in package.json
- Restart both servers

### JWT Token Issues

If authentication isn't working:
- Check that JWT_SECRET is set in .env
- Clear browser localStorage and try logging in again
- Check browser console for error messages

## 📊 Database Management

### View Data in MongoDB

```bash
# Connect to MongoDB
mongosh

# Switch to taskmanager database
use taskmanager

# View all users
db.users.find().pretty()

# View all tasks
db.tasks.find().pretty()

# Delete all tasks (careful!)
db.tasks.deleteMany({})
```

### Reset Database

```bash
# In mongosh
use taskmanager
db.dropDatabase()
```

## 🎯 Development Tips

### Hot Reload

- Frontend: Changes auto-reload with React hot reload
- Backend: Install nodemon for auto-restart:

```bash
cd backend
npm install -D nodemon
npm run dev  # Uses nodemon instead of node
```

### Debugging

Enable detailed logs:

```javascript
// In backend/server.js, add:
app.use((req, res, next) => {
  console.log(`${req.method} ${req.path}`);
  next();
});
```

## 🚢 Deployment

### Deploy to Heroku (Backend)

```bash
# Install Heroku CLI
# Login to Heroku
heroku login

# Create app
heroku create your-task-manager-api

# Add MongoDB addon
heroku addons:create mongolab

# Set environment variables
heroku config:set JWT_SECRET=your_secret_key
heroku config:set NODE_ENV=production

# Deploy
git subtree push --prefix backend heroku main
```

### Deploy Frontend to Netlify/Vercel

1. Build the frontend:
```bash
cd frontend
npm run build
```

2. Deploy the `build` folder to Netlify or Vercel
3. Set environment variable: `REACT_APP_API_URL=https://your-api-url.herokuapp.com/api`

## 📚 Next Steps

Once you have the basic app running, you can:

1. Add more features:
   - Task categories/tags
   - File attachments
   - Task sharing between users
   - Email notifications
   - Search functionality

2. Improve the UI:
   - Add animations
   - Implement drag-and-drop for task reordering
   - Add dark mode
   - Make it fully responsive

3. Add testing:
   - Unit tests with Jest
   - Integration tests
   - E2E tests with Cypress

4. Optimize performance:
   - Add caching
   - Implement pagination
   - Add database indexes

Good luck with your project! 🎉
