
const path = require("path");
const http = require("http");
const express = require("express");
const { Server } = require("socket.io");

const app = express();
const server = http.createServer(app);
const io = new Server(server);
const PORT = process.env.PORT || 3000;

app.use(express.static(path.join(__dirname, "public")));

const rooms = new Map();

const ranks = [
  {name:"2", value:2},{name:"3", value:3},{name:"4", value:4},{name:"5", value:5},
  {name:"6", value:6},{name:"7", value:7},{name:"8", value:8},{name:"9", value:9},
  {name:"10", value:10},{name:"J", value:11},{name:"Q", value:12},{name:"K", value:13},
  {name:"A", value:14}
];
const suits = [
  {name:"♠", color:"black"}, {name:"♥", color:"red"},
  {name:"♦", color:"red"}, {name:"♣", color:"black"}
];

function makeDeck() {
  const deck = [];
  for (const r of ranks) for (const s of suits) {
    deck.push({ id: `${r.name}${s.name}`, rank:r.name, value:r.value, suit:s.name, color:s.color });
  }
  return deck;
}
function shuffle(a) {
  for (let i=a.length-1;i>0;i--) {
    const j=Math.floor(Math.random()*(i+1));
    [a[i],a[j]]=[a[j],a[i]];
  }
  return a;
}
function code() {
  let c;
  do c = String(Math.floor(100000 + Math.random()*900000)); while (rooms.has(c));
  return c;
}
function publicRoom(room) {
  return {
    code: room.code,
    started: room.started,
    hostId: room.hostId,
    turn: room.turn,
    pile: room.pile.map(x=>({rank:x.rank,suit:x.suit,color:x.color,value:x.value})),
    winnerId: room.winnerId,
    players: room.players.map(p => ({
      id:p.id, name:p.name, cards:p.cards.length,
      ready:p.ready, host:p.id===room.hostId
    }))
  };
}
function emitRoom(room) {
  io.to(room.code).emit("room:update", publicRoom(room));
}
function emitPrivateHands(room) {
  for (const p of room.players) {
    io.to(p.id).emit("hand:update", p.cards);
  }
}
function deal(room) {
  const deck = shuffle(makeDeck());
  room.players.forEach(p=>p.cards=[]);
  deck.forEach((card,i)=> room.players[i % room.players.length].cards.push(card));
  room.players.forEach(p => p.cards.sort((a,b)=>a.value-b.value));
  room.pile = [];
  room.winnerId = null;
  room.started = true;
  room.turn = 0;
}
function currentPlayer(room) { return room.players[room.turn]; }
function broadcastState(room) {
  emitRoom(room);
  emitPrivateHands(room);
}

io.on("connection", socket => {
  socket.on("room:create", ({name}) => {
    name = String(name || "Player").trim().slice(0,20);
    const c = code();
    const room = {
      code:c, hostId:socket.id, started:false, turn:0,
      pile:[], winnerId:null, players:[{id:socket.id,name,cards:[],ready:true}]
    };
    rooms.set(c, room);
    socket.join(c);
    socket.data.room=c;
    socket.emit("room:created", {code:c});
    broadcastState(room);
  });

  socket.on("room:join", ({code,name}) => {
    code=String(code||"").trim();
    name=String(name||"Player").trim().slice(0,20);
    const room=rooms.get(code);
    if(!room) return socket.emit("error:message","Room not found.");
    if(room.started) return socket.emit("error:message","Game already started.");
    if(room.players.length>=6) return socket.emit("error:message","Room is full.");
    room.players.push({id:socket.id,name,cards:[],ready:true});
    socket.join(code); socket.data.room=code;
    socket.emit("room:joined",{code});
    broadcastState(room);
  });

  socket.on("player:ready", () => {
    const room=rooms.get(socket.data.room); if(!room) return;
    const p=room.players.find(x=>x.id===socket.id); if(!p) return;
    p.ready=!p.ready; emitRoom(room);
  });

  socket.on("game:start", () => {
    const room=rooms.get(socket.data.room); if(!room) return;
    if(room.hostId!==socket.id) return;
    if(room.players.length<2) return socket.emit("error:message","At least 2 players are required.");
    if(!room.players.every(p=>p.ready)) return socket.emit("error:message","All players must be ready.");
    deal(room);
    broadcastState(room);
  });

  socket.on("card:play", ({cardId}) => {
    const room=rooms.get(socket.data.room); if(!room || !room.started || room.winnerId) return;
    const p=currentPlayer(room);
    if(!p || p.id!==socket.id) return socket.emit("error:message","Not your turn.");
    const idx=p.cards.findIndex(c=>c.id===cardId);
    if(idx<0) return;
    const card=p.cards.splice(idx,1)[0];
    room.pile.push({playerId:p.id, playerName:p.name, ...card});
    if(p.cards.length===0) {
      room.winnerId=p.id;
      broadcastState(room);
      io.to(room.code).emit("game:winner",{name:p.name});
      return;
    }
    room.turn=(room.turn+1)%room.players.length;
    broadcastState(room);
  });

  socket.on("thola", () => {
    const room=rooms.get(socket.data.room); if(!room || !room.started || room.winnerId) return;
    if(room.pile.length===0) return socket.emit("error:message","There are no cards in the center.");
    // Prototype rule: highest rank in the pile wins all cards.
    const highest=room.pile.reduce((a,b)=>b.value>a.value?b:a);
    const winner=room.players.find(p=>p.id===highest.playerId);
    if(!winner) return;
    const taken=room.pile.map(c=>({id:c.id,rank:c.rank,value:c.value,suit:c.suit,color:c.color}));
    winner.cards.push(...taken);
    winner.cards.sort((a,b)=>a.value-b.value);
    room.pile=[];
    room.turn=room.players.findIndex(p=>p.id===winner.id);
    broadcastState(room);
    io.to(room.code).emit("game:thola",{name:winner.name});
  });

  socket.on("disconnect", () => {
    const room=rooms.get(socket.data.room); if(!room) return;
    room.players=room.players.filter(p=>p.id!==socket.id);
    if(room.players.length===0) return rooms.delete(room.code);
    if(room.hostId===socket.id) room.hostId=room.players[0].id;
    if(room.turn>=room.players.length) room.turn=0;
    if(room.started && room.players.length<2) {
      room.started=false; room.pile=[]; room.players.forEach(p=>p.cards=[]);
    }
    broadcastState(room);
  });
});

server.listen(PORT, "0.0.0.0", ()=>console.log(`Bhabhi Thola running on port ${PORT}`));
