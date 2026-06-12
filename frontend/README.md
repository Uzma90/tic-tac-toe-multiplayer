# Frontend Setup Guide

## Prerequisites

- Node.js 18+ installed
- npm or yarn package manager
- Backend server running on http://localhost:5000

## Installation

1. **Navigate to frontend directory:**
```bash
cd frontend
```

2. **Install dependencies:**
```bash
npm install
```

## Running the Application

**Development mode:**
```bash
npm start
```

The app will open at `http://localhost:3000`

**Production build:**
```bash
npm run build
```

## Project Structure

```
frontend/
├── src/
│   ├── App.js                # Main app component & routing
│   ├── App.css               # Global styles
│   ├── index.js              # React entry point
│   ├── pages/
│   │   ├── Login.js          # Login page
│   │   ├── Login.css
│   │   ├── Register.js       # Registration page
│   │   ├── Register.css
│   │   ├── Lobby.js          # Main lobby page
│   │   ├── Lobby.css
│   │   ├── Game.js           # Game page
│   │   ├── Game.css
│   │   ├── Profile.js        # Player profile
│   │   ├── Profile.css
│   ├── components/
│   │   ├── GameBoard.js      # 3x3 game board
│   │   ├── GameBoard.css
│   │   ├── Timer.js          # 30-second timer
│   │   ├── Timer.css
│   │   ├── Chat.js           # In-game chat
│   │   ├── Chat.css
│   │   ├── House.js          # House visualization
│   │   └── House.css
│   └── public/
│       └── index.html        # HTML template
├── package.json
└── Dockerfile
```

## Components Overview

### Pages

**Login.js**
- User login form
- Email and password fields
- Link to registration
- Token stored in localStorage

**Register.js**
- User registration form
- Username, email, password fields
- Password confirmation validation
- Automatic login after registration

**Lobby.js**
- Main dashboard after login
- Display player stats (wins, losses, draws, house level)
- "Play Now" button to find opponent
- "View Profile" button
- Logout functionality

**Game.js**
- Real-time game display
- GameBoard component
- Timer component
- Chat component
- Turn indicator
- Winner display

**Profile.js**
- Player profile display
- House visualization (progresses with wins)
- Player statistics
- Game history
- View other players' profiles

### Components

**GameBoard.js**
- 3x3 grid display
- Click to make moves
- Shows X and O symbols
- Disabled during opponent's turn
- Prevents moves on filled cells

**Timer.js**
- 30-second countdown
- Color changes: Green → Yellow → Red
- Pulse animation when time running out

**Chat.js**
- Message display area
- Input field for typing
- Send button
- Enter key to send

**House.js**
- Progressive house visualization
- 7+ construction stages
- Updates with each win
- Shows current level

## Features Implemented

✅ User Authentication (Register/Login)
✅ JWT Token Management
✅ Real-time Opponent Matching
✅ Live Game Board Updates
✅ 30-Second Turn Timer
✅ In-Game Chat
✅ Player Profiles
✅ House Building System
✅ Game Statistics
✅ Leaderboard
✅ Game History
✅ Responsive Design
✅ Protected Routes

## API Integration

All API calls made through Axios:

**Authentication:**
```javascript
POST /api/auth/register
POST /api/auth/login
```

**User Data:**
```javascript
GET /api/users/profile/:userId
GET /api/users/history/:userId
PUT /api/users/profile
GET /api/users/leaderboard
```

**Game Data:**
```javascript
GET /api/games/:gameId
```

## WebSocket Integration

Connected via Socket.io:

**Sending Events:**
```javascript
socket.emit('join-lobby', userId)
socket.emit('make-move', {gameId, position, playerId})
socket.emit('send-message', {gameId, playerId, message})
```

**Receiving Events:**
```javascript
socket.on('game-started', handler)
socket.on('board-updated', handler)
socket.on('message', handler)
socket.on('game-over', handler)
socket.on('opponent-disconnected', handler)
```

## State Management

- localStorage for authentication token and user data
- React hooks (useState, useEffect) for component state
- Socket.io for real-time state synchronization

## Styling

- CSS3 for responsive design
- Mobile-first approach
- Gradient backgrounds
- Smooth transitions and animations
- Flexbox and Grid layouts

## Responsive Breakpoints

- Desktop (1024px+): Full layout with sidebar chat
- Tablet (768px-1023px): Adjusted spacing
- Mobile (<768px): Stacked layout

## Performance Optimizations

- Lazy loading components
- Efficient re-renders
- Debounced socket events
- Image optimization
- CSS minification in production

## Environment Variables

Create `.env.local` file:
```
REACT_APP_API_URL=http://localhost:5000
```

## Development Tips

- Use React DevTools browser extension
- Check browser console for errors
- Verify Socket.io connection in DevTools
- Test with multiple browser windows for multiplayer

## Troubleshooting

**Can't connect to backend:**
- Ensure backend is running on port 5000
- Check CORS settings in backend
- Verify API_URL matches backend address

**Socket.io not connecting:**
- Check browser console for errors
- Ensure socket connection URL is correct
- Verify backend Socket.io is configured

**Game board not updating:**
- Check socket events in DevTools
- Verify game state updates
- Check console for errors

**Chat not working:**
- Verify socket connection
- Check message emit/receive events
- Ensure gameId is passed correctly

## Production Build

```bash
npm run build
```

Creates optimized production build in `build/` directory.

Deploy to:
- Vercel
- Netlify
- AWS S3 + CloudFront
- GitHub Pages
- Any static hosting service

## Testing

Run tests with:
```bash
npm test
```

## Browser Support

- Chrome/Edge (latest)
- Firefox (latest)
- Safari (latest)
- Mobile browsers
