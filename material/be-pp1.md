### Pair Programming Activity - Iteration 1

---

## Overview

In this activity, you will collaboratively build and improve an API server for a tours system. You'll start with existing code and reorganize it using the **MVC (Model-View-Controller) pattern**, which makes your code organized, easy to understand, and easy to maintain.

**What is Pair Programming?**
- **Driver**: The person writing the code
- **Navigator**: The person reviewing and guiding the code
- You will **switch roles after each major step** so both of you write and review code

---

## Instructions

---

## Step 0: Setup - Getting Your Project Ready

Think of this step like setting up your workspace before starting a project. You're preparing all the tools and code you need to begin.

### 0.1 Decide Who Goes First (Driver & Navigator)

Before you start, decide:
- **Who will be the Driver first?** (This person will write the code)
- **Who will be the Navigator first?** (This person will review and guide)

**Remember**: You'll switch roles after each major step! This is important because both of you need to practice coding.

---

### 0.2 Get the Starter Code


**Clone from GitHub**: This downloads the code from the internet and puts it on your computer.

```bash
# Download the starter code from GitHub
git clone https://github.com/tx00-resources-en/week3-be-pp-sample-sol week4-be-pp

# Move into the new folder
cd week4-be-pp

# Remove the old Git history (so you start fresh)
rm -rf .git
```

**What does this do?**
- `git clone` = Download the code
- `cd week4-be-pp` = Enter the folder
- `rm -rf .git` = Delete the old Git history so you have a clean slate

---

### 0.3 Install and Start Your Server

Now you need to get all the tools (dependencies) your project needs.

```bash
# Install all the tools listed in package.json
npm install

# Start the server in "development mode" (it will auto-restart when you make changes)
npm run dev
```

**What's happening?**
- `npm install` reads `package.json` and downloads all required packages (Express, Nodemon, etc.)
- `npm run dev` starts your server and watches for changes. When you edit a file, the server automatically restarts!

**You should see:**
```
Server is running on port 4000
```

✅ **Success!** Your server is now running!

---

### 0.4 Test That Everything Works

Before you continue, make sure the existing API works correctly. You'll use **Postman** (the tool for testing APIs).

**Follow these steps:**
1. Open Postman
2. Create a new request
3. Test each endpoint from last week:
   - `GET http://localhost:4000/tours` (Get all tours)
   - `GET http://localhost:4000/tours/1` (Get tour with ID 1)
   - `POST http://localhost:4000/tours` (Create a new tour)
   - etc.

**Reference**: If you forgot how, [check these instructions from last week](https://github.com/FullStack-DP/session3/blob/main/material/be-pair-prog2.md#step-by-step-guide-to-testing-endpoints)

✅ **Success!** All endpoints should work. If not, ask for help before continuing.

---

### 0.5 Save Your Work to GitHub

Now you need to save your code to GitHub (like making a backup on the cloud).

**Steps:**

```bash
# Create a new Git repository (tracking system for your code)
git init

# Add all files to be tracked
git add .

# Save a snapshot of your code (commit)
git commit -m "Initial commit"

# Add the remote location (where your code lives on GitHub)
git remote add origin <your-repo-url>

# Upload your code to GitHub
git push -u origin main
```

**What does each command do?**
- `git init` = Start a new tracking system
- `git add .` = Tell Git to track all files, except the files in `.gitignore`
- `git commit -m "..."` = Save a snapshot with a message
- `git remote add origin` = Tell Git where to upload (GitHub)
- `git push` = Upload to GitHub

**⚠️ Important:** Make sure your `.gitignore` file has `node_modules` listed. This prevents uploading huge folders to GitHub.

---

---

## Step 1: Refactoring to MVC Pattern - Organizing Your Code

**What is MVC?** 
Think of it like organizing a restaurant:
- **Model** = The kitchen (where data/recipes are stored)
- **View** = The presentation (how food is presented)
- **Controller** = The waiter (handles requests, gets data from kitchen, gives it to the customer)

Right now, the code is messy - everything is mixed together. We're going to organize it!

---

### 1.1 Understand Your Current Code Structure

Look at the project. You see files like this:

```
project/
├── app.js (the main server file)
├── tourHandlers.js (functions that handle requests)
├── tourLib.js (the data/list of tours)
├── package.json
```

**The Problem:** Everything is jumbled together. The code is hard to find and hard to maintain.

**Current flow:**
```
Request comes in → tourHandlers (waiter) → tourLib (kitchen) → Response
```

---

### 1.2 The Goal: Reorganize Into MVC Structure

After refactoring, your project should look like this:

```
project/
├── controllers/
│   └── tourControllers.js (handles requests - the waiter)
├── models/
│   └── tourModel.js (stores/manages data - the kitchen)
├── routes/
│   └── tourRouter.js (defines which URLs map to which handlers)
├── app.js (main server setup)
└── package.json
```

**New flow:**
```
Request comes in → tourRouter (organizer) → tourControllers (waiter) → tourModel (kitchen) → Response
```

---

### 1.3 Step-by-Step Refactoring Instructions

**Remember to switch driver/navigator roles now!**

**Step A: Create the folder structure**

Create three new folders in your project:

```bash
mkdir controllers
mkdir models
mkdir routes
```

**What does this do?** 
- Creates empty folders for organizing your code

---

**Step B: Move and Rename - Create `models/tourModel.js`**

1. Open `tourLib.js` (the file with the data/array of tours)
2. Copy ALL the contents
3. Create a new file: `models/tourModel.js`
4. Paste the contents there

**The file should contain:**
- The tours array (your data)
- Functions to add/delete/find tours

**Example of what it might look like:**
```javascript
// models/tourModel.js

// This is our data
let tours = [
  { id: 1, name: "Beach Tour", price: 100 },
  { id: 2, name: "Mountain Tour", price: 150 }
];

// Function to get all tours
function getAllTours() {
  return tours;
}

// Function to add a new tour
function addTour(newTour) {
  tours.push(newTour);
  return newTour;
}

module.exports = { getAllTours, addTour, ... };
```

---

**Step C: Move and Rename - Create `controllers/tourControllers.js`**

1. Open `tourHandlers.js` (the file with the request handler functions)
2. Copy ALL the contents
3. Create a new file: `controllers/tourControllers.js`
4. Paste the contents there

**The file should contain:**
- `getAllTours(req, res)` function
- `getTourById(req, res)` function
- `createTour(req, res)` function
- `updateTour(req, res)` function
- `deleteTour(req, res)` function

**But make one change:** At the top, instead of importing from `tourLib.js`, import from `tourModel.js`:

```javascript
// OLD:
const { getAllTours, addTour } = require('../tourLib.js');

// NEW:
const Tour = require('../models/tourModel.js');
```

---

**Step D: Create `routes/tourRouter.js`**

This file tells your app which URLs map to which functions. Create a new file: `routes/tourRouter.js`

**Add this code:**

```javascript
// routes/tourRouter.js

const express = require('express');
const router = express.Router();

// Import the controller functions
const {
  getAllTours,
  getTourById,
  createTour,
  updateTour,
  deleteTour
} = require('../controllers/tourControllers.js');

// Define routes
router.get('/', getAllTours);           // GET /tours
router.get('/:tourId', getTourById);    // GET /tours/123
router.post('/', createTour);           // POST /tours
router.put('/:tourId', updateTour);     // PUT /tours/123
router.delete('/:tourId', deleteTour);  // DELETE /tours/123

module.exports = router;
```

**What does this do?**
- Creates a router (organizer) for all tour routes
- Maps each URL to the correct handler function

---

**Step E: Update `app.js`**

Now update your main server file to use the router.

**OLD `app.js` probably looks like:**
```javascript
const express = require('express');
const app = express();

const { getAllTours, getTourById, ... } = require('./tourHandlers.js');

app.use(express.json());

app.get('/tours', getAllTours);
app.get('/tours/:tourId', getTourById);
app.post('/tours', createTour);
app.put('/tours/:tourId', updateTour);
app.delete('/tours/:tourId', deleteTour);

const port = 4000;
app.listen(port, () => console.log(`Server on port ${port}`));
```

**NEW `app.js` should look like:**
```javascript
const express = require('express');
const app = express();

// Import the router (instead of individual functions)
const tourRouter = require('./routes/tourRouter.js');

app.use(express.json());

// Use the router for all /tours routes
app.use('/tours', tourRouter);

const port = 4000;
app.listen(port, () => console.log(`Server on port ${port}`));
```

**What changed?**
- Removed individual function imports
- Imported the router instead
- Used `app.use('/tours', tourRouter)` to attach the router

---

### 1.4 Test That Everything Still Works

This is crucial! After reorganizing, everything should still work exactly the same as before.

**Test in Postman:**
- `GET http://localhost:4000/tours` - Should return all tours
- `POST http://localhost:4000/tours` - Should create a new tour
- `GET http://localhost:4000/tours/1` - Should get tour with ID 1
- `DELETE http://localhost:4000/tours/1` - Should delete the tour

**If something doesn't work:**
- Check that all import paths are correct
- Check that all files are in the right folders
- Check the console for error messages
- Ask your partner (Navigator) to review the code

✅ **Success!** All endpoints should work exactly like before!

---

### 1.5 Save Your Progress to GitHub

Now commit your changes:

```bash
# Add all changes
git add .

# Save with a meaningful message
git commit -m "Refactor: Reorganize tours API into MVC pattern"

# Upload to GitHub
git push origin main
```

---

## ✅ Iteration 1 Complete!

You've successfully:
- ✅ Set up your project
- ✅ Organized your code into MVC pattern
- ✅ Tested that everything works
- ✅ Saved to GitHub

**Before continuing to Iteration 2:** Take a break and let your partner review what you did. Ask them: "Do you understand the structure? Is the code organized well?" Their feedback is important!

---

**When you're ready, let's move to Iteration 2: Update Status Codes & Implement User Routes**

---

## Step 2: Update Status Codes - Speaking the Language of the Web

**What Are Status Codes?**

When a website responds to a request, it sends back a **status code** (a 3-digit number) that tells you what happened:
- **200** = "OK, everything worked!"
- **201** = "Created! A new thing was made"
- **204** = "No Content - it worked, but I have nothing to show you"
- **404** = "Not Found - that thing doesn't exist"
- **500** = "Server Error - something broke"

Right now, your API probably just sends `200` for everything. That's not specific enough. Let's fix it!

---

### 2.1 Why Does This Matter?

Imagine a restaurant:
- Customer asks for a drink → Restaurant gives it → **Status: 200 (Here's your drink!)**
- Customer asks to delete their order → Restaurant deletes it → **Status: 204 (It's gone, nothing to show)**
- Customer places an order → Restaurant takes it → **Status: 201 (New order created!)**

Same results, but the **status codes tell the story**. This is called **RESTful API** - a professional way to build APIs.

---

### 2.2 Update Your Controller Functions

**Remember to switch driver/navigator roles now!**

Open `controllers/tourControllers.js` and make two changes:

---

#### Change 1: Fix `createTour()` function

**FIND THIS CODE:**
```javascript
function createTour(req, res) {
  const newTour = req.body;
  tours.push(newTour);
  res.json(newTour);  // ← This line
}
```

**CHANGE IT TO:**
```javascript
function createTour(req, res) {
  const newTour = req.body;
  tours.push(newTour);
  res.status(201).json(newTour);  // ← Changed: added .status(201)
}
```

**What does `.status(201)` mean?**
- `201` = "Created" - tells the client a new resource was made
- It's more accurate than `200`

---

#### Change 2: Fix `deleteTour()` function

**FIND THIS CODE:**
```javascript
function deleteTour(req, res) {
  const id = req.params.tourId;
  tours = tours.filter(tour => tour.id != id);
  res.json({ message: "Deleted successfully" });  // ← This line
}
```

**CHANGE IT TO:**
```javascript
function deleteTour(req, res) {
  const id = req.params.tourId;
  tours = tours.filter(tour => tour.id != id);
  res.status(204).send();  // ← Changed: added .status(204) and .send()
}
```

**What does `.status(204)` mean?**
- `204` = "No Content" - tells the client the delete worked, but there's no body to return
- `.send()` instead of `.json()` because we're not sending data back

---

### 2.3 Test Your Changes

Open **Postman** and test:

**Test 1: Create (should return 201)**
```
POST http://localhost:4000/tours
Body: { "name": "City Tour", "price": 50 }

Expected: Status shows 201, and you see the new tour data
```

**Test 2: Delete (should return 204)**
```
DELETE http://localhost:4000/tours/1

Expected: Status shows 204, and there's NO response body (empty)
```

**Test 3: Other endpoints (should still return 200)**
```
GET http://localhost:4000/tours

Expected: Status shows 200, and you see the tours list
```

✅ **Success!** You've updated your status codes!

---

### 2.4 Save to GitHub

```bash
git add .
git commit -m "Update: Add correct HTTP status codes (201 for POST, 204 for DELETE)"
git push origin main
```

---

---

## Step 3: Add Users - Building a Second Feature

Now your API only handles tours. Let's add users! This is the same pattern as tours, just repeated for a new entity.

**🔄 Remember to switch driver/navigator roles now!**

---

### 3.1 Create `models/userModel.js`

This file will store and manage user data (similar to `tourModel.js`).

**Create a new file:** `models/userModel.js`

**Add this code:**

```javascript
// models/userModel.js

// Sample user data
let users = [
  {
    id: 1,
    name: "Matti Seppänen",
    email: "matti@example.com",
    password: "M@45mtg$",
    phone_number: "+358401234567",
    gender: "Male",
    date_of_birth: "2000-01-15",
    membership_status: "Active"
  },
  {
    id: 2,
    name: "Anna Korhonen",
    email: "anna@example.com",
    password: "An#98xyz!",
    phone_number: "+358401234568",
    gender: "Female",
    date_of_birth: "1998-05-22",
    membership_status: "Active"
  }
];

// Get all users
function getAllUsers() {
  return users;
}

// Get one user by ID
function getUserById(id) {
  return users.find(user => user.id == id);
}

// Create a new user
function createUser(newUser) {
  // Auto-generate ID (take the highest ID and add 1)
  const maxId = Math.max(...users.map(u => u.id), 0);
  newUser.id = maxId + 1;
  users.push(newUser);
  return newUser;
}

// Update a user
function updateUser(id, updatedData) {
  const userIndex = users.findIndex(user => user.id == id);
  if (userIndex === -1) return null;
  
  users[userIndex] = { ...users[userIndex], ...updatedData };
  return users[userIndex];
}

// Delete a user
function deleteUser(id) {
  users = users.filter(user => user.id != id);
}

module.exports = {
  getAllUsers,
  getUserById,
  createUser,
  updateUser,
  deleteUser
};
```

**What does this file do?**
- Stores user data in an array
- Provides functions to get, create, update, and delete users
- It's basically a mini-database for users

---

### 3.2 Create `controllers/userControllers.js`

This file will handle user requests (similar to `tourControllers.js`).

**Create a new file:** `controllers/userControllers.js`

**Add this code:**

```javascript
// controllers/userControllers.js

// Import the user data functions
const {
  getAllUsers,
  getUserById,
  createUser,
  updateUser,
  deleteUser
} = require('../models/userModel');

// Get all users
function getAllUsersHandler(req, res) {
  const users = getAllUsers();
  res.json(users);
}

// Get one user by ID
function getUserByIdHandler(req, res) {
  const id = req.params.userId;
  const user = getUserById(id);
  
  if (!user) {
    return res.status(404).json({ message: "User not found" });
  }
  
  res.json(user);
}

// Create a new user
function createUserHandler(req, res) {
  const newUser = req.body;
  const user = createUser(newUser);
  res.status(201).json(user);
}

// Update a user
function updateUserHandler(req, res) {
  const id = req.params.userId;
  const updatedData = req.body;
  const user = updateUser(id, updatedData);
  
  if (!user) {
    return res.status(404).json({ message: "User not found" });
  }
  
  res.json(user);
}

// Delete a user
function deleteUserHandler(req, res) {
  const id = req.params.userId;
  deleteUser(id);
  res.status(204).send();
}

module.exports = {
  getAllUsersHandler,
  getUserByIdHandler,
  createUserHandler,
  updateUserHandler,
  deleteUserHandler
};
```

**What does this file do?**
- Handles HTTP requests for users
- Calls functions from `userModel.js` to get/create/update/delete data
- Sends responses back to the client

---

### 3.3 Create `routes/userRouter.js`

This file maps URLs to the user controller functions (similar to `tourRouter.js`).

**Create a new file:** `routes/userRouter.js`

**Add this code:**

```javascript
// routes/userRouter.js

const express = require('express');
const router = express.Router();

const {
  getAllUsersHandler,
  getUserByIdHandler,
  createUserHandler,
  updateUserHandler,
  deleteUserHandler
} = require('../controllers/userControllers');

// Define routes
router.get('/', getAllUsersHandler);             // GET /users
router.get('/:userId', getUserByIdHandler);     // GET /users/1
router.post('/', createUserHandler);            // POST /users
router.put('/:userId', updateUserHandler);      // PUT /users/1
router.delete('/:userId', deleteUserHandler);   // DELETE /users/1

module.exports = router;
```

**What does this file do?**
- Maps each URL to the correct controller function
- When someone hits `/users`, it calls `getAllUsersHandler`
- When someone hits `/users/1`, it calls `getUserByIdHandler`
- Etc.

---

### 3.4 Update `app.js` to Use the User Router

Now you need to tell your main app to use the user router.

**FIND THIS CODE in `app.js`:**
```javascript
const tourRouter = require('./routes/tourRouter.js');

app.use(express.json());

app.use('/tours', tourRouter);
```

**ADD THIS AFTER IT:**
```javascript
const tourRouter = require('./routes/tourRouter.js');
const userRouter = require('./routes/userRouter.js');  // ← Add this line

app.use(express.json());

app.use('/tours', tourRouter);
app.use('/users', userRouter);  // ← Add this line
```

**What does this do?**
- Imports the user router
- Tells the app: "When someone hits `/users/*`, use the userRouter"

---

### 3.5 Test the User Endpoints

Open **Postman** and test all user endpoints:

**Test 1: Get all users**
```
GET http://localhost:4000/users

Expected: 200 status, see list of users
```

**Test 2: Get one user**
```
GET http://localhost:4000/users/1

Expected: 200 status, see Matti's data
```

**Test 3: Create a new user**
```
POST http://localhost:4000/users

Body (JSON):
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "Joh#2024pwd",
  "phone_number": "+358401234569",
  "gender": "Male",
  "date_of_birth": "1990-03-15",
  "membership_status": "Active"
}

Expected: 201 status, see new user with auto-generated ID
```

**Test 4: Update a user**
```
PUT http://localhost:4000/users/1

Body (JSON):
{
  "name": "Matti Updated"
}

Expected: 200 status, see updated user
```

**Test 5: Delete a user**
```
DELETE http://localhost:4000/users/1

Expected: 204 status, no response body
```

✅ **Success!** Your API now handles both tours AND users!

---

### 3.6 Save to GitHub

```bash
git add .
git commit -m "Feature: Add User model, controller, and router with full CRUD operations"
git push origin main
```

---

## ✅ Iteration 2 Complete!

You've successfully:
- ✅ Updated HTTP status codes
- ✅ Created user model with sample data
- ✅ Created user controllers with handler functions
- ✅ Created user router with all routes
- ✅ Integrated users into the main app
- ✅ Tested all user endpoints
- ✅ Saved to GitHub

**Take a moment:** Ask your partner to review what you built. Did they understand the pattern? Can they see how users mirror the tours structure?

---

**When you're ready, we'll continue to Iteration 3: Adding `/api` prefix to routes & Implementing Morgan logging**

---

## Step 4: Add `/api` Prefix - Professional API Organization

**What is the `/api` prefix?**

Right now your URLs look like:
- `http://localhost:4000/tours`
- `http://localhost:4000/users`

But professional APIs look like:
- `http://localhost:4000/api/tours`
- `http://localhost:4000/api/users`

The `/api` prefix tells clients: "This is the application programming interface (API) - a way to access data programmatically, not for web pages."

---

### 4.1 Why Add `/api`?

**Imagine a website:**
- `http://example.com/` = Website home page
- `http://example.com/about` = Website about page
- `http://example.com/api/tours` = API to get tour data

By using `/api`, you separate **web pages** from **data endpoints**. This is a professional practice.

---

### 4.2 Update `app.js`

**🔄 Remember to switch driver/navigator roles now!**

**FIND THIS CODE in `app.js`:**
```javascript
const express = require('express');
const app = express();

const tourRouter = require('./routes/tourRouter.js');
const userRouter = require('./routes/userRouter.js');

app.use(express.json());

app.use('/tours', tourRouter);
app.use('/users', userRouter);
```

**CHANGE IT TO:**
```javascript
const express = require('express');
const app = express();

const tourRouter = require('./routes/tourRouter.js');
const userRouter = require('./routes/userRouter.js');

app.use(express.json());

app.use('/api/tours', tourRouter);  // ← Changed: added /api
app.use('/api/users', userRouter);  // ← Changed: added /api
```

**What changed?**
- Added `/api` before each route
- Now all requests must go through `/api/` prefix

---

### 4.3 Test the New URLs

Open **Postman** and test with the new URLs:

**Old URL (should NOT work now):**
```
GET http://localhost:4000/tours

Expected: 404 error - not found!
```

**New URL (should work):**
```
GET http://localhost:4000/api/tours

Expected: 200 status, see list of tours
```

**Test all endpoints with `/api` prefix:**
```
GET http://localhost:4000/api/tours
GET http://localhost:4000/api/tours/1
POST http://localhost:4000/api/users
GET http://localhost:4000/api/users/1
DELETE http://localhost:4000/api/tours/1
```

✅ **Success!** All your endpoints now have the professional `/api` prefix!

---

### 4.4 Save to GitHub

```bash
git add .
git commit -m "Refactor: Add /api prefix to all routes for professional API organization"
git push origin main
```

---

---

## Step 5: Add Morgan Logging - Seeing What's Happening

**What is Morgan?**

Morgan is a **logging middleware** - a tool that watches your API and prints out every request/response. It's like having a security camera for your API!

**Why do you need it?**

Imagine you're running a restaurant. You want to know:
- What orders came in?
- When did they arrive?
- How long did they take?
- Did any orders fail?

Morgan does the same for your API!

---

### 5.1 Install Morgan

**🔄 Remember to switch driver/navigator roles now!**

Open your terminal and run:

```bash
npm install morgan
```

**What does this do?**
- Downloads the Morgan package from npm
- Adds it to your `package.json`
- Now you can use it in your code

---

### 5.2 Add Morgan to `app.js`

**FIND THIS CODE in `app.js`:**
```javascript
const express = require('express');
const app = express();

const tourRouter = require('./routes/tourRouter.js');
const userRouter = require('./routes/userRouter.js');

app.use(express.json());

app.use('/api/tours', tourRouter);
app.use('/api/users', userRouter);
```

**CHANGE IT TO:**
```javascript
const express = require('express');
const morgan = require('morgan');  // ← Add this line
const app = express();

const tourRouter = require('./routes/tourRouter.js');
const userRouter = require('./routes/userRouter.js');

app.use(express.json());
app.use(morgan('tiny'));  // ← Add this line (BEFORE your routes!)

app.use('/api/tours', tourRouter);
app.use('/api/users', userRouter);
```

**Important:** Add Morgan BEFORE your routes. This way it logs every request.

---

### 5.3 Understanding Morgan Formats

Morgan has different output formats. Here are the main ones:

**Format: `tiny`** (what we're using)
```
GET /api/tours 200 45B - 5.432 ms
```
- Super compact
- Only shows the essentials

**Format: `short`**
```
GET /api/tours 200 - 5.432 ms
```
- Still compact
- Slightly more info

**Format: `dev`** (best for development)
```
GET /api/tours 200 5.432 ms - 45B
```
- Colored output (green for success, red for errors)
- Easiest to read

**Format: Custom**
```javascript
app.use(morgan(':method :url :status :res[content-length] - :response-time ms'));
```
- You can create custom formats

---

### 5.4 Test Morgan Logging

Start your server:
```bash
npm run dev
```

Open **Postman** and make some requests:

```
GET http://localhost:4000/api/tours
POST http://localhost:4000/api/users
DELETE http://localhost:4000/api/tours/1
```

**Look at your terminal.** You should see output like:
```
GET /api/tours 200 - 2.345 ms
POST /api/users 201 - 3.567 ms
DELETE /api/tours/1 204 - 1.234 ms
```

**What does each part mean?**
- `GET` = The HTTP method
- `/api/tours` = The URL path
- `200` = The status code
- `-` = Response size (not shown in tiny format)
- `2.345 ms` = How long the request took

✅ **Success!** You can now see every request your API handles!

---

### 5.5 Try Different Morgan Formats

**Optional:** Change the format to see the difference

**Try `dev` format (my recommendation for development):**

**Change this line:**
```javascript
app.use(morgan('tiny'));
```

**To this:**
```javascript
app.use(morgan('dev'));
```

Restart your server and make more requests. The output will be **colored** and easier to read!

---

### 5.6 Save to GitHub

```bash
git add .
git commit -m "Feature: Add Morgan middleware for HTTP request logging"
git push origin main
```

---

---

## Step 6: Add Authentication Middleware - Protecting Routes

**What is Authentication?**

Authentication means checking "Who are you?" before letting them do something.

**Example:**
- At a restaurant: "Only staff can access the kitchen"
- At a bank: "Only account owners can see their balance"
- At your API: "Only admins can delete tours"

**Middleware for Authentication:**

A middleware is like a **security guard** at a door. Before letting a request through, the guard checks:
- Do you have permission? ✅ Let them pass
- No permission? ❌ Stop them

---

### 6.1 Create `middleware/auth.js`

**🔄 Remember to switch driver/navigator roles now!**

First, create a new folder:
```bash
mkdir middleware
```

Now create the file: `middleware/auth.js`

**Add this code:**

```javascript
// middleware/auth.js

// This middleware checks if the user is an admin
function auth(req, res, next) {
  // Check if admin query parameter is "true"
  const isAdmin = req.query.admin === 'true';
  
  if (isAdmin) {
    // Admin access granted - let them through to the next handler
    next();
  } else {
    // Not admin - send 403 Forbidden
    res.status(403).json({ 
      message: "Access denied. You need admin privileges." 
    });
  }
}

module.exports = auth;
```

**What does this code do?**
- Checks the `admin` query parameter (e.g., `?admin=true`)
- If it's `true`, calls `next()` to let the request pass through
- If not, returns status `403 Forbidden` with an error message

---

### 6.2 Understand How Middleware Works

**Without middleware:**
```
Request → Controller → Response
```

**With middleware:**
```
Request → Middleware (security check) → Controller → Response
```

**The middleware can:**
- ✅ Let the request pass (call `next()`)
- ❌ Block the request (send a response)

---

### 6.3 Add Auth to Your Routes

You want to **protect some routes** but allow others:

**Allow (no auth needed):**
- `GET /api/tours` - Anyone can view tours
- `GET /api/users` - Anyone can view users

**Protect (admin only):**
- `POST /api/tours` - Only admins can create tours
- `PUT /api/tours/:id` - Only admins can update tours
- `DELETE /api/tours/:id` - Only admins can delete tours
- `POST /api/users` - Only admins can create users
- `PUT /api/users/:id` - Only admins can update users
- `DELETE /api/users/:id` - Only admins can delete users

---

### 6.4 Update `routes/tourRouter.js`

**FIND THIS CODE:**
```javascript
const express = require('express');
const router = express.Router();

const {
  getAllTours,
  getTourById,
  createTour,
  updateTour,
  deleteTour
} = require('../controllers/tourControllers.js');

// Define routes
router.get('/', getAllTours);
router.get('/:tourId', getTourById);
router.post('/', createTour);
router.put('/:tourId', updateTour);
router.delete('/:tourId', deleteTour);

module.exports = router;
```

**CHANGE IT TO:**
```javascript
const express = require('express');
const router = express.Router();
const auth = require('../middleware/auth');  // ← Add this line

const {
  getAllTours,
  getTourById,
  createTour,
  updateTour,
  deleteTour
} = require('../controllers/tourControllers.js');

// Public routes - no auth needed
router.get('/', getAllTours);
router.get('/:tourId', getTourById);

// Protected routes - auth middleware applied
router.post('/', auth, createTour);           // ← Add auth
router.put('/:tourId', auth, updateTour);     // ← Add auth
router.delete('/:tourId', auth, deleteTour);  // ← Add auth

module.exports = router;
```

**What changed?**
- Imported the auth middleware
- Added `auth` parameter to POST, PUT, DELETE routes
- Now these routes require `?admin=true` to work

---

### 6.5 Update `routes/userRouter.js`

**Do the same thing for users:**

**FIND THIS CODE:**
```javascript
const express = require('express');
const router = express.Router();

const {
  getAllUsersHandler,
  getUserByIdHandler,
  createUserHandler,
  updateUserHandler,
  deleteUserHandler
} = require('../controllers/userControllers');

// Define routes
router.get('/', getAllUsersHandler);
router.get('/:userId', getUserByIdHandler);
router.post('/', createUserHandler);
router.put('/:userId', updateUserHandler);
router.delete('/:userId', deleteUserHandler);

module.exports = router;
```

**CHANGE IT TO:**
```javascript
const express = require('express');
const router = express.Router();
const auth = require('../middleware/auth');  // ← Add this line

const {
  getAllUsersHandler,
  getUserByIdHandler,
  createUserHandler,
  updateUserHandler,
  deleteUserHandler
} = require('../controllers/userControllers');

// Public routes - no auth needed
router.get('/', getAllUsersHandler);
router.get('/:userId', getUserByIdHandler);

// Protected routes - auth middleware applied
router.post('/', auth, createUserHandler);        // ← Add auth
router.put('/:userId', auth, updateUserHandler);  // ← Add auth
router.delete('/:userId', auth, deleteUserHandler);  // ← Add auth

module.exports = router;
```

---

### 6.6 Test Authentication

Open **Postman** and test both scenarios:

**Test 1: Try DELETE without admin (should fail)**
```
DELETE http://localhost:4000/api/tours/1

Expected: 403 Forbidden status
Response: { "message": "Access denied. You need admin privileges." }
```

**Test 2: Try DELETE with admin=true (should succeed)**
```
DELETE http://localhost:4000/api/tours/1?admin=true

Expected: 204 status (success!)
```

**Test 3: Try GET without admin (should work - public route)**
```
GET http://localhost:4000/api/tours

Expected: 200 status, see all tours
```

**Test all protected routes with and without `?admin=true`:**
- ❌ `POST /api/tours` → should fail
- ✅ `POST /api/tours?admin=true` → should work
- ❌ `POST /api/users` → should fail
- ✅ `POST /api/users?admin=true` → should work

✅ **Success!** Your API is now protected with authentication!

---

### 6.7 Save to GitHub

```bash
git add .
git commit -m "Security: Add authentication middleware to protect admin routes"
git push origin main
```

---

## ✅ Iteration 3 Complete!

You've successfully:
- ✅ Added `/api` prefix to routes
- ✅ Installed Morgan logging middleware
- ✅ Added Morgan to `app.js`
- ✅ Tested logging with Postman
- ✅ Created authentication middleware
- ✅ Protected admin routes with auth
- ✅ Tested protected routes
- ✅ Saved to GitHub

---

## ✅ All Iterations Complete!

Congratulations! Your API now has:

**Iteration 1:**
- ✅ MVC pattern (organized code)
- ✅ Proper folder structure

**Iteration 2:**
- ✅ Correct HTTP status codes
- ✅ User feature (second entity)

**Iteration 3:**
- ✅ Professional `/api` prefix
- ✅ Morgan logging (watch requests)
- ✅ Authentication middleware (protect routes)

---

## Final Step: Team Synchronization

Make sure both team members have the same code on GitHub.

```bash
# Pull latest from GitHub
git pull origin main

# Make sure everything is saved
git add .
git commit -m "Final: All iterations complete and synced"
git push origin main
```

---

## Reference Links

- [Express Middleware Guide](https://blog.webdevsimplified.com/2019-12/express-middleware-in-depth/)
- [HTTP Status Codes Explained](https://blog.webdevsimplified.com/2022-12/http-status-codes/)
- [Morgan Logging Package](https://www.npmjs.com/package/morgan)