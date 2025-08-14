package.json

{
  "name": "vedic-jyotish-backend",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "mongoose": "^7.3.1",
    "cors": "^2.8.5",
    "dotenv": "^16.3.1"
  },
  "devDependencies": {
    "nodemon": "^3.0.1"
  }
}
package.json

{
  "name": "vedic-jyotish-backend",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "mongoose": "^7.3.1",
    "cors": "^2.8.5",
    "dotenv": "^16.3.1"
  },
  "devDependencies": {
    "nodemon": "^3.0.1"
  }
}
PORT=5000
MONGODB_URI=mongodb://atlas-sql-689db6de8f00a703e9bdc901-rxuk6c.a.query.mongodb.net/sample_mflix?ssl=true&authSource=admin
---

models/index.js

Paste the full models code I gave you before (all 6 models in one file).


---

server.js

require('dotenv').config();
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');
const { User, Astrologer, Booking, ChatMessage, Payment, Review } = require('./models');

const app = express();
const PORT = process.env.PORT || 5000;

app.use(cors());
app.use(express.json());

// Connect MongoDB
mongoose.connect(process.env.MONGODB_URI, { 
  useNewUrlParser: true, 
  useUnifiedTopology: true 
})
.then(() => console.log("✅ MongoDB connected"))
.catch(err => {
  console.error("❌ MongoDB connection error:", err);
  process.exit(1);
});

// === CRUD ROUTES ===
// For brevity, I’ll show User routes fully, then other routes you can add similarly

// --- User Routes ---
app.get('/users', async (req, res) => {
  const users = await User.find();
  res.json(users);
});

app.get('/users/:id', async (req, res) => {
  const user = await User.findById(req.params.id);
  if (!user) return res.status(404).json({ error: "User not found" });
  res.json(user);
});

app.post('/users', async (req, res) => {
  try {
    const user = new User(req.body);
    await user.save();
    res.status(201).json(user);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});

app.put('/users/:id', async (req, res) => {
  try {
    const user = await User.findByIdAndUpdate(req.params.id, req.body, { new: true });
    if (!user) return res.status(404).json({ error: "User not found" });
    res.json(user);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});

app.delete('/users/:id', async (req, res) => {
  const user = await User.findByIdAndDelete(req.params.id);
  if (!user) return res.status(404).json({ error: "User not found" });
  res.json({ message: "User deleted" });
});

// --- Astrologer Routes ---
app.get('/astrologers', async (req, res) => {
  const astrologers = await Astrologer.find();
  res.json(astrologers);
});
app.get('/astrologers/:id', async (req, res) => {
  const astrologer = await Astrologer.findById(req.params.id);
  if (!astrologer) return res.status(404).json({ error: "Astrologer not found" });
  res.json(astrologer);
});
app.post('/astrologers', async (req, res) => {
  try {
    const astrologer = new Astrologer(req.body);
    await astrologer.save();
    res.status(201).json(astrologer);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.put('/astrologers/:id', async (req, res) => {
  try {
    const astrologer = await Astrologer.findByIdAndUpdate(req.params.id, req.body, { new: true });
    if (!astrologer) return res.status(404).json({ error: "Astrologer not found" });
    res.json(astrologer);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.delete('/astrologers/:id', async (req, res) => {
  const astrologer = await Astrologer.findByIdAndDelete(req.params.id);
  if (!astrologer) return res.status(404).json({ error: "Astrologer not found" });
  res.json({ message: "Astrologer deleted" });
});

// --- Booking Routes ---
app.get('/bookings', async (req, res) => {
  const bookings = await Booking.find();
  res.json(bookings);
});
app.get('/bookings/:id', async (req, res) => {
  const booking = await Booking.findById(req.params.id);
  if (!booking) return res.status(404).json({ error: "Booking not found" });
  res.json(booking);
});
app.post('/bookings', async (req, res) => {
  try {
    const booking = new Booking(req.body);
    await booking.save();
    res.status(201).json(booking);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.put('/bookings/:id', async (req, res) => {
  try {
    const booking = await Booking.findByIdAndUpdate(req.params.id, req.body, { new: true });
    if (!booking) return res.status(404).json({ error: "Booking not found" });
    res.json(booking);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.delete('/bookings/:id', async (req, res) => {
  const booking = await Booking.findByIdAndDelete(req.params.id);
  if (!booking) return res.status(404).json({ error: "Booking not found" });
  res.json({ message: "Booking deleted" });
});

// --- ChatMessage Routes ---
app.get('/chatmessages', async (req, res) => {
  const messages = await ChatMessage.find();
  res.json(messages);
});
app.get('/chatmessages/:id', async (req, res) => {
  const message = await ChatMessage.findById(req.params.id);
  if (!message) return res.status(404).json({ error: "Message not found" });
  res.json(message);
});
app.post('/chatmessages', async (req, res) => {
  try {
    const message = new ChatMessage(req.body);
    await message.save();
    res.status(201).json(message);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.put('/chatmessages/:id', async (req, res) => {
  try {
    const message = await ChatMessage.findByIdAndUpdate(req.params.id, req.body, { new: true });
    if (!message) return res.status(404).json({ error: "Message not found" });
    res.json(message);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.delete('/chatmessages/:id', async (req, res) => {
  const message = await ChatMessage.findByIdAndDelete(req.params.id);
  if (!message) return res.status(404).json({ error: "Message not found" });
  res.json({ message: "Message deleted" });
});

// --- Payment Routes ---
app.get('/payments', async (req, res) => {
  const payments = await Payment.find();
  res.json(payments);
});
app.get('/payments/:id', async (req, res) => {
  const payment = await Payment.findById(req.params.id);
  if (!payment) return res.status(404).json({ error: "Payment not found" });
  res.json(payment);
});
app.post('/payments', async (req, res) => {
  try {
    const payment = new Payment(req.body);
    await payment.save();
    res.status(201).json(payment);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.put('/payments/:id', async (req, res) => {
  try {
    const payment = await Payment.findByIdAndUpdate(req.params.id, req.body, { new: true });
    if (!payment) return res.status(404).json({ error: "Payment not found" });
    res.json(payment);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.delete('/payments/:id', async (req, res) => {
  const payment = await Payment.findByIdAndDelete(req.params.id);
  if (!payment) return res.status(404).json({ error: "Payment not found" });
  res.json({ message: "Payment deleted" });
});

// --- Review Routes ---
app.get('/reviews', async (req, res) => {
  const reviews = await Review.find();
  res.json(reviews);
});
app.get('/reviews/:id', async (req, res) => {
  const review = await Review.findById(req.params.id);
  if (!review) return res.status(404).json({ error: "Review not found" });
  res.json(review);
});
app.post('/reviews', async (req, res) => {
  try {
    const review = new Review(req.body);
    await review.save();
    res.status(201).json(review);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.put('/reviews/:id', async (req, res) => {
  try {
    const review = await Review.findByIdAndUpdate(req.params.id, req.body, { new: true });
    if (!review) return res.status(404).json({ error: "Review not found" });
    res.json(review);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.delete('/reviews/:id', async (req, res) => {
  const review = await Review.findByIdAndDelete(req.params.id);
  if (!review) return res.status(404).json({ error: "Review not found" });
  res.json({ message: "Review deleted" });
});



// Start server
app.listen(PORT, () => {
  console.log(`🚀 Server running on port ${PORT}`);
});PORT=5000
MONGODB_URI=mongodb://atlas-sql-689db6de8f00a703e9bdc901-rxuk6c.a.query.mongodb.net/sample_mflix?ssl=true&authSource=admin
---

models/index.js

Paste the full models code I gave you before (all 6 models in one file).


---

server.js

require('dotenv').config();
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');
const { User, Astrologer, Booking, ChatMessage, Payment, Review } = require('./models');

const app = express();
const PORT = process.env.PORT || 5000;

app.use(cors());
app.use(express.json());

// Connect MongoDB
mongoose.connect(process.env.MONGODB_URI, { 
  useNewUrlParser: true, 
  useUnifiedTopology: true 
})
.then(() => console.log("✅ MongoDB connected"))
.catch(err => {
  console.error("❌ MongoDB connection error:", err);
  process.exit(1);
});

// === CRUD ROUTES ===
// For brevity, I’ll show User routes fully, then other routes you can add similarly

// --- User Routes ---
app.get('/users', async (req, res) => {
  const users = await User.find();
  res.json(users);
});

app.get('/users/:id', async (req, res) => {
  const user = await User.findById(req.params.id);
  if (!user) return res.status(404).json({ error: "User not found" });
  res.json(user);
});

app.post('/users', async (req, res) => {
  try {
    const user = new User(req.body);
    await user.save();
    res.status(201).json(user);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});

app.put('/users/:id', async (req, res) => {
  try {
    const user = await User.findByIdAndUpdate(req.params.id, req.body, { new: true });
    if (!user) return res.status(404).json({ error: "User not found" });
    res.json(user);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});

app.delete('/users/:id', async (req, res) => {
  const user = await User.findByIdAndDelete(req.params.id);
  if (!user) return res.status(404).json({ error: "User not found" });
  res.json({ message: "User deleted" });
});

// --- Astrologer Routes ---
app.get('/astrologers', async (req, res) => {
  const astrologers = await Astrologer.find();
  res.json(astrologers);
});
app.get('/astrologers/:id', async (req, res) => {
  const astrologer = await Astrologer.findById(req.params.id);
  if (!astrologer) return res.status(404).json({ error: "Astrologer not found" });
  res.json(astrologer);
});
app.post('/astrologers', async (req, res) => {
  try {
    const astrologer = new Astrologer(req.body);
    await astrologer.save();
    res.status(201).json(astrologer);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.put('/astrologers/:id', async (req, res) => {
  try {
    const astrologer = await Astrologer.findByIdAndUpdate(req.params.id, req.body, { new: true });
    if (!astrologer) return res.status(404).json({ error: "Astrologer not found" });
    res.json(astrologer);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.delete('/astrologers/:id', async (req, res) => {
  const astrologer = await Astrologer.findByIdAndDelete(req.params.id);
  if (!astrologer) return res.status(404).json({ error: "Astrologer not found" });
  res.json({ message: "Astrologer deleted" });
});

// --- Booking Routes ---
app.get('/bookings', async (req, res) => {
  const bookings = await Booking.find();
  res.json(bookings);
});
app.get('/bookings/:id', async (req, res) => {
  const booking = await Booking.findById(req.params.id);
  if (!booking) return res.status(404).json({ error: "Booking not found" });
  res.json(booking);
});
app.post('/bookings', async (req, res) => {
  try {
    const booking = new Booking(req.body);
    await booking.save();
    res.status(201).json(booking);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.put('/bookings/:id', async (req, res) => {
  try {
    const booking = await Booking.findByIdAndUpdate(req.params.id, req.body, { new: true });
    if (!booking) return res.status(404).json({ error: "Booking not found" });
    res.json(booking);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.delete('/bookings/:id', async (req, res) => {
  const booking = await Booking.findByIdAndDelete(req.params.id);
  if (!booking) return res.status(404).json({ error: "Booking not found" });
  res.json({ message: "Booking deleted" });
});

// --- ChatMessage Routes ---
app.get('/chatmessages', async (req, res) => {
  const messages = await ChatMessage.find();
  res.json(messages);
});
app.get('/chatmessages/:id', async (req, res) => {
  const message = await ChatMessage.findById(req.params.id);
  if (!message) return res.status(404).json({ error: "Message not found" });
  res.json(message);
});
app.post('/chatmessages', async (req, res) => {
  try {
    const message = new ChatMessage(req.body);
    await message.save();
    res.status(201).json(message);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.put('/chatmessages/:id', async (req, res) => {
  try {
    const message = await ChatMessage.findByIdAndUpdate(req.params.id, req.body, { new: true });
    if (!message) return res.status(404).json({ error: "Message not found" });
    res.json(message);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.delete('/chatmessages/:id', async (req, res) => {
  const message = await ChatMessage.findByIdAndDelete(req.params.id);
  if (!message) return res.status(404).json({ error: "Message not found" });
  res.json({ message: "Message deleted" });
});

// --- Payment Routes ---
app.get('/payments', async (req, res) => {
  const payments = await Payment.find();
  res.json(payments);
});
app.get('/payments/:id', async (req, res) => {
  const payment = await Payment.findById(req.params.id);
  if (!payment) return res.status(404).json({ error: "Payment not found" });
  res.json(payment);
});
app.post('/payments', async (req, res) => {
  try {
    const payment = new Payment(req.body);
    await payment.save();
    res.status(201).json(payment);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.put('/payments/:id', async (req, res) => {
  try {
    const payment = await Payment.findByIdAndUpdate(req.params.id, req.body, { new: true });
    if (!payment) return res.status(404).json({ error: "Payment not found" });
    res.json(payment);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.delete('/payments/:id', async (req, res) => {
  const payment = await Payment.findByIdAndDelete(req.params.id);
  if (!payment) return res.status(404).json({ error: "Payment not found" });
  res.json({ message: "Payment deleted" });
});

// --- Review Routes ---
app.get('/reviews', async (req, res) => {
  const reviews = await Review.find();
  res.json(reviews);
});
app.get('/reviews/:id', async (req, res) => {
  const review = await Review.findById(req.params.id);
  if (!review) return res.status(404).json({ error: "Review not found" });
  res.json(review);
});
app.post('/reviews', async (req, res) => {
  try {
    const review = new Review(req.body);
    await review.save();
    res.status(201).json(review);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.put('/reviews/:id', async (req, res) => {
  try {
    const review = await Review.findByIdAndUpdate(req.params.id, req.body, { new: true });
    if (!review) return res.status(404).json({ error: "Review not found" });
    res.json(review);
  } catch(e) {
    res.status(400).json({ error: e.message });
  }
});
app.delete('/reviews/:id', async (req, res) => {
  const review = await Review.findByIdAndDelete(req.params.id);
  if (!review) return res.status(404).json({ error: "Review not found" });
  res.json({ message: "Review deleted" });
});



// Start server
app.listen(PORT, () => {
  console.log(`🚀 Server running on port ${PORT}`);
});
