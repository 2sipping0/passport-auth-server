# Passport Authentication Server

A complete authentication server using Express.js and Passport.js with local and Google OAuth strategies.

## Features

- 🔐 Local authentication (email/password)
- 🔑 Google OAuth integration
- 👤 User registration and login
- 🗄️ MongoDB integration
- 📝 Session management
- 🛡️ Protected routes
- 👮 Role-based access control

## Prerequisites

- Node.js v14+ and npm
- MongoDB (running locally or cloud instance)

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/2sipping0/passport-auth-server.git
   cd passport-auth-server
   ```

2. Install dependencies:
   ```bash
   npm install
   ```
## MongoDB Setup

This application requires MongoDB Atlas. Local MongoDB is not supported.

1. Create a MongoDB Atlas account at [https://www.mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas)
2. Set up a free cluster
3. Create a database user
4. Add your IP address to the IP Access List
5. Get your connection string by clicking "Connect" > "Connect your application"
6. Add your connection string to the `.env` file:
 MONGODB_URI=mongodb+srv://username@cluster0.xxxxx.mongodb.net/auth-server?retryWrites=true&w=majority

Replace `username`, `password`, and the cluster information with your actual credentials.  

3. Create `.env` file in the root directory:
   ```
   NODE_ENV=development
   PORT=5000
   MONGODB_URI=mongodb+srv://<username>:<password>@<cluster>.mongodb.net/<database>?retryWrites=true&w=majority
   SESSION_SECRET=your_super_secret_key_change_this
   FRONTEND_URL=http://localhost:3000
   GOOGLE_CLIENT_ID=your_google_client_id
   GOOGLE_CLIENT_SECRET=your_google_client_secret
   ```

## Google OAuth Setup (Optional)

1. Go to the [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project
3. Navigate to APIs & Services > Credentials
4. Create OAuth client ID (Web application)
5. Add authorized redirect URI: `http://localhost:5000/api/auth/google/callback`
6. Copy your Client ID and Client Secret to your `.env` file

## Usage

### Development mode

```bash
npm run dev
```

### Production mode

```bash
npm start
```

## API Endpoints

### Authentication

| Method | Endpoint | Description | Request Body |
|--------|----------|-------------|-------------|
| POST | `/api/auth/register` | Register a new user | `{ "name": "User Name", "email": "user@example.com", "password": "password123" }` |
| POST | `/api/auth/login` | Login with credentials | `{ "email": "user@example.com", "password": "password123" }` |
| GET | `/api/auth/logout` | Logout current user | - |
| GET | `/api/auth/current-user` | Get current user info | - |
| GET | `/api/auth/google` | Initiate Google OAuth | - |
| GET | `/api/auth/google/callback` | Google OAuth callback | - |

### Protected Resources

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|--------------|
| GET | `/api/profile` | Get user profile | Yes |

## Project Structure

```
passport-auth-server/
├── config/
│   └── passport.js      # Passport.js configuration
├── models/
│   └── User.js          # User model schema
├── routes/
│   └── auth.js          # Authentication routes
├── .env                 # Environment variables
├── server.js            # Main application file
└── package.json         # Project dependencies
```

## Testing

You can test the API using tools like Postman, Insomnia, or curl.

**Example with curl:**

```bash
# Register
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name":"Test User","email":"test@example.com","password":"password123"}'

# Login
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password123"}' \
  --cookie-jar cookies.txt

# Access protected route
curl http://localhost:5000/api/profile \
  --cookie cookies.txt
```

## Frontend Integration

To integrate with frontend applications:

1. Set `FRONTEND_URL` in your `.env` file
2. Make API calls from your frontend with credentials:

```javascript
// Example login request with fetch API
const login = async (email, password) => {
  const response = await fetch('http://localhost:5000/api/auth/login', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ email, password }),
    credentials: 'include'  // Important for cookies
  });
  
  return response.json();
};
```

## Security Considerations

- In production, always use HTTPS
- Update session configuration for production
- Consider implementing rate limiting
- Add email verification for new users

## License

MIT# passport-auth-server
