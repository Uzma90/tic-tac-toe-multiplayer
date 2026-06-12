# Backend Setup Guide

## Prerequisites

- Node.js 18+ installed
- MongoDB running (locally or via Docker)
- npm or yarn package manager

## Installation

1. **Navigate to backend directory:**
```bash
cd backend
```

2. **Install dependencies:**
```bash
npm install
```

3. **Create environment file:**
```bash
cp .env.example .env
```

4. **Configure .env file:**
Edit `.env` and update the following variables:
```
MONGODB_URI=mongodb://localhost:27017/tictactoe
PORT=5000
CLIENT_URL=http://localhost:3000
JWT_SECRET=your_jwt_secret_key_here
NODE_ENV=development
```

## Running the Server

**Development mode (with auto-reload):**
```bash
npm run dev
```

**Production mode:**
```bash
npm start
```

The server will start on `http://localhost:5000`

## Database Models

### User Model
```javascript
{
  username: String (unique),
  email: String (unique),
  password: String (hashed),
  wins: Number,
  losses: Number,
  draws: Number,
  houseLevel: Number,
  avatar: String,
  createdAt: Date,
  lastLogin: Date,
  isOnline: Boolean
}
```

### Game Model
```javascript
{
  player1: ObjectId (User),
  player2: ObjectId (User),
  board: Array(9),
  currentTurn: ObjectId (User),
  status: String (waiting|in-progress|completed),
  winner: ObjectId (User),
  isDraw: Boolean,
  moves: Array,
  messages: Array,
  startTime: Date,
  endTime: Date,
  turnStartTime: Date
}
```

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register a new user
  - Body: `{ username, email, password }`
  - Returns: `{ token, user }`

- `POST /api/auth/login` - Login user
  - Body: `{ email, password }`
  - Returns: `{ token, user }`

### Users
- `GET /api/users/profile/:userId` - Get user profile
  - Returns: User profile with stats

- `GET /api/users/history/:userId` - Get game history
  - Requires: Auth token
  - Returns: Array of games

- `PUT /api/users/profile` - Update user profile
  - Requires: Auth token
  - Body: `{ avatar }`
  - Returns: Updated user

- `GET /api/users/leaderboard` - Get top 50 players
  - Returns: Sorted array of users

### Games
- `GET /api/games/:gameId` - Get game details
  - Returns: Game details with populated user refs

## WebSocket Events

### Emitted from Client
- `join-lobby` - Join matchmaking queue
  - Data: `userId`
  
- `make-move` - Make a game move
  - Data: `{ gameId, position, playerId, socketId }`
  
- `send-message` - Send chat message
  - Data: `{ gameId, playerId, message }`

### Sent to Client
- `game-started` - Game session started
  - Data: `{ gameId, player1, player2, isYourTurn, symbol }`
  
- `board-updated` - Board state changed
  - Data: `{ board, currentTurn, isYourTurn }`
  
- `message` - Chat message received
  - Data: `{ playerId, message, timestamp }`
  
- `game-over` - Game completed
  - Data: `{ winner, isDraw }`
  
- `opponent-disconnected` - Opponent disconnected
  
- `waiting-for-opponent` - Waiting for player to join

## Project Structure

```
backend/
├── src/
│   ├── index.js              # Server entry point & Socket.io setup
│   ├── models/
│   │   ├── User.js           # User schema & methods
│   │   └── Game.js           # Game schema
│   ├── routes/
│   │   ├── auth.js           # Authentication endpoints
│   │   ├── users.js          # User profile & stats endpoints
│   │   └── games.js          # Game endpoints
│   └── managers/
│       └── GameManager.js    # Real-time game logic & state
├── package.json
├── Dockerfile
└── .env.example
```

## Game Logic

The `GameManager` class handles:
- Player lobby management
- Automatic opponent matching
- Game state synchronization
- Win/draw detection
- Turn management (30-second limit)
- User statistics updates
- Message routing
- Player disconnection handling

## Authentication Flow

1. User registers with email/password
2. Password hashed with bcryptjs
3. User document created in MongoDB
4. JWT token generated and returned
5. Client stores token in localStorage
6. Token sent in Authorization header for protected routes

## Security Features

✅ Password hashing with bcryptjs
✅ JWT token-based authentication
✅ CORS protection
✅ Input validation with express-validator
✅ Protected API routes
✅ Secure WebSocket connections
✅ XSS protection
✅ CSRF-ready architecture

## Development Tips

- Use `nodemon` for auto-reload during development
- Check MongoDB connection in console logs
- Socket.io debug logs: `DEBUG=socket.io:* npm run dev`
- Use Postman to test REST endpoints
- Monitor WebSocket events in browser DevTools

## Troubleshooting

**MongoDB connection error:**
```
MongooseError: Cannot connect to MongoDB
```
Solution: Ensure MongoDB is running and MONGODB_URI is correct

**Socket.io connection error:**
```
WebSocket connection failed
```
Solution: Verify CLIENT_URL matches frontend address, check CORS settings

**Port already in use:**
```
Error: EADDRINUSE: address already in use :::5000
```
Solution: Change PORT in .env or kill process on port 5000

**JWT errors:**
```
JsonWebTokenError: invalid signature
```
Solution: Ensure JWT_SECRET is consistent

## Performance Considerations

- Connection pooling with MongoDB
- Efficient game state updates
- Message broadcasting optimization
- Database query indexing
- Socket.io room management

## Scaling Considerations

- Use Redis for session management
- Implement message queuing (RabbitMQ/Kafka)
- Database replication
- Horizontal scaling with load balancer
- CDN for static assets

## Testing

Run tests with:
```bash
npm test
```

## Production Deployment

1. Set `NODE_ENV=production` in .env
2. Generate strong JWT_SECRET
3. Use production MongoDB URI
4. Configure production CLIENT_URL
5. Enable HTTPS/WSS
6. Set up proper logging
7. Configure monitoring & alerts

## Environment Variables Reference

| Variable | Description | Example |
|----------|-------------|---------|
| MONGODB_URI | MongoDB connection string | mongodb://localhost:27017/tictactoe |
| PORT | Server port | 5000 |
| CLIENT_URL | Frontend URL for CORS | http://localhost:3000 |
| JWT_SECRET | Secret key for JWT signing | your_secret_key_here |
| NODE_ENV | Environment mode | development/production |

## Monitoring & Logging

- Console logs for development
- Error handling with try-catch
- Request/response logging middleware
- WebSocket event tracking
- Database query logging

## API Response Format

**Success Response:**
```javascript
{
  data: {...},
  message: "Success message"
}
```

**Error Response:**
```javascript
{
  message: "Error message",
  errors: [...]
}
```

## Rate Limiting

Consider implementing rate limiting for:
- Authentication endpoints
- WebSocket connections
- API requests

## Caching Strategy

- Cache user profiles
- Cache leaderboard
- Cache game history
