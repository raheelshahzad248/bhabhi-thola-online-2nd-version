
# Bhabhi Thola Online — Real-Time Multiplayer Prototype

This is a starter real-time browser game using Node.js + Socket.IO.

## Included
- 2–6 players per room
- 6-digit room code
- Real-time player list and turns
- 52-card deck
- A > K > Q > J > 10 ... > 2
- Cards dealt automatically
- Manual THOLA button
- Highest card in the center pile takes the pile
- First player with 0 cards wins
- Mobile-friendly UI

## Run on your computer
1. Install Node.js (LTS).
2. Open a terminal in this folder.
3. Run: `npm install`
4. Run: `npm start`
5. Open `http://localhost:3000`
6. Open the same address in another browser/device on the same network for local testing.

## Make it worldwide
Deploy this Node project to a Node-compatible hosting service (for example Render, Railway, Fly.io, or a VPS). Use the generated public HTTPS URL as the game link.

## Important game-rule note
The prototype treats THOLA as a manual action. When THOLA is pressed, the highest-ranked card currently in the center pile wins the whole pile and that player gets the next turn. This can be changed once the exact traditional Bhabhi Thola timing is confirmed.

## Next production upgrades
- Persistent rooms / reconnect support
- Anti-cheat/server-authoritative validation
- Private/public matchmaking
- Invite link / WhatsApp share
- Sound and animations
- Player avatars
- Spectator mode
- Leaderboard and accounts
- HTTPS domain and production monitoring
