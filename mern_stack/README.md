# MERN Web Stack Implementation (StegHub Assignment)

## Introduction

The **MERN stack** is made up of **MongoDB, Express.js, React.js, and Node.js** this is one of the most widely used JavaScript-based stacks for building modern web applications.  
I’ll walk you through how I built a simple but functional full-stack project with MERN, explaining the steps as a story rather than just a list of commands. Along the way, I’ll also highlight pitfalls and little issues I encountered (and how I solved them), because that’s usually where the real learning happens.

---

## Step 0: Preparing the Ground (Prerequisites)

Before starting, I made sure I had the right tools ready:

- **Node.js**: The engine that allows us to run JavaScript on the server.
- **npm**: Comes bundled with Node.js, and works like a package manager for our dependencies.
- **MongoDB**: The NoSQL database where our data will live.
- **Express.js**: The minimal framework to handle routes and APIs.
- **React.js**: The library we’ll use for building the frontend user interface.
- **Postman/Insomnia**: For testing backend APIs before connecting them to the frontend.

![Node JS installation and Verification](./images/nodejs%20installation.png)

Pitfall: When I first installed Node.js using `apt`, I got an older version. The fix was to install from the NodeSource repository to get Node 18.x, which worked fine.

---

## Step 1: Backend Configuration — The Brains of the Operation

Now that the environment was ready, it was time to build the backend. This is the "engine room" of our stack.

### 1. Installing Node.js

After updating the server, I installed Node.js and npm:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt-get install -y nodejs
```

Then I verified:

```bash
node -v
npm -v
```

![Server update](./images/update%20ubuntu.png)

![Server update and upgrade verification](./images/ubuntu%20upgrade.png)

![node and npm version](./images/nodejs%20installation%20verification.png)

---

### 2. Application Code Setup

I created a new project folder for the backend, initialized npm, and got a `package.json` file to manage dependencies:

```bash
npm init -y
```

---

### 3. Installing Express.js

Express was next. It makes routing much easier:

```bash
npm install express
```

Then I created an entry file (`index.js`) with a barebones server:

![Index.js file creation](./images/index.js%20file%20creation.png)

```js
const express = require('express');
const app = express();

const PORT = process.env.PORT || 5000;

app.get('/', (req, res) => {
  res.send('Welcome to StegHub MERN Backend');
});

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

Running it with:

```bash
node index.js
```

![Index running](./images/index.js%20file.png)

Pitfall: I forgot to open port `5000` in my security group/firewall rules at first — leading to “connection refused” errors until I allowed traffic through.

---

### 4. Routes

I then structured my project with a `/routes` folder. Routes are like the paths users and APIs take to interact with the backend.

---

### 5. Models

For data structure, I used **Mongoose** to create models.

Example (`models/User.js`):

```js
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, unique: true },
  password: { type: String, required: true },
});

module.exports = mongoose.model('User', userSchema);
```

![mongose installation](./images/mongoose%20installation.png)

---

### 6. MongoDB Database Connection

I connected the backend to MongoDB:

```js
const mongoose = require('mongoose');
require('dotenv').config();

mongoose
  .connect(process.env.MONGO_URI, {
    useNewUrlParser: true,
    useUnifiedTopology: true,
  })
  .then(() => console.log('MongoDB connected'))
  .catch((err) => console.error(err));
```

Pitfall: The app initially failed to connect. The issue was my MongoDB Atlas cluster not having my IP whitelisted. Fix: update the cluster’s network settings to `Allow access from anywhere`.

![allow access from anywhere](./images/allow%20access%20from%20anywhere.png)

---

### 7. Testing Backend APIs Without Frontend

Before React comes into the picture, I tested my backend with **Postman**:

- **POST /api/users** → Create a user
- **GET /api/users** → Fetch all users
- **DELETE /api/users/\:id** → Remove a user

![GET from POSTMAN](./images/GET%20data%20on%20postman.png)

![POST from POSTMAN](./images/POST%20data%20on%20postman.png)

![DELETE from POSTMAN](./images/DELETE%20data%20on%20postman.png)

This step saved me a lot of debugging later on.

---

## Step 2: Frontend Creation — Giving Life to the App

Once the backend was stable, I moved to the frontend.

### 1. Running a React App

In the project root, I created a React app:

```bash
npx create-react-app client
cd client
npm start
```

React booted up on `http://localhost:3000`.
![port 3000](./images/checking%20port%203000.png)

---

### 2. Creating React Components

I created a `/src/components` folder to organize UI parts.

Example component (`UserList.js`):

```jsx
import React from 'react';

function UserList({ users }) {
  return (
    <div>
      <h2>Users</h2>
      <ul>
        {users.map((user) => (
          <li key={user._id}>
            {user.name} - {user.email}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default UserList;
```

![TODO up and running on the server](./images/todo%20running%20on%20site.png)

---

## Conclusion

By the end of this journey, I successfully:

1. Built a backend using Node.js and Express.
2. Created models and connected them to MongoDB.
3. Tested the backend thoroughly using Postman.
4. Bootstrapped a React frontend and created reusable components.

⚡ Key Takeaways:

- Always test backend APIs before connecting them to the frontend.
- MongoDB Atlas requires correct IP/network settings, otherwise connection will fail.
- React’s default port (3000) often clashes with other running apps.

---
