# Day 35 – Multi-Stage Builds & Docker Hub

## Task 1: The Problem with Large Images

### Write a simple Go, Java, or Node.js app (even a "Hello World" is fine)

# Node.js Conversion Steps

This project was converted from a static HTML application into a Node.js application using Express.js.

## 1. Initialize Node.js Project

Create a Node.js project:

```bash
npm init -y
```

This creates:

```
package.json
```

---

## 2. Install Express.js

Install Express framework:

```bash
npm install express
```

This creates:

```
node_modules/
package-lock.json
```

---

## 3. Project Structure

After conversion, the project structure:

```
weather/
│
├── server.js
├── package.json
├── package-lock.json
│
└── public/
    └── index.html
```

---

## 4. Create server.js

Create a file named:

```
server.js
```

Add the following code:

```javascript
const express = require("express");

const app = express();

const PORT = 3000;


// Serve frontend files
app.use(express.static("public"));


// Weather API endpoint
app.get("/api/weather", async (req, res) => {

    try {

        const { lat, lon } = req.query;


        if (!lat || !lon) {
            return res.status(400).json({
                error: "Latitude and longitude are required"
            });
        }


        const url =
        `https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}&current=temperature_2m,relative_humidity_2m,wind_speed_10m`;


        const response = await fetch(url);

        const data = await response.json();


        res.json(data);


    } catch(error) {

        res.status(500).json({
            error: error.message
        });

    }

});


// Start server
app.listen(PORT, () => {

    console.log(`Weather app running on port ${PORT}`);

});
```

---

## 5. Update package.json

Add the start script:

```json
"scripts": {
  "start": "node server.js"
}
```

---

## 6. Move Frontend Files

Create a public folder:

```bash
mkdir public
```

Move the HTML file:

```bash
mv index.html public/
```

---

## 7. Run Node.js Application

Install dependencies:

```bash
npm install
```

Start the application:

```bash
npm start
```

Output:

```
Weather app running on port 3000
```

The application will run on:

```
http://localhost:3000
```

---

## API Endpoint

Node.js backend provides:

```
GET /api/weather
```

Example:

```
http://localhost:3000/api/weather?lat=28.61&lon=77.20
```

The server fetches weather data from Open-Meteo API and returns the response to the frontend.

<img width="1365" height="680" alt="image" src="https://github.com/user-attachments/assets/33aff0d6-ddbf-499a-af09-64537039bac1" />


