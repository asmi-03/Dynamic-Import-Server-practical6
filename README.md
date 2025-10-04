# Dynamic-Import-Server-practical6
This is a Node.js server that demonstrates dynamic imports and top-level await using ES modules. It serves HTML and JSON dynamically based on the route.
1️⃣ Prerequisites
Node.js v14+ (ES modules & top-level await support)
"type": "module" in package.json
{
  "type": "module",
  "scripts": {
    "start": "node server.mjs",
    "dev": "nodemon server.mjs"
  }
}
2️⃣ Project Structure
graphql
project/
│
├─ server.mjs        # Main server file
├─ index7.html       # HTML page served dynamically
├─ modules/
│   └─ about7.js     # Custom module for /about route
└─ package.json
3️⃣ server.mjs Explained
import http from 'http';
Import the built-in HTTP module to create a server.

const server = http.createServer(async (req, res) => {
Create a server using http.createServer.

Use async because we will use top-level await and dynamic imports inside.
  try {
    if (req.url === '/' || req.url === '/index') {
      const { readFile } = await import('fs/promises');
      const data = await readFile('./index7.html', 'utf8');
      res.writeHead(200, { 'Content-Type': 'text/html' });
      res.end(data);
    }
For the home page (/ or /index):
Dynamically import fs/promises on demand.
Read index7.html asynchronously.
Set response header to text/html and send the file content.
    else if (req.url === '/about') {
      const { getAboutInfo } = await import('./modules/about7.js');
      res.writeHead(200, { 'Content-Type': 'application/json' });
      res.end(JSON.stringify({ message: getAboutInfo() }));
    }
For the /about page:
Dynamically import a custom module about7.js.
Call getAboutInfo() function.
Respond with JSON.
    else {
      res.writeHead(404, { 'Content-Type': 'text/plain' });
      res.end('404 Not Found');
    }
  } catch (err) {
    res.writeHead(500, { 'Content-Type': 'text/plain' });
    res.end('Server Error: ' + err.message);
  }
});
Handles unknown routes with 404.
Wrap everything in try/catch to handle errors gracefully and respond with 500 if needed.
server.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
Start the server on port 3000.

Log a message to the console once it’s running.

4️⃣ index7.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Dynamic Import Server</title>
</head>
<body>
 <h1>Hello from Top-Level Await + Dynamic Import!</h1>
</body>
</html>
Simple HTML page served for the home route.

Demonstrates dynamic file serving.

5️⃣ modules/about7.js
export function getAboutInfo() {
  return "This is the About page info.";
}
Custom module dynamically imported.

Exports a function to provide JSON data for /about.

6️⃣ Run the Server
npm run start
Open a browser:
http://localhost:3000/ → Home page (HTML)
http://localhost:3000/about → About page (JSON)

Any other route → 404 Not Found

