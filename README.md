# Job Portal Backend API

A robust Node.js/Express.js backend API for the Job Portal application. This server provides comprehensive RESTful APIs for user authentication, job management, application processing, and administrative functions with MongoDB as the database.

## Features

### Core Functionality

- **User Authentication** - JWT-based secure authentication system
- **Role-Based Access Control** - Job Seeker, Recruiter, and Admin roles
- **Job Management** - Complete CRUD operations for job postings
- **Application Processing** - Job application submission and management
- **User Profiles** - User registration, profile management, and updates
- **Admin Dashboard** - Administrative controls and platform statistics

### Security & Performance

- **Password Hashing** - Bcrypt for secure password storage
- **JWT Tokens** - Secure token-based authentication
- **Input Validation** - Express-validator for data validation
- **Error Handling** - Comprehensive error handling middleware
- **CORS Support** - Cross-origin resource sharing configuration
- **Cookie Management** - Secure cookie handling for sessions

## Tech Stack

- **Node.js** - JavaScript runtime environment
- **Express.js 4.21.2** - Fast, minimalist web framework
- **MongoDB** - NoSQL database for flexible data storage
- **Mongoose 8.18.2** - MongoDB object modeling for Node.js
- **JWT (jsonwebtoken) 9.0.2** - JSON Web Token implementation
- **bcrypt 5.1.1** - Password hashing library
- **Express Validator 7.0.1** - Middleware for input validation
- **CORS 2.8.5** - Cross-Origin Resource Sharing middleware
- **Cookie Parser 1.4.6** - Parse HTTP request cookies
- **Day.js 1.11.10** - Date manipulation library
- **Dotenv 16.3.1** - Environment variable loader
- **Nanoid 3** - Unique ID generator

## Project Structure

```
full-stack-job-portal-server-main/
├── Controller/                    # Request handlers
│   ├── AdminController.js        # Admin-specific operations
│   ├── ApplicationController.js  # Job application handling
│   ├── JobController.js          # Job-related operations
│   └── UserController.js         # User management operations
├── Middleware/                    # Custom middleware
│   ├── UserAuthenticationMiddleware.js  # JWT authentication
│   └── UserAuthorizationMiddleware.js   # Role-based authorization
├── Model/                         # Database models
│   ├── ApplicationModel.js       # Job application schema
│   ├── JobModel.js              # Job posting schema
│   └── UserModel.js             # User profile schema
├── Router/                        # API routes
│   ├── AdminRouter.js           # Admin-specific routes
│   ├── ApplicationRouter.js     # Application routes
│   ├── AuthRouter.js            # Authentication routes
│   ├── JobRouter.js             # Job-related routes
│   └── UserRouter.js            # User management routes
├── Utils/                         # Utility functions
│   ├── ApplicationConstants.js  # Application-related constants
│   ├── DBconnect.js            # Database connection handler
│   ├── JobConstants.js         # Job-related constants
│   └── JWTGenerator.js         # JWT token utilities
├── Validation/                    # Data validation
│   ├── ApplicationDataRules.js  # Application validation rules
│   ├── JobDataRules.js         # Job data validation rules
│   ├── UserDataRules.js        # User data validation rules
│   └── ValidationMiddleware.js  # Validation middleware
├── .env                          # Environment variables
├── App.js                        # Express app configuration
├── Server.js                     # Server startup file
├── package.json                  # Dependencies and scripts
└── vercel.json                   # Deployment configuration
```

## Installation & Setup

### Prerequisites

- Node.js (v14 or higher)
- npm or yarn
- MongoDB database (local or cloud)

### Setup Instructions

1. **Navigate to the backend directory:**

   ```bash
   cd full-stack-job-portal-server-main
   ```

2. **Install dependencies:**

   ```bash
   npm install
   ```

3. **Environment Configuration:**
   Create a `.env` file in the root directory:

   ```env
   # Database Configuration
   MONGODB_URI=mongodb://localhost:27017/job-portal
   # or for MongoDB Atlas:
   # MONGODB_URI=mongodb+srv://<username>:<password>@cluster.mongodb.net/job-portal

   # JWT Configuration
   JWT_SECRET=your_super_secret_jwt_key_here
   JWT_LIFETIME=1d

   # Server Configuration
   PORT=80
   NODE_ENV=development

   # CORS Configuration (optional)
   FRONTEND_URL=http://localhost:5173
   ```

4. **Start the server:**

   ```bash
   # Development mode with auto-reload
   npm run dev

   # Production mode
   npm start
   ```

5. **Verify server is running:**
   Visit `http://localhost:80` - you should see "Job Hunter Server is running!"

## API Endpoints

### Authentication Routes (`/auth`)

| Method | Endpoint         | Description       | Access        |
| ------ | ---------------- | ----------------- | ------------- |
| POST   | `/auth/register` | Register new user | Public        |
| POST   | `/auth/login`    | User login        | Public        |
| POST   | `/auth/logout`   | User logout       | Authenticated |

**Example - User Registration:**

```json
POST /auth/register
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "securePassword123",
  "role": "job-seeker"
}
```

### Job Routes (`/jobs`)

| Method | Endpoint    | Description                            | Access          |
| ------ | ----------- | -------------------------------------- | --------------- |
| GET    | `/jobs`     | Get all jobs with pagination/filtering | Authenticated   |
| GET    | `/jobs/:id` | Get specific job details               | Authenticated   |
| POST   | `/jobs`     | Create new job posting                 | Recruiter only  |
| PUT    | `/jobs/:id` | Update existing job                    | Recruiter/Admin |
| DELETE | `/jobs/:id` | Delete job posting                     | Recruiter/Admin |

**Example - Create Job:**

```json
POST /jobs
{
  "title": "Senior Frontend Developer",
  "company": "Tech Corp",
  "location": "New York, NY",
  "salary": 85000,
  "description": "We are looking for a senior frontend developer...",
  "requirements": ["React", "TypeScript", "5+ years experience"],
  "type": "full-time"
}
```

### Application Routes (`/applications`)

| Method | Endpoint                   | Description                       | Access          |
| ------ | -------------------------- | --------------------------------- | --------------- |
| GET    | `/applications`            | Get user's applications           | Authenticated   |
| GET    | `/applications/job/:jobId` | Get applications for specific job | Recruiter/Admin |
| POST   | `/applications/:jobId`     | Apply for a job                   | Job Seeker      |
| PUT    | `/applications/:id/status` | Update application status         | Recruiter/Admin |
| DELETE | `/applications/:id`        | Withdraw application              | Job Seeker      |

### User Routes (`/users`)

| Method | Endpoint         | Description              | Access        |
| ------ | ---------------- | ------------------------ | ------------- |
| GET    | `/users/profile` | Get current user profile | Authenticated |
| PUT    | `/users/profile` | Update user profile      | Authenticated |
| GET    | `/users/stats`   | Get user statistics      | Authenticated |

### Admin Routes (`/admin`)

| Method | Endpoint                | Description             | Access     |
| ------ | ----------------------- | ----------------------- | ---------- |
| GET    | `/admin/users`          | Get all users           | Admin only |
| GET    | `/admin/stats`          | Get platform statistics | Admin only |
| PUT    | `/admin/users/:id/role` | Update user role        | Admin only |
| DELETE | `/admin/users/:id`      | Delete user account     | Admin only |

## Authentication & Authorization

### JWT Authentication Flow

1. **User Registration/Login** - Server generates JWT token
2. **Token Storage** - Token sent via HTTP-only cookies
3. **Protected Routes** - Middleware verifies JWT on each request
4. **Role Validation** - Additional middleware checks user permissions

### Middleware Stack

```javascript
// Authentication middleware
const authenticateUser = async (req, res, next) => {
  // Extract token from cookies or headers
  // Verify JWT token
  // Attach user to request object
};

// Authorization middleware
const authorizeRoles = (...roles) => {
  return (req, res, next) => {
    // Check if user role is authorized
  };
};
```

## Data Validation

### Input Validation Rules

**User Registration:**

- Name: Required, 2-50 characters
- Email: Valid email format, unique
- Password: Minimum 6 characters, must contain numbers and letters

**Job Posting:**

- Title: Required, 3-100 characters
- Company: Required, 2-100 characters
- Salary: Optional, positive number
- Description: Required, minimum 50 characters

### Validation Implementation

```javascript
const { body, validationResult } = require("express-validator");

const validateJobData = [
  body("title").trim().isLength({ min: 3, max: 100 }),
  body("description").isLength({ min: 50 }),
  body("salary").optional().isNumeric().isInt({ min: 0 }),
];
```

## Deployment

### Environment Setup

```bash
# Production environment variables
NODE_ENV=production
MONGODB_URI=mongodb+srv://...
JWT_SECRET=your_production_secret
PORT=80
```

### Vercel Deployment

1. **Configure vercel.json:**

   ```json
   {
     "version": 2,
     "builds": [
       {
         "src": "Server.js",
         "use": "@vercel/node"
       }
     ],
     "routes": [
       {
         "src": "/(.*)",
         "dest": "Server.js"
       }
     ]
   }
   ```

## Troubleshooting

### Common Issues

1. **Database Connection Errors:**

   - Check MongoDB URI in `.env`
   - Ensure MongoDB service is running
   - Verify network connectivity

2. **JWT Token Issues:**

   - Check JWT_SECRET in environment variables
   - Verify token expiration settings
   - Clear cookies/localStorage in frontend

3. **CORS Errors:**

   - Verify FRONTEND_URL in environment
   - Check CORS middleware configuration
   - Ensure credentials are enabled

4. **Validation Errors:**
   - Check request body format
   - Verify all required fields are provided
   - Review validation rules in middleware

