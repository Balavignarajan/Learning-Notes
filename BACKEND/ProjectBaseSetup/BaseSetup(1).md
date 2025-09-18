_Now, let’s start your first Express server (super tiny — like “Hello World” for backend). We’ll do this step by step._

- Step 1: Initialize a project :
  **npm init -y** - 👉 This creates a package.json (like a project ID card).

- Step 2: Install Express :
  **npm install express**

- Step 3: Create your first server file :
  Make a new file: create or
  **touch index.js** >this will create a index file

- Step 4: Open index.js and add this code:
  const express = require("express");
  const app = express();
  // basic route
  app.get("/", (req, res) => {
  res.send("Hello Bala! 🚀 Your first Express server is running.");
  });
  // start server
  app.listen(3000, () => {
  console.log("Server running on http://localhost:3000");
  });

-Step 5: Run the server :
**node index.js** - You should see:  
 -- Hello Bala! 🚀 Your first Express server is running.

-Step 6 : Use nodeMon for :

_Nodemon automatically restarts a Node.js application when file changes are detected, improving development efficiency._
**npm install --save-dev nodemon**
Add scripts to package.json (so you can run dev easily). Open package.json and under "scripts" add:
**
"dev": "nodemon index.js",
"start": "node index.js"
**
