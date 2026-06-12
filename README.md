# Multiplayer Tic Tac Toe Game

A real-time multiplayer Tic Tac Toe game with player profiles, house building system, and live chat functionality.

## ✨ Features

### Gameplay
- **Real-Time Multiplayer**: Play online with other players in real-time
- **30-Second Turn Limit**: Each player has 30 seconds to make their move (timer turns red when time is running out)
- **Instant Opponent Matching**: Automatic matchmaking system finds opponents quickly
- **Real-Time Board Sync**: Game state synchronized across both players instantly

### Player Profiles & House Building System
- **Personalized Profiles**: View player statistics and achievements
- **House Building Progression**: Each win adds a construction stage to your house
  - Level 1: Foundation + Walls
  - Level 2: Add First Window
  - Level 3: Add Door
  - Level 4: Add Second Window
  - Level 5: Add Roof
  - Level 6: Add Chimney
  - Level 7: Add Fence
- **Visual House Representation**: Each player's profile displays their house level as a visual indicator of their wins
- **Player Statistics**: Track wins, losses, draws, and win rate

### Chat System
- **In-Game Chat**: Chat with your opponent during matches
- **Real-Time Messaging**: Messages delivered instantly via WebSocket
- **Chat History**: View entire conversation during a game

### Additional Features
- **User Authentication**: JWT-based secure login/registration
- **Leaderboard**: Global leaderboard showing top players by wins
- **Game History**: View past games and opponents
- **Responsive Design**: Works on desktop and mobile devices
- **Online Status**: See if players are currently online

## 🛠️ Technology Stack

- **Backend**: 
  - Node.js + Express.js
  - Socket.io for real-time communication
  - MongoDB for data persistence
  - JWT for authentication
  - bcryptjs for password hashing

- **Frontend**:
  - React 18
  - React Router for navigation
  - Socket.io-client for WebSocket communication
  - Axios for HTTP requests
  - CSS3 for styling

- **Deployment**:
  - Docker & Docker Compose
  - MongoDB (Docker image)

## 📁 Project Structure

```
.
├── backend/
│   ├── src/
│   │   ├── index.js              # Server entry point
│   │   ├── models/
│   │   │   ├── User.js           # User data model
│   │   │   └── Game.js           # Game data model
│   │   ├── routes/
│   │   │   ├── auth.js           # Authentication endpoints
│   │   │   ├── users.js          # User profile endpoints
│   │   │   └── games.js          # Game endpoints
│   │   └── managers/
│   │       └── GameManager.js    # Real-time game logic
│   ├── package.json
│   ├── Dockerfile
│   └── .env.example
├── frontend/
│   ├── src/
│   │   ├── App.js                # Main app component
│   │   ├── pages/
│   │   │   ├── Login.js          # Login page
│   │   │   ├── Register.js       # Registration page
│   │   │   ├── Lobby.js          # Main lobby
│   │   │   ├── Game.js           # Game page
│   │   │   └── Profile.js        # Player profile
│   │   ├── components/
│   │   │   ├── GameBoard.js      # Game board display
│   │   │   ├── Timer.js          # Turn timer
│   │   │   ├── Chat.js           # Chat widget
│   │   │   └── House.js          # House visualization
│   │   └── index.js              # React entry point
│   ├── package.json
│   ├── Dockerfile
│   └── public/
├── docker-compose.yml            # Docker Compose configuration
└── README.md
```

## 🚀 Quick Start

### Prerequisites
- Docker & Docker Compose
- Or: Node.js 18+, MongoDB

### Using Docker (Recommended)

```bash
# Clone the repository
git clone https://github.com/Uzma90/tic-tac-toe-multiplayer.git
cd tic-tac-toe-multiplayer

# Start all services
docker-compose up --build

# Access the application
# Frontend: http://localhost:3000
# Backend API: http://localhost:5000
```

### Manual Setup

**Backend:**
```bash
cd backend
npm install
# Create .env file (copy from .env.example)
npm run dev  # Starts with nodemon for development
```

**Frontend:**
```bash
cd frontend
npm install
npm start
```

**MongoDB:**
```bash
# Make sure MongoDB is running on localhost:27017
```

## 📋 Game Rules

1. Standard Tic Tac Toe rules apply (3 in a row to win)
2. Player 1 is always 'X', Player 2 is always 'O'
3. Each player has 30 seconds per turn
4. Running out of time results in automatic loss
5. Game ends when someone wins or the board is full (draw)

## 🎮 How to Play

1. **Register/Login**: Create an account or sign in
2. **Lobby**: Click "Play Now" to search for an opponent
3. **Game**: Make moves by clicking board cells, use chat to communicate
4. **Profile**: View your house level and game statistics

## 🔐 Authentication

- Secure JWT-based authentication
- Passwords hashed with bcryptjs
- Protected routes require valid token

## 🏠 House Building System

The house building system provides visual progression:

| Level | Construction |
|-------|--------------|
| 0 | Foundation only |
| 1 | Foundation + Walls |
| 2 | + First Window |
| 3 | + Door |
| 4 | + Second Window |
| 5 | + Roof |
| 6 | + Chimney |
| 7+ | + Fence & More |

Every win adds one level to your house!

## 🎯 API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user

### Users
- `GET /api/users/profile/:userId` - Get user profile
- `GET /api/users/history/:userId` - Get game history
- `PUT /api/users/profile` - Update profile
- `GET /api/users/leaderboard` - Get global leaderboard

### Games
- `GET /api/games/:gameId` - Get game details

## 🔌 WebSocket Events

### Client → Server
- `join-lobby` - Join the matchmaking lobby
- `make-move` - Make a game move
- `send-message` - Send chat message

### Server → Client
- `game-started` - Game has started
- `board-updated` - Board state changed
- `message` - New chat message
- `game-over` - Game ended
- `opponent-disconnected` - Opponent left

## 📱 Responsive Design

The application is fully responsive:
- Desktop (1024px+): Full layout with chat sidebar
- Tablet (768px-1023px): Adjusted layout
- Mobile (< 768px): Stacked layout

## 🚨 Error Handling

- Invalid moves are rejected
- Disconnected players forfeit the game
- Server validates all moves
- Input validation on frontend and backend

## 🔄 Real-Time Updates

- Socket.io enables real-time communication
- All events are pushed instantly
- No polling required
- Efficient binary protocol for performance

## 📊 Performance

- Optimized database queries
- Efficient WebSocket messaging
- Debounced updates where appropriate
- Lazy loading of components

## 🛡️ Security

- JWT token-based authentication
- CORS protection
- Input validation and sanitization
- Secure password hashing
- Protected API routes

## 📝 Future Enhancements

- [ ] Seasonal rankings
- [ ] Daily challenges
- [ ] Tournament mode
- [ ] Friend system
- [ ] Custom house themes
- [ ] Spectator mode
- [ ] Mobile app
- [ ] Sound effects and animations
- [ ] AI opponent
- [ ] Replay system

## 👨‍💻 Development

### Running Tests
```bash
cd backend
npm test
```

### Building for Production
```bash
docker-compose -f docker-compose.yml up --build -d
```

## 📄 License

MIT License - feel free to use this project for personal or commercial purposes.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📧 Support

For issues or questions, please open an issue on GitHub.

---

**Enjoy playing Tic Tac Toe with friends! 🎮**
